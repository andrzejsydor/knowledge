---
tags:
  - Databases
  - SQL
  - NoSQL
---

# Databases

## Summary

This document provides an overview of different database types and their use cases. Each database type is optimized for specific data patterns, access patterns, and scalability requirements. Understanding these categories helps in selecting the right database technology for your application's needs.

## Relational
Traditional row-and-column databases, great for structured data and transactions.

## Columnar
Optimized for analytics, storing data by columns for fast aggregations.

## Key-Value
Stores data as simple key–value pairs, enabling fast lookups.

## In-memory
Stores data in RAM for ultra-low latency lookups, ideal for caching or session management.

## Wide-Column
Handles massive amounts of semi-structured data across distributed nodes.

## Time-series
Specialized for metrics, logs, and sensor data with time as a primary dimension.

## Immutable Ledger
Ensures tamper-proof, cryptographically verifiable transaction logs.

## Graph
Models complex relationships, perfect for social networks and fraud detection.

## Document
Flexible JSON-like storage, great for modern apps with evolving schemas.

## Geospatial
Manages location-aware data such as maps, routes, and spatial queries.

## Text-search
Full-text indexing and search with ranking, filters, and analytics.

## Blob
Stores unstructured objects like images, videos, and files.

## Vector
Powers AI/ML apps by enabling similarity search across embeddings.

## NewSQL
Combines the scalability of NoSQL with the ACID guarantees of relational databases.

## Multi-model
Supports multiple data models (e.g., document, graph, relational) within a single backend.

---

## Transaction Isolation Levels

Transaction isolation levels define how concurrent transactions interact with each other, controlling the trade-off between data consistency and performance. They are part of the **ACID** properties (Atomicity, Consistency, **Isolation**, Durability).

### Isolation Levels Overview

| Level | Dirty Read | Non-Repeatable Read | Phantom Read | Performance |
|-------|:----------:|:-------------------:|:------------:|:-----------:|
| Read Uncommitted | ✓ Possible | ✓ Possible | ✓ Possible | Fastest |
| Read Committed | ✗ Prevented | ✓ Possible | ✓ Possible | Fast |
| Repeatable Read | ✗ Prevented | ✗ Prevented | ✓ Possible | Moderate |
| Serializable | ✗ Prevented | ✗ Prevented | ✗ Prevented | Slowest |

### Anomalies Explained

- **Dirty Read**: Reading uncommitted changes from another transaction that may be rolled back.
- **Non-Repeatable Read**: Reading the same row twice yields different values because another transaction modified it.
- **Phantom Read**: Re-executing a query returns new rows that were inserted by another committed transaction.

### When to Use Each Level

- **Read Uncommitted**: Rarely used; only for approximate analytics where dirty reads are acceptable.
- **Read Committed**: General-purpose default; good balance for most OLTP workloads.
- **Repeatable Read**: When you need consistent reads within a transaction (e.g., financial reports).
- **Serializable**: Critical transactions requiring full isolation (e.g., bank transfers, inventory updates).
