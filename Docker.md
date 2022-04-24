
# Docker #docker

-- Understanding how uid and gid work in Docker containers

## Docker Indefinitely

### Pseudo-TTY

```
docker run -it ubuntu bash
docker run -d -t ubuntu
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

