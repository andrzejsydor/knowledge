---
tags:
 - Architecture
 - CQRS
 - CQS
aliases:
 - CQS
 - CQRS
---

# CQS vs CQRS

## Comparison Table

| Aspect | CQS | CQRS |
| :-- | :-- | :-- |
| **Level** | Method/Class (micro) | Architecture/System (macro) |
| **Separation** | Methods: read or write, not both | Entire subsystems: separate read and write models |
| **Data Store** | Single, unified | Can use separate stores for commands and queries |
| **Consistency** | Immediate (strong) | Often eventual (in distributed systems) |
| **Complexity** | Low | Higher (especially with event sourcing, multiple DBs) |
| **Use Case** | Any size system, code clarity | Large, complex, scalable, high-performance systems |


# #CQS

# #CQRS 

CQRS - Command Query Responsibility Segregation

- Command, Command Handler, Query, Query Handler




Benefits:

 - Clear boundaries between the system behaviour

 - Closer to business logic

 - Loose coupling

 - Reduced cognitive load

 - Easier scaling, optimisations and architectural changes

 - Predictability

PROS:

 - Scalability

 - Flexibility

 - Improved Maintability

 - Enhanced Security

CONS:

 - Increased Complexity

 - Learning Curve

 - Eventual Consistency






# #Link 




[https://deviq.com/design-patterns/cqrs-pattern](https://deviq.com/design-patterns/cqrs-pattern)




[https://martinfowler.com/bliki/CQRS.html](https://martinfowler.com/bliki/CQRS.html)




[https://martinfowler.com/bliki/CommandQuerySeparation.html](https://martinfowler.com/bliki/CommandQuerySeparation.html)






[https://lab.scub.net/command-query-responsibility-segregation-cqrs-93e35d1929ec](https://lab.scub.net/command-query-responsibility-segregation-cqrs-93e35d1929ec)




[https://www.eventstore.com/cqrs-pattern](https://www.eventstore.com/cqrs-pattern)






## #Stackoverflow




[https://stackoverflow.com/questions/34255490/difference-between-cqrs-and-cqs](https://stackoverflow.com/questions/34255490/difference-between-cqrs-and-cqs)






## #PDF




[https://cqrs.wordpress.com/wp-content/uploads/2010/11/cqrs_documents.pdf](https://cqrs.wordpress.com/wp-content/uploads/2010/11/cqrs_documents.pdf)

