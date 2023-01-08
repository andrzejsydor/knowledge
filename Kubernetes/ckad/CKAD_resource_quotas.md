# Resource Quotas

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: hive-resourcequota
  namespace: hive
spec:
  hard:
    requests.memory: 1Gi
    requests.cpu: "1"
    limits.memory: 2Gi
    limits.cpu: "2"
```
