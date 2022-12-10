
# Docker #docker

-- Understanding how uid and gid work in Docker containers

## Docker

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

## Docker Security #security

[https://github.com/andrzejsydor/devops/tree/master/_security](https://github.com/andrzejsydor/devops/tree/master/_security)

[Notary](https://github.com/theupdateframework/notary)

[Trivy](https://github.com/aquasecurity/trivy)

[Clair](https://github.com/quay/clair)

[Snyk](https://snyk.io/product/container-vulnerability-management/)

## Anchore Engine

[https://hub.docker.com/r/anchore/anchore-engine/](https://hub.docker.com/r/anchore/anchore-engine/)

[https://docs.anchore.com/current/docs/engine/quickstart/](https://docs.anchore.com/current/docs/engine/quickstart/)

[https://docs.anchore.com/current/](https://docs.anchore.com/current/)


### Understanding how uid and gid work in Docker containers

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

# Links

[https://github.com/andrzejsydor/devops](https://github.com/andrzejsydor/devops)

[random Docker tips](https://csabapalfi.github.io/random-docker-tips/)
