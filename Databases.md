---
tags:
  - Databases
  - SQL
  - NoSQL
---

# Databases

---

- [Summary](#summary)
- [Database Types](#database-types)
  - [Relational](#relational)
  - [Columnar](#columnar)
  - [Key-Value](#key-value)
  - [In-memory](#in-memory)
  - [Wide-Column](#wide-column)
  - [Time-series](#time-series)
  - [Immutable Ledger](#immutable-ledger)
  - [Graph](#graph)
  - [Document](#document)
  - [Geospatial](#geospatial)
  - [Text-search](#text-search)
  - [Blob](#blob)
  - [Vector](#vector)
  - [NewSQL](#newsql)
  - [Multi-model](#multi-model)
- [Transaction Isolation Levels](#transaction-isolation-levels)
  - [Read Uncommitted](#1-read-uncommitted)
  - [Read Committed](#2-read-committed)
  - [Repeatable Read](#3-repeatable-read)
  - [Serializable](#4-serializable)

---

## Summary

This document provides an overview of different database types and their use cases. Each database type is optimized for specific data patterns, access patterns, and scalability requirements. Understanding these categories helps in selecting the right database technology for your application's needs.

## Database Types

### Relational

Traditional row-and-column databases, great for structured data and transactions.

### Columnar

Optimized for analytics, storing data by columns for fast aggregations.

### Key-Value

Stores data as simple key–value pairs, enabling fast lookups.

### In-memory

Stores data in RAM for ultra-low latency lookups, ideal for caching or session management.

### Wide-Column

Handles massive amounts of semi-structured data across distributed nodes.

### Time-series

Specialized for metrics, logs, and sensor data with time as a primary dimension.

### Immutable Ledger

Ensures tamper-proof, cryptographically verifiable transaction logs.

### Graph

Models complex relationships, perfect for social networks and fraud detection.

### Document

Flexible JSON-like storage, great for modern apps with evolving schemas.

### Geospatial

Manages location-aware data such as maps, routes, and spatial queries.

### Text-search

Full-text indexing and search with ranking, filters, and analytics.

### Blob

Stores unstructured objects like images, videos, and files.

### Vector

Powers AI/ML apps by enabling similarity search across embeddings.

### NewSQL

Combines the scalability of NoSQL with the ACID guarantees of relational databases.

### Multi-model

Supports multiple data models (e.g., document, graph, relational) within a single backend.

---

## Transaction Isolation Levels

Isolation determines how transaction integrity is maintained and how changes made by one transaction are visible to others.

| Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read |
| --- | --- | --- | --- |
| **Read Uncommitted** | ✅ Allowed | ✅ Allowed | ✅ Allowed |
| **Read Committed** | ❌ Prevented | ✅ Allowed | ✅ Allowed |
| **Repeatable Read** | ❌ Prevented | ❌ Prevented | ✅ Allowed |
| **Serializable** | ❌ Prevented | ❌ Prevented | ❌ Prevented |

**Definitions:**

- **Dirty Read:** Reading uncommitted data.
- **Non-Repeatable Read:** Reading the same row twice gets different values (because someone updated it).
- **Phantom Read:** Running the same query twice gets a different set of rows (because someone inserted/deleted rows).

---

### 1. Read Uncommitted

This is the lowest level of isolation. Transactions are allowed to read data that has been modified by other transactions but not yet committed.

- **Key Characteristic:** "Dirty Reads" are allowed.
- **Pros:** Fastest performance; no locking overhead.
- **Cons:** Extremely risky. You might read data that is essentially "rolled back" or never actually existed in a final state.

### 2. Read Committed

This restricts a transaction to reading only data that has been permanently committed. It guarantees that you never read temporary or aborted changes.

- **Key Characteristic:** Prevents Dirty Reads.
- **Note:** This is the **default** setting for many popular databases (e.g., PostgreSQL, SQL Server).
- **Cons:** Does not guarantee that if you read a row twice in the same transaction, you will get the same result (Non-Repeatable Reads).

### 3. Repeatable Read

This level guarantees that if a transaction reads a row, subsequent reads of that same row within the same transaction will yield the same data, even if other transactions change it in the meantime.

- **Key Characteristic:** Prevents Dirty Reads and Non-Repeatable Reads.
- **Cons:** "Phantom Reads" are still possible (where new rows matching a search criteria are added by another user appearing as "phantoms" in subsequent queries).
- **Note:** This is the default in MySQL (InnoDB).

### 4. Serializable

The highest and strictest level. It emulates serial transaction execution, as if transactions were run one after another rather than concurrently.

- **Key Characteristic:** Prevents **all** anomalies: Dirty Reads, Non-Repeatable Reads, and Phantom Reads.
- **Pros:** Perfect data consistency.
- **Cons:** Lowest concurrency and performance. High probability of transaction timeouts or deadlocks.
