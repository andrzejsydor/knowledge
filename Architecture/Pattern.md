---
tags: [Pattern]
---

# #Pattern

## Table of Contents

- [Singleton](#singleton)
- [Strangler application](#strangler-application)
- [Ambassador](#ambassador)

## Singleton

### Common use cases for the Singleton pattern include:

- Database connection pools that manage limited database connections efficiently
- Logger instances that centralize logging functionality across an application
- Configuration managers that store application-wide settings
- Cache managers that maintain shared data across multiple components
- Thread pools that manage worker threads for concurrent operations

### thread-safety

- [Bill Pugh Singleton](https://www.digitalocean.com/community/tutorials/java-singleton-design-pattern-best-practices-examples#5-bill-pugh-singleton-implementation) for most use cases
- [Double-Checked Locking](https://www.digitalocean.com/community/tutorials/java-singleton-design-pattern-best-practices-examples#4-thread-safe-singleton) for performance-critical lazy initialization
- [Enum](https://www.digitalocean.com/community/tutorials/java-singleton-design-pattern-best-practices-examples#7-enum-singleton) Singleton for simplicity and safety

## Strangler application

Problem: How do you migrate a legacy monolithic application to a microservice architecture?

## Ambassador

- The Ambassador Design Pattern places an intermediary (the “ambassador”) between clients and remote services to offload infrastructure concerns.
- It is especially valuable in microservice, cloud-native, and SOA architectures where network latency, failures, or security issues are common.
- Typical ambassador responsibilities include retry logic, timeout management, logging, tracing, metrics collection, and security (TLS, tokens).
- The provided Java example shows an UnreliableMathService wrapped by a MathServiceAmbassador that retries three times, logs attempts, and returns a fallback value.
- By isolating cross-cutting code, the pattern improves maintainability, promotes reuse across multiple clients, and standardizes security practices.
- Key benefits are enhanced resilience, better observability, cleaner business logic, and easier integration of legacy systems.
- *Drawbacks* include extra indirection, possible performance overhead, added deployment/maintenance effort, and tighter coupling to specific infra tools.

[https://dzone.com/articles/design-patterns-for-microservices-ambassador-anti](https://dzone.com/articles/design-patterns-for-microservices-ambassador-anti)

[https://learn.microsoft.com/en-us/azure/architecture/patterns/ambassador](https://learn.microsoft.com/en-us/azure/architecture/patterns/ambassador0)

[https://www.geeksforgeeks.org/system-design/ambassador-pattern-in-distributed-systems/](https://www.geeksforgeeks.org/system-design/ambassador-pattern-in-distributed-systems/)

# #Link 

[https://microservices.io/patterns/refactoring/strangler-application.html](https://microservices.io/patterns/refactoring/strangler-application.html)




[https://www.geeksforgeeks.org/strangler-pattern-in-micro-services-system-design/](https://www.geeksforgeeks.org/strangler-pattern-in-micro-services-system-design/)




[https://medium.com/@josesousa8/the-strangler-pattern-kill-legacy-like-a-boss-db3db41564ed](https://medium.com/@josesousa8/the-strangler-pattern-kill-legacy-like-a-boss-db3db41564ed)




[https://www.redhat.com/architect/pros-and-cons-strangler-architecture-pattern](https://www.redhat.com/architect/pros-and-cons-strangler-architecture-pattern)


