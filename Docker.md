# Docker #docker

-- Understanding how uid and gid work in Docker containers

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
