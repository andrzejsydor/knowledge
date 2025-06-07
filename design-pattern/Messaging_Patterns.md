---
tags:
  - Patterns for Decoupling in Distributed Systems
  - Eventsourcing Patterns
  - Messaging Patterns
---

## Table of Contents

1. **Patterns for Decoupling in Distributed Systems**  

   1.1. [Domain Queries](https://verraes.net/2019/05/patterns-for-decoupling-distsys-domain-query/)  
   1.2. [Summary Event](https://verraes.net/2019/05/patterns-for-decoupling-distsys-summary-event/)  
   1.3. [Completeness Guarantee](https://verraes.net/2019/05/patterns-for-decoupling-distsys-completeness-guarantee/)  
   1.4. [Passage of Time Event](https://verraes.net/2019/05/patterns-for-decoupling-distsys-passage-of-time-event/)  
   1.5. [Fat Event](https://verraes.net/2019/05/patterns-for-decoupling-distsys-fat-event/)  
   1.6. [Explicit Public Events](https://verraes.net/2019/05/patterns-for-decoupling-distsys-explicit-public-events/)  
   1.7. [Segregated Event Layers](https://verraes.net/2019/05/patterns-for-decoupling-distsys-segregated-event-layers/)  

2. **Eventsourcing Patterns**  

   2.1. [Forgettable Payloads](https://verraes.net/2019/05/eventsourcing-patterns-forgettable-payloads/)  
   2.2. [Crypto-Shredding](https://verraes.net/2019/05/eventsourcing-patterns-throw-away-the-key/)  
   2.3. [Decision Tracking](https://verraes.net/2019/05/eventsourcing-patterns-decision-tracking/)  
   2.4. [Migration Events in a Ghost Context](https://verraes.net/2019/06/eventsourcing-patterns-migration-events-ghost-context/)  
   2.5. [Multi-temporal Events](https://verraes.net/2022/03/multi-temporal-events/)  

3. **Messaging Patterns**  

   3.1. [Ephemeral Events](https://verraes.net/2019/05/messaging-patterns-ephemeral-events/)  
   3.2. [Throttling](https://verraes.net/2019/05/messaging-patterns-throttling/)  
   3.3. [Change Detection Events](https://verraes.net/2019/05/messaging-patterns-change-detection-events/)  
   3.4. [Natural Language Message Names](https://verraes.net/2019/06/messaging-patterns-natural-language-message-names/)  

# Patterns for Decoupling in Distributed Systems

[Domain Queries](https://verraes.net/2019/05/patterns-for-decoupling-distsys-domain-query/)
> Replace Free Queries with Domain Queries to decouple from knowledge of the server’s internals.

[Summary Event](https://verraes.net/2019/05/patterns-for-decoupling-distsys-summary-event/)
> Instead of emitting a stream of Domain Events, emit a single Summary.

[Completeness Guarantee](https://verraes.net/2019/05/patterns-for-decoupling-distsys-completeness-guarantee/)
> Design the set of Domain Events from a producer so that they can be used to rebuild the producer’s state.

[Passage of Time Event](https://verraes.net/2019/05/patterns-for-decoupling-distsys-passage-of-time-event/)
> Replace cron jobs and scheduled commands, with an agnostic event to indicate the passage of time.

[Fat Event](https://verraes.net/2019/05/patterns-for-decoupling-distsys-fat-event/)
> Add redundant information to a Domain Event to reduce complexity in the consumer.

[Explicit Public Events](https://verraes.net/2019/05/patterns-for-decoupling-distsys-explicit-public-events/)
> Mark a small subset of events as public, keep the rest private by default.

[Segregated Event Layers](https://verraes.net/2019/05/patterns-for-decoupling-distsys-segregated-event-layers/)
> Explicitly segregate a Bounded Context’s events in visibility layers, with their own language.

# Eventsourcing Patterns

[Forgettable Payloads](https://verraes.net/2019/05/eventsourcing-patterns-forgettable-payloads/)
> Store the sensitive payload of an event in a separate store to control access and removal.

[Crypto-Shredding](https://verraes.net/2019/05/eventsourcing-patterns-throw-away-the-key/)
> Encrypt sensitive information in an event and delete the key.

[Decision Tracking](https://verraes.net/2019/05/eventsourcing-patterns-decision-tracking/)
> Store the outcome of a decision to guard against rule changes.

[Migration Events in a Ghost Context](https://verraes.net/2019/06/eventsourcing-patterns-migration-events-ghost-context/)
> Explicitly conform to the legacy component’s model in an eventsourcing migration.

[Multi-temporal Events](https://verraes.net/2022/03/multi-temporal-events/)
> Within a Domain Event, use separate timestamps to distinguish when the event occurred and when it was captured.

# Messaging Patterns

[Ephemeral Events](https://verraes.net/2019/05/messaging-patterns-ephemeral-events/)
> Mark the events as being Ephemeral Events. The infrastructure knows that only the latest event matters. When a consumer can’t keep up with the event stream coming in, the infrastructure prefers to drop older events and skip to the most recent one.

---

[Throttling](https://verraes.net/2019/05/messaging-patterns-throttling/)
> Only keep one event per time unit from a high frequency stream.

---

[Change Detection Events](https://verraes.net/2019/05/messaging-patterns-change-detection-events/)
> Listen to a stream of events and produce a new event when a value changes.

---

Natural Language Message Names](https://verraes.net/2019/06/messaging-patterns-natural-language-message-names/)

