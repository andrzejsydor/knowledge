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
