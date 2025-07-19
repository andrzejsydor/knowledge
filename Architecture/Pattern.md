---
tags: [Pattern]
---

# #Pattern

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

# #Link 

[https://microservices.io/patterns/refactoring/strangler-application.html](https://microservices.io/patterns/refactoring/strangler-application.html)




[https://www.geeksforgeeks.org/strangler-pattern-in-micro-services-system-design/](https://www.geeksforgeeks.org/strangler-pattern-in-micro-services-system-design/)




[https://medium.com/@josesousa8/the-strangler-pattern-kill-legacy-like-a-boss-db3db41564ed](https://medium.com/@josesousa8/the-strangler-pattern-kill-legacy-like-a-boss-db3db41564ed)




[https://www.redhat.com/architect/pros-and-cons-strangler-architecture-pattern](https://www.redhat.com/architect/pros-and-cons-strangler-architecture-pattern)


