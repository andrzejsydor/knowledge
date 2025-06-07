---
tags:
  - Eventsourcing Patterns
  - Messaging Patterns
---

## Table of Contents

1. **Eventsourcing Patterns**
   1.1. [Forgettable Payloads](https://verraes.net/2019/05/eventsourcing-patterns-forgettable-payloads/)  
   1.2. [Crypto-Shredding](https://verraes.net/2019/05/eventsourcing-patterns-throw-away-the-key/)  
   1.3. [Decision Tracking](https://verraes.net/2019/05/eventsourcing-patterns-decision-tracking/)  
   1.4. [Migration Events in a Ghost Context](https://verraes.net/2019/06/eventsourcing-patterns-migration-events-ghost-context/)  
   1.5. [Multi-temporal Events](https://verraes.net/2022/03/multi-temporal-events/)  

2. **Messaging Patterns**
   2.1. [Ephemeral Events](https://verraes.net/2019/05/messaging-patterns-ephemeral-events/)  
   2.2. [Throttling](https://verraes.net/2019/05/messaging-patterns-throttling/)  
   2.3. [Change Detection Events](https://verraes.net/2019/05/messaging-patterns-change-detection-events/)  
   2.4. [Natural Language Message Names](https://verraes.net/2019/06/messaging-patterns-natural-language-message-names/)


# Eventsourcing Patterns

**Forgettable Payloads**
[forgettable-payloads](https://verraes.net/2019/05/eventsourcing-patterns-forgettable-payloads/)

> Store the sensitive payload of an event in a separate store to control access and removal.

**Crypto-Shredding**
[throw-away-the-key](https://verraes.net/2019/05/eventsourcing-patterns-throw-away-the-key/)

> Encrypt sensitive information in an event and delete the key.

**Decision Tracking**
[patterns-decision-tracking](https://verraes.net/2019/05/eventsourcing-patterns-decision-tracking/)

> Store the outcome of a decision to guard against rule changes.

**Migration Events in a Ghost Context**
[migration-events-ghost-context](https://verraes.net/2019/06/eventsourcing-patterns-migration-events-ghost-context/)

> Explicitly conform to the legacy component’s model in an eventsourcing migration.

**Multi-temporal Events**  
[multi-temporal-events](https://verraes.net/2022/03/multi-temporal-events/)

> Within a Domain Event, use separate timestamps to distinguish when the event occurred and when it was captured.


# Messaging Patterns

**Ephemeral Events**  
[messaging-patterns-ephemeral-events](https://verraes.net/2019/05/messaging-patterns-ephemeral-events/)

> Mark the events as being Ephemeral Events. The infrastructure knows that only the latest event matters. When a consumer can’t keep up with the event stream coming in, the infrastructure prefers to drop older events and skip to the most recent one.

---

**Throttling**  
[messaging-patterns-throttling](https://verraes.net/2019/05/messaging-patterns-throttling/)

> Only keep one event per time unit from a high frequency stream.

---

**Change Detection Events**  
[messaging-patterns-change-detection-events](https://verraes.net/2019/05/messaging-patterns-change-detection-events/)

> Listen to a stream of events and produce a new event when a value changes.

---

**Natural Language Message Names**  
[messaging-patterns-natural-language-message-names](https://verraes.net/2019/06/messaging-patterns-natural-language-message-names/)


