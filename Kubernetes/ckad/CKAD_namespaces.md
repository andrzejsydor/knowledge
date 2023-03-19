# Namespaces

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
  labels:
    name: dev
```

```shell
kubectl create ns <name>
kubectl get ns <name> -o yaml
```
