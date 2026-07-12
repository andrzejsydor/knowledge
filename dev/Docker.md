---
tags:
  - Docker
  - Container
---

# Docker

---
- [Docker Basics](#docker-basics)
  - [Docker data-root](#docker-data-root)
- [Docker Internals](#docker-internals)
  - [Namespaces (isolation — what the process sees)](#namespaces-isolation--what-the-process-sees)
  - [cgroups (limits — what the process can use)](#cgroups-limits--what-the-process-can-use)
  - [Mounts & storage (overlayfs — what the process stores)](#mounts--storage-overlayfs--what-the-process-stores)
  - [Networking (veth + bridge + NAT)](#networking-veth--bridge--nat)
  - [Internals — links](#internals--links)
- [Docker Indefinitely](#docker-indefinitely)
  - [Pseudo-TTY](#pseudo-tty)
  - [looking up port mappings](#looking-up-port-mappings)
  - [container IPs](#container-ips)
  - [Never Ending Commands](#never-ending-commands)
  - [wait](#wait)
  - [Multiple Commands in Docker Run](#multiple-commands-in-docker-run)
- [Docker Security](#docker-security)
- [Copy files](#copy-files)
  - [Container -> Local Host](#container---local-host)
  - [Local Host -> Container](#local-host---container)
- [Dockerfile](#dockerfile)
  - [USER in Dockerfiles](#user-in-dockerfiles)
  - [Dockerfile Caching](#dockerfile-caching)
- [Link](#link)
---

## Docker Basics

### Docker data-root

```
--data-root is the path where persisted data such as images, volumes, and cluster state are stored. The default value is /var/lib/docker. To avoid any conflict with other daemons, set this parameter separately for each daemon.
```

[https://docs.docker.com/engine/reference/commandline/dockerd/](https://docs.docker.com/engine/reference/commandline/dockerd/)

then:
```
$ sudo systemctl restart docker.socket 
$ sudo systemctl restart docker
```

## Docker Internals

A container is not a VM — it is a regular Linux process isolated by **namespaces** (what the process can *see*), limited by **cgroups** (what it can *use*), running on a layered **overlay filesystem** (what it can *store*), wired into the host via **virtual networking**.

### Namespaces (isolation — what the process sees)

| Namespace | Isolates | Effect in a container |
|-----------|----------|------------------------|
| `pid` | Process IDs | Container's main process is PID 1; can't see host processes |
| `net` | Network stack | Own interfaces, IP, routing table, iptables |
| `mnt` | Mount points | Own filesystem tree (rootfs from image layers) |
| `uts` | Hostname, domain | `hostname` returns the container ID / `--hostname` value |
| `ipc` | SysV IPC, POSIX queues | No shared memory with host processes |
| `user` | UID/GID mapping | Root in container can map to unprivileged host UID (**off by default** — that's why UIDs match the host, see [UID and GID](#understanding-uid-and-gid-in-docker-containers)) |
| `cgroup` | Cgroup root view | Container sees only its own cgroup subtree |

```sh
# list namespaces on the host
lsns

# namespaces of a container's main process
docker inspect --format '{{.State.Pid}}' <containerId>
ls -l /proc/<pid>/ns

# enter a container's namespaces without docker exec
nsenter -t <pid> -n ip addr        # only net ns — e.g. debug networking of a distroless container
nsenter -t <pid> -m -u -i -n -p sh # "manual docker exec"

# weaken isolation on purpose (debugging)
docker run --pid=host --net=host --privileged ...
```

Note: `docker exec` = start a new process inside the existing namespaces of a container. `kubectl debug` / sidecars work the same way (pods share `net` and `ipc` namespaces).

### cgroups (limits — what the process can use)

Control groups meter and limit CPU, memory, IO, and PIDs per container. Modern distros use **cgroup v2** (unified hierarchy, default since Docker 20.10 on kernels ≥ 5.2 with systemd).

```sh
docker run -d --memory=512m --cpus=1.5 --pids-limit=256 nginx

# live usage per container (reads cgroup stats)
docker stats

# where the limits actually live (cgroup v2 + systemd driver)
cat /sys/fs/cgroup/system.slice/docker-<containerId>.scope/memory.max
cat /sys/fs/cgroup/system.slice/docker-<containerId>.scope/cpu.max
```

Gotchas:
- Exceeding `--memory` ⇒ the kernel **OOM-kills** the process (exit code 137). For the JVM make sure it is container-aware (`-XX:MaxRAMPercentage`, default since JDK 10; older JVMs saw the *host* memory).
- `--cpus` is enforced via CFS quota — the app sees all host cores in `/proc/cpuinfo`; use `Runtime.availableProcessors()` on a container-aware JDK.
- cgroups **limit**, namespaces **hide** — `free -m` in a container shows host memory unless something like LXCFS is used.

### Mounts & storage (overlayfs — what the process stores)

Image = ordered set of **read-only layers**. Container = those layers + one **writable layer** on top, merged by the `overlay2` driver (copy-on-write):

```
merged   ← what the container sees (mount at runtime)
upperdir ← container's writable layer (lost on container removal)
lowerdir ← image layers (read-only, shared between containers)
```

```sh
# see the overlay directories of a container
docker inspect --format '{{json .GraphDriver.Data}}' <containerId> | jq

# all of it lives under data-root (default /var/lib/docker)
ls /var/lib/docker/overlay2
```

- **Copy-on-write**: modifying a file from an image layer copies it up to `upperdir` first — cheap for small writes, slow for heavy IO ⇒ keep databases and hot data **out** of the container layer.
- Persistence options:

| Type | Flag | Use case |
|------|------|----------|
| Volume | `-v myvol:/data` | Persistent data, managed by Docker under data-root |
| Bind mount | `-v /host/path:/data` | Sharing host files (dev setups, config) |
| tmpfs | `--tmpfs /tmp` | Secrets/scratch in RAM, never touches disk |

- This is also why layer order matters for [Dockerfile Caching](#dockerfile-caching) — each instruction produces one immutable layer.

### Networking (veth + bridge + NAT)

Default `bridge` mode: each container gets its own `net` namespace connected to the host by a **veth pair** — one end inside the container (`eth0`), the other attached to the `docker0` bridge. Outbound traffic is masqueraded (SNAT), published ports are DNAT-ed by iptables rules that Docker manages.

```
container eth0 ── veth pair ── docker0 bridge ── iptables NAT ── host eth0
```

```sh
# the plumbing
ip link show type veth          # host side of the pairs
brctl show docker0              # or: ip link show master docker0
iptables -t nat -L DOCKER -n    # DNAT rules created by -p 8080:80

# per-network details, connected containers, subnet
docker network inspect bridge
```

Network drivers:

| Driver | What it does | When |
|--------|--------------|------|
| `bridge` | Private subnet (172.17.0.0/16 by default) + NAT | Default, single host |
| `host` | No `net` namespace — container shares host stack | Max performance, no port mapping possible |
| `none` | Only loopback | Manual/custom networking |
| `overlay` | VXLAN across hosts | Swarm / multi-host |
| `macvlan` | Container gets own MAC on the LAN | Container must look like a physical host |

- On **user-defined** bridge networks containers resolve each other **by name** via Docker's embedded DNS (`127.0.0.11` inside the container); the default bridge has no name resolution — one more reason to always `docker network create`.
- `-p 8080:80` works from outside, but two containers on the same bridge talk directly to `containerIP:80` — no NAT involved.

### Internals — links

[Docker overview — the underlying technology](https://docs.docker.com/get-started/overview/#the-underlying-technology)

[namespaces(7)](https://man7.org/linux/man-pages/man7/namespaces.7.html) / [cgroups(7)](https://man7.org/linux/man-pages/man7/cgroups.7.html)

[Docker storage drivers — overlay2](https://docs.docker.com/engine/storage/drivers/overlayfs-driver/)

[Docker networking overview](https://docs.docker.com/engine/network/)

[Containers from scratch (Liz Rice, live-coding a container in Go)](https://www.youtube.com/watch?v=8fi7uSYlOdc)

## Docker Indefinitely

### Pseudo-TTY

```
docker run -it ubuntu bash
docker run -d -t ubuntu
```

### looking up port mappings

`docker run` accepts explicit port mappings as parameters or you can specify -P to map all ports automatically. The latter has the advantage of preventing conflicts and looking up the assigned ports can be done as follows:
 
 ```
docker port <containerId> <portNumber>
# or
docker inspect --format '{{.NetworkSettings.Ports}}' <containerId>
```

### container IPs

Each container has it's IP in a private subnet (which is 172.17.0.0/16 by default). 
The IP can change with restart but can be looked up should you need it:

```
docker inspect --format '{{.NetworkSettings.IPAddress}}' <containerId>
```

### Never Ending Commands

```
# command keeps looking for new changes in the file to display. It never ends as long as the file exists
docker run ubuntu tail -f /dev/null

# infinite loop
docker run ubuntu while true; do sleep 1; done

# command keeps the container idle and does nothing
docker run ubuntu sleep infinity
```

### wait

```
	#!/bin/sh
	while ! nc -z config-server 8888 ; do
	    echo "Waiting for upcoming Config Server"
	    sleep 2
	done
	java -jar /opt/spring-cloud/lib/config-client.jar
```

### Multiple Commands in Docker Run

#### `docker run`

```
docker run centos:latest sh -c "whoami && date"
```

#### Using CMD/ENTRYPOINT in the Dockerfile

```
...
ENTRYPOINT ["sh", "-c", "whoami && date"]
```

## Docker Security

[https://github.com/andrzejsydor/devops/tree/master/_security](https://github.com/andrzejsydor/devops/tree/master/_security)

[Notary](https://github.com/theupdateframework/notary)

[Trivy](https://github.com/aquasecurity/trivy)

[Clair](https://github.com/quay/clair)

[Snyk](https://snyk.io/product/container-vulnerability-management/)

### Understanding UID and GID in Docker Containers

```
docker run -d ubuntu:latest sleep infinity
ps aux | grep sleep
```

```
echo $UID
```

```
FROM ubuntu:latest
RUN useradd -r -u 1001 -g appuser appuser
USER appuser
ENTRYPOINT [“sleep”, “infinity”]
```

```
docker run -d --user 1001 ubuntu:latest sleep infinity
ps aux | grep sleep
```

# Copy files

## Container -> Local Host

```
docker cp <containerId>:/file/path/in/container/file /host/local/path/file
```

## Local Host -> Container

```
docker cp /host/local/path/file <containerId>:/file/path/in/container/file
```

# Dockerfile

## USER in Dockerfiles

By default docker runs everything as root but you can use USER in Dockerfiles. There's no user namespacing in docker so the container sees the users on the host but only uids hence you need the add the users in the container.

## Dockerfile Caching

Use caching properly for your Dockerfiles: docker containers are built very quickly as long as you make use of the caching capability. A quick set of gotchas and advices:
- ADD & VOLUMES are cache invalidators.
- RUN commands are cached while unchanged.
- Dockerfile execution is sequential, hence: order matters and a changed step deprecate the next caches.
- Group your RUN commands (shell sequences) together per type (e.g. ssh related). This allows you to tune and forget a "feature" of your container and focus on the next one without ever blowing up your cache.
- Maintain common command orders in between your various Dockerfiles. This allow to use "common" caching from one container to another for as long as they share common features

# #Link

[Rancher Desktop](https://rancherdesktop.io/)

[Podman](https://podman.io/)

[https://github.com/andrzejsydor/devops](https://github.com/andrzejsydor/devops)

[Docker - copy files](https://mkyong.com/docker/how-to-copy-files-from-docker-container-to-host/)

[Docker - copy files](https://www.baeldung.com/ops/docker-copying-files)

[random Docker tips](https://csabapalfi.github.io/random-docker-tips/)

[Docker Labs Debug Tools](https://hub.docker.com/extensions/docker/labs-debug-tools-extension)
