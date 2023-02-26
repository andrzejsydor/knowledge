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

## Tips

```
alias k=kubectl
```
