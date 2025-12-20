---
tags:
  - Architecture
  - CQRS
  - CQS
aliases:
  - CQS
  - CQRS
---

# CQRS

## Table of Contents

- [CQS vs CQRS](#cqs-vs-cqrs)
  - [CQS vs CQRS: Key Differences and Relationship](#cqs-vs-cqrs-key-differences-and-relationship)
  - [Comparison Table](#comparison-table)
- [CQS](#cqs)
- [CQRS](#cqrs)
- [Links](#links)
  - [Stackoverflow](#stackoverflow)
  - [PDF](#pdf)

## CQS vs CQRS

Command Query Separation (CQS) vs Command Query Responsibility Segregation (CQRS)

## CQS vs CQRS: Key Differences and Relationship

Command Query Separation (CQS) and Command Query Responsibility Segregation (CQRS) are related software design concepts, but they operate at different levels of abstraction and have distinct goals.

## Comparison Table

| Aspect | CQS | CQRS |
| :-- | :-- | :-- |
| **Level** | Method/Class (micro) | Architecture/System (macro) |
| **Separation** | Methods: read or write, not both | Entire subsystems: separate read and write models |
| **Data Store** | Single, unified | Can use separate stores for commands and queries |
| **Consistency** | Immediate (strong) | Often eventual (in distributed systems) |
| **Complexity** | Low | Higher (especially with event sourcing, multiple DBs) |
| **Use Case** | Any size system, code clarity | Large, complex, scalable, high-performance systems |

## Relationship

### CQS is the foundation for CQRS: CQRS extends CQS from the method level to the architectural level, applying the same separation of concerns but across the entire system.

### CQRS uses CQS principles: In CQRS, commands and queries are not just separated in code, but may be handled by different services, databases, or even teams.

## When to Use Each

### CQS: Use for clear, maintainable code in any application. It's a best practice for method design.

### CQRS: Use when your system needs to scale read and write operations independently, or when read and write workloads are very different. It's especially useful in distributed, high-scale, or event-sourced systems, but comes with added complexity

## CQS

## CQRS

CQRS - Command Query Responsibility Segregation

- Command, Command Handler, Query, Query Handler

### Benefits

- Clear boundaries between the system behaviour
- Closer to business logic
- Loose coupling
- Reduced cognitive load
- Easier scaling, optimisations and architectural changes
- Predictability

### Pros

- Scalability
- Flexibility
- Improved Maintainability
- Enhanced Security

### Cons

- Increased Complexity
- Learning Curve
- Eventual Consistency

# Links

[https://deviq.com/design-patterns/cqrs-pattern](https://deviq.com/design-patterns/cqrs-pattern)

[https://martinfowler.com/bliki/CQRS.html](https://martinfowler.com/bliki/CQRS.html)

[https://martinfowler.com/bliki/CommandQuerySeparation.html](https://martinfowler.com/bliki/CommandQuerySeparation.html)

[https://lab.scub.net/command-query-responsibility-segregation-cqrs-93e35d1929ec](https://lab.scub.net/command-query-responsibility-segregation-cqrs-93e35d1929ec)

[https://www.eventstore.com/cqrs-pattern](https://www.eventstore.com/cqrs-pattern)

### Stackoverflow

[https://stackoverflow.com/questions/34255490/difference-between-cqrs-and-cqs](https://stackoverflow.com/questions/34255490/difference-between-cqrs-and-cqs)

### PDF

[https://cqrs.wordpress.com/wp-content/uploads/2010/11/cqrs_documents.pdf](https://cqrs.wordpress.com/wp-content/uploads/2010/11/cqrs_documents.pdf)
