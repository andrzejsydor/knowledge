# Deployments

## Scaling a Deployment [](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#scaling-a-deployment)


```shell
kubectl scale deployment/nginx-deployment --replicas=10
```


Assuming [horizontal Pod autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/) is enabled in your cluster:

```shell
kubectl autoscale deployment/nginx-deployment --min=10 --max=15 --cpu-percent=80
```


```
```

