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
