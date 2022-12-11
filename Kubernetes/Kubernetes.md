# Kubernetes

## tips

### Get IPs of all nodes

```
kubectl get nodes -o jsonpath=‘{.items[*].status.addresses[?(@.type==“ExternalIP”)].address}’
```

https://jsonpath.com/
https://support.smartbear.com/alertsite/docs/monitors/api/endpoint/jsonpath.html



## Tools

[OperatorHub.io](https://operatorhub.io/)

[Rancher](https://www.rancher.com/)

## other

### Why the double dash?

Note: The double dash (--) separates the arguments you want to pass to the command from the kubectl arguments.

https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/
