# Secret

```shell
echo text | base64
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-code
  namespace: dev
type: Opaque
data:
  code: dHJ1c3RubzEK
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  namespace: dev
  name: secret-keeper
spec:
  containers:
    - name: busybox
      image: busybox:latest
      command: ['sh', '-c', 'echo $SECRET_STUFF; sleep 3600']
      env:
      - name: SECRET_STUFF
        valueFrom:
          secretKeyRef:
            name: secret-code
            key: code
```
