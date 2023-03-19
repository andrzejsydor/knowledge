# CKAD Tasks

- For all use ns: `ckad`
```
k create ns ckad
```

## Basic

### Basic 1

- run pod 'nginx', image: `nginx`, port, label
- create service 'nginx' expose `nginx`

```
k run nginx --image nginx -p 80 -l app=nginx -n demo

```

### Basic 2

- create yaml definition, pod: 'redis1', image: `redis`, export port: `6379`

```
k run redis1 --image=redis --port=6379 --dry-run=client -o yaml
```

### Basic 3

- create pod 'busybox', runs the command: `env`

```
k run busybox --image=busybox --restart=Never -- env
k logs busybox
```

### Basic 4 Alpine+Curl

```shell
k run al1 --image=alpine --rm -i --tty -- sh
k run al1 --image=alpine --rm -i -t -- sh

apk add curl
```

## Config Map

- create Config Map k1=v1, k2=v2

```shell
k create configmap --help
k create configmap cm1 --from-literal=k1=v1 --from-literal=k2=v2
```

```shell
k create po b1 --image=busybox -o yaml >> b1.yaml
```

```yaml

```

- create Config Map with file `haproxy.cfg`

-- create Ambassador container image `haproxy`

## Deployment

- create deployment of `nginx`, replicas 2

```shell
kubectl create deployment nx --image=nginx --port 80 --replicas=3 -o yaml
```

-- service (Type: NodePort)

-- `curl k8s-control:<node_port>`

## Volumes

- create PV `pv1`, PVC `pvc1`, pod `busybox` that write


## Network Policy

- create pod `nginx` and `busybox`

-- define Network Policy that allow network access between the pods

```
k get networkpolicy
k describe networkpolicy <name>
```

## Tips

```
alias k=kubectl
alias kaf="kubectl apply -f"
```
