---
tags:
  - Kubernetes
  - k8s
  - Helm
  - Istio
---

# Kubernetes

## tips

### Get IPs of all nodes

```
kubectl get nodes -o jsonpath=‘{.items[*].status.addresses[?(@.type==“ExternalIP”)].address}’
```

https://jsonpath.com/
https://support.smartbear.com/alertsite/docs/monitors/api/endpoint/jsonpath.html


## Certificate

Signing certificates through the CertificateSigningRequest resource

```
kubectl certificate approve <name of the CSR>
```

[Get the CertificateSigningRequest approved](https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/#get-the-certificate-signing-request-approved)

[PKI certificates and requirements](https://kubernetes.io/docs/setup/best-practices/certificates/)

## Tools

[OperatorHub.io](https://operatorhub.io/)

[Rancher](https://www.rancher.com/)

## other

### Why the double dash?

Note: The double dash (--) separates the arguments you want to pass to the command from the kubectl arguments.

https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/

### Troubleshooting services

[Monitoring, Logging, and Debugging](https://kubernetes.io/docs/tasks/debug/)

[Debug Pods](https://kubernetes.io/docs/tasks/debug/debug-application/debug-pods/)

[Debug Services](https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/)

### Session affinity

https://kubernetes.io/docs/reference/networking/virtual-ips/#session-affinity

https://stackoverflow.com/questions/58955409/how-to-use-session-affinity-on-requests-to-kubernetes-service

## Links

[Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)

[Storage Class](https://kubernetes.io/docs/concepts/storage/storage-classes/)

[Helm - The package manager for Kubernetes](https://helm.sh/)
