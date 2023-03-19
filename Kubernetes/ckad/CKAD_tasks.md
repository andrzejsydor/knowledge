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

## Config Map

- create Config Map k1=v1, k2=v2

```shell
k create configmap --help
k create configmap cm1 --from-literal=k1=v1 --from-literal=k2=v2
```

- create Config Map with file `haproxy.cfg`

-- create Ambassador container image `haproxy`

## Deployment

- create deployment of `nginx`, replicas 2

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
