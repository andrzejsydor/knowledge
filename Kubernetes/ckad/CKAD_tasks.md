# CKAD Tasks


- For all use ns: `ckad`
```
k create ns ckad
```

- create pod 'nginx', image: `nginx`

```
k run nginx --image nginx -n demo
```

- create yaml definition, pod: 'redis1', image: `regis`, export port: `6379`

```
k run redis1 --image=redis --port=6379 --dry-run=client -o yaml
```

- create pod 'busybox', runs the command: `env`

```
k run busybox --image=busybox --restart=Never -- env
k logs busybox
```

- create deployment of `nginx`, replicas 2

-- service (Type: NodePort)

-- `curl k8s-control:<node_port>`



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
