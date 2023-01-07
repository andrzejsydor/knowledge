# Main

## [Namespaces](CKAD_namespaces.md)
https://kubernetes.io/docs/tasks/administer-cluster/namespaces-walkthrough/
```
kubectl get namespaces --show-labels
```
## [Secret](CKAD_secret.md)
https://kubernetes.io/docs/concepts/configuration/secret/#use-cases

```

```

## [Resources](CKAD_resources.md)
https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

```

```

# [Events](CKAD_events.md)

```
kubectl get events -n dev -o wide
```

# Lifecycle Events
https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/


## ?!
```
alias k=kubectl

kubectl config get-contexts
kubectl config use-context <context>

kubectl get pod <pod> -n <namespace>
```

# tips
```
--all-namespaces
```

# shorts
```
pod->po
```

# other

```
echo text | base64
```