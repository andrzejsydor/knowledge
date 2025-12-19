---
tags:
  - Design Patterns
  - Messaging Patterns
  - Event-Driven Architecture
  - Eventsourcing Patterns
  - Canonical Data Model
summary: Curated messaging and eventsourcing patterns for distributed systems.
---

# Messaging Patterns

## Table of Contents
- [Overview](#overview)
- [Patterns](#patterns)
  - [Patterns for Decoupling in Distributed Systems](#patterns-for-decoupling-in-distributed-systems)
  - [Eventsourcing Patterns](#eventsourcing-patterns)
  - [Messaging Patterns](#messaging-patterns)
  - [Other Messaging Concepts](#other-messaging-concepts)
- [Links](#links)

## Overview
- Decouple producers and consumers while keeping contracts explicit.
- Manage sensitive data, migrations, and timelines when events are the source of truth.
- Tune messaging flows for throughput, ordering, and clarity.

## Patterns

### Patterns for Decoupling in Distributed Systems
- [Domain Queries](https://verraes.net/2019/05/patterns-for-decoupling-distsys-domain-query/) — Replace Free Queries with Domain Queries to decouple from knowledge of the server’s internals.
- [Summary Event](https://verraes.net/2019/05/patterns-for-decoupling-distsys-summary-event/) — Emit a single summary instead of a stream of domain events.
- [Completeness Guarantee](https://verraes.net/2019/05/patterns-for-decoupling-distsys-completeness-guarantee/) — Design producer events so consumers can rebuild producer state.
- [Passage of Time Event](https://verraes.net/2019/05/patterns-for-decoupling-distsys-passage-of-time-event/) — Use an agnostic event to signal time rather than cron jobs.
- [Fat Event](https://verraes.net/2019/05/patterns-for-decoupling-distsys-fat-event/) — Add redundant information to reduce consumer complexity.
- [Explicit Public Events](https://verraes.net/2019/05/patterns-for-decoupling-distsys-explicit-public-events/) — Mark a small subset of events as public; keep the rest private by default.
- [Segregated Event Layers](https://verraes.net/2019/05/patterns-for-decoupling-distsys-segregated-event-layers/) — Separate a bounded context’s events into visibility layers with their own language.

### Eventsourcing Patterns
- [Forgettable Payloads](https://verraes.net/2019/05/eventsourcing-patterns-forgettable-payloads/) — Store sensitive payloads separately to control access and removal.
- [Crypto-Shredding](https://verraes.net/2019/05/eventsourcing-patterns-throw-away-the-key/) — Encrypt sensitive information in an event and delete the key.
- [Decision Tracking](https://verraes.net/2019/05/eventsourcing-patterns-decision-tracking/) — Store the outcome of a decision to guard against rule changes.
- [Migration Events in a Ghost Context](https://verraes.net/2019/06/eventsourcing-patterns-migration-events-ghost-context/) — Conform explicitly to a legacy model during eventsourcing migrations.
- [Multi-temporal Events](https://verraes.net/2022/03/multi-temporal-events/) — Separate timestamps for when an event occurred vs when it was captured.

### Messaging Patterns
- [Ephemeral Events](https://verraes.net/2019/05/messaging-patterns-ephemeral-events/) — Only the latest event matters; infrastructure can drop older ones if consumers lag.
- [Throttling](https://verraes.net/2019/05/messaging-patterns-throttling/) — Keep one event per time unit from a high-frequency stream.
- [Change Detection Events](https://verraes.net/2019/05/messaging-patterns-change-detection-events/) — Produce a new event when a value changes across a stream.
- [Natural Language Message Names](https://verraes.net/2019/06/messaging-patterns-natural-language-message-names/) — Name messages in plain language to clarify intent.

### Other Messaging Concepts
- [Canonical Data Model](https://www.enterpriseintegrationpatterns.com/patterns/messaging/CanonicalDataModel.html) — Standardized common data format for interop across systems.

## Links
- [Patterns for Decoupling in Distributed Systems (series)](https://verraes.net/2019/05/patterns-for-decoupling-distsys-domain-query/)
- [Enterprise Integration Patterns: Canonical Data Model](https://www.enterpriseintegrationpatterns.com/patterns/messaging/CanonicalDataModel.html)
