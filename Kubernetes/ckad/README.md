# Main

## [Helm](./../Helm/Helm.md)

## [Namespaces](CKAD_namespaces.md)
https://kubernetes.io/docs/tasks/administer-cluster/namespaces-walkthrough/
```
kubectl get namespaces --show-labels
```

## [ConfigMap](CKAD_ConfigMap.md)
https://kubernetes.io/docs/concepts/configuration/configmap/

## [Secret](CKAD_secret.md)
https://kubernetes.io/docs/concepts/configuration/secret/#use-cases

```

```

## [Resources](CKAD_resources.md)
https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

```

```

## [Deployments](CKAD_deployments.md)
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

```
kubectl describe <deployment>
kubectl edit deployment <name>
kubectl get deployment <name>
```
## [Health Check](CKAD_health_check.md)

## [Resource Quotas](CKAD_resource_quotas.md)
https://kubernetes.io/docs/concepts/policy/resource-quotas/
```
kubectl get resourcequota
```
## [Events](CKAD_events.md)

```
kubectl get events -n dev -o wide
```

# Lifecycle Events
https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/


## ?!
```
kubectl config get-contexts
kubectl config use-context <context>

kubectl get pod <pod> -n <namespace>
```

# tips
```
--all-namespaces
kubectl get pod <pod> -o yaml > pod_name.yaml
kubectl logs <pod> -c <container> 
```

## Alias

`alias k=kubectl`

`alias kg="kubectl get"`

`alias kaf="kubectl apply -f"`

`export do="--dry-run=client -o yaml"`

## shorts
```
pod->po
namespaces->ns
replicaset->rs
deployment->deploy
service->svc
endpoints->ep
```

# other

```
echo text | base64
```
