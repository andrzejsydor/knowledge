# Kubernetes Services

## Exposing services to external clients

Exposing services to external clients:
- NodePort
- LoadBalancer
- Ingress resource

Type values and their behaviours are: 

- ClusterIP: Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default ServiceType.

- [NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport): Exposes the Service on each Node's IP at a static port (the NodePort). A ClusterIP Service, to which the NodePort Service routes, is automatically created. You'll be able to contact the NodePort Service, from outside the cluster, by requesting <NodeIP>:<NodePort>.

- [LoadBalancer](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer): Exposes the Service externally using a cloud provider's load balancer. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created.

- [ExternalName](https://kubernetes.io/docs/concepts/services-networking/service/#externalname): Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up.

## Links

https://kubernetes.io/docs/concepts/services-networking/service/

https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types
