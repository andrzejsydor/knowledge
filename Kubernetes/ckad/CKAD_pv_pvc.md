# Deployments

## PV & PVC
```shell
```

```shell
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-data
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: static
  hostPath:
    path: /var/pvdata
```


```shell
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-data-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 256Mi
  storageClassName: static
```

```shell
apiVersion: v1
kind: Pod
metadata:
  name: pv-pod
spec:
  containers:
  - name: busybox
    image: busybox:stable
    command: ['sh', '-c', 'while true; do sleep 3600; done']
    volumeMounts:
    - name: static-content
      mountPath: /var/data
  volumes:
  - name: static-content
    persistentVolumeClaim:
      claimName: pv-data-pvc
```
