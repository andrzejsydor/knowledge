# Resources

```
apiVersion: v1
kind: Pod
metadata:
  namespace: dev
  name: pod1
spec:
  containers:
    - name: nginx
      image: nginx:stable
      resources:
        requests:
          memory: 256Mi
          cpu: 250m
        limits:
          memory: 512Mi
          cpu: 500m
```
