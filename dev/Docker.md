---
tags:
  - Docker
  - Container
---

# Docker

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

## Anchore Engine

[https://hub.docker.com/r/anchore/anchore-engine/](https://hub.docker.com/r/anchore/anchore-engine/)

[https://docs.anchore.com/current/docs/engine/quickstart/](https://docs.anchore.com/current/docs/engine/quickstart/)

[https://docs.anchore.com/current/](https://docs.anchore.com/current/)


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
