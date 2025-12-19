---
tags:
  - Design Patterns
  - Rate Limiting
  - Traffic Management
summary: Quick reference for rate limiting strategies, trade-offs, and flow.
---

# Rate Limiting Patterns

## Table of Contents
- [Overview](#overview)
- [Patterns](#patterns)
  - [Strategy Comparison](#strategy-comparison)
  - [Request Flow](#request-flow)
- [Links](#links)

## Overview
- Control inbound requests to protect downstream systems and enforce quotas.
- Select strategies based on accuracy needs, cost to compute, and tolerance for bursts.
- Use a shared store (e.g., Redis) when coordinating counters across instances.

## Patterns

### Strategy Comparison
| Strategy | Core Idea | Strength | Trade-off | Typical Fit |
|---|---|---|---|---|
| Fixed Window Counter | Count per discrete window | Simple, cheap | Boundary spikes | Basic quotas |
| Sliding Window Log | Exact per-request timestamps | Most accurate | Memory/CPU heavy | Strict fairness |
| Sliding Window Counter | Weighted prev/current windows | Efficient, near-accurate | Approximation | High-scale APIs |
| Token Bucket | Tokens refill at rate $r$, cap $b$ | Allows bursts, easy to distribute | Not constant outflow | Client/API ingress control |
| Leaky Bucket | FIFO drains at fixed rate | Smooth, protects backend | Queue latency, drops | Downstream protection |

### Request Flow
```mermaid
sequenceDiagram
    autonumber
    participant C as Client
    participant RL as Rate Limiter
    participant DS as Data Store (e.g., Redis)
    participant API as API Server

    C->>RL: Request
    RL->>DS: Check & update usage
    alt Within limit
        RL->>API: Forward request
        API-->>C: 2xx Response
    else Exceeded
        RL-->>C: 429 Too Many Requests
    end
```

## Links
- [Cloud Design Patterns](https://learn.microsoft.com/en-us/azure/architecture/patterns/)
- [Design Patterns](https://refactoring.guru/design-patterns)
