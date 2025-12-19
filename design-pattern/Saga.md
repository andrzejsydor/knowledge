---
tags:
  - Design Patterns
  - Saga
  - Choreography
  - Orchestration
summary: Saga coordination options for long-running, distributed transactions.
---

# Saga Pattern

## Table of Contents
- [Overview](#overview)
- [Patterns](#patterns)
  - [Saga Approaches](#saga-approaches)
  - [Notes and Comparisons](#notes-and-comparisons)
- [Links](#links)

## Overview
- Sagas split a long-running transaction into ordered local transactions with compensating actions.
- Coordination can be decentralized via events or centralized via a dedicated orchestrator.
- Choose based on coupling tolerance, visibility requirements, and governance needs.

## Patterns

### Saga Approaches
#### Choreography
- Each local transaction publishes domain events that trigger subsequent steps in other services.
- Pros: simple and decentralized; Cons: reactive sprawl and harder global policy enforcement.

#### Orchestration
- A coordinator issues commands telling participants which local transaction to execute.
- Pros: clear control/visibility; Cons: coordinator can become a bottleneck or single point of failure.

### Notes and Comparisons
- [Saga vs Process Manager](https://stackoverflow.com/questions/15528015/what-is-the-difference-between-a-saga-a-process-manager-and-a-document-based-ap) — Process managers focus on message routing/state while sagas emphasize transactional guarantees and compensation.

## Links
- [Saga Pattern (microservices.io)](https://microservices.io/patterns/data/saga.html)
