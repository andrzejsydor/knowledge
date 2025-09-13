---
tags: 
- Kafka
- Redis
---

## Contents

- [Using Redis Streams Instead of Kafka](#using-redis-streams-instead-of-kafka)
- [Core Ideas](#core-ideas)
- [Comparison](#comparison)
- [When to Choose Which](#when-to-choose-which)
- [Glossary](#glossary)

# Using Redis Streams Instead of Kafka

- Redis Streams provide stream processing with very low latency via in-memory append-only logs, optional persistence, consumer groups, and range queries.
- Kafka is optimized for durable, high-throughput, disk-backed streaming with partition-based scaling and consumer-group rebalancing.
- Choose Redis Streams for low-latency, queue-like workloads and simpler ops; choose Kafka for massive throughput, long retention, and strong durability.

# Core Ideas
- Stream processing often needs:
  - Many producers → central log → many consumers
  - Fan-out to multiple destinations
  - Consumer groups to divide work within a destination
- Redis Lists: simple queues with blocking pops; no easy fan-out.
- Redis Streams: append-only log; each entry holds a mini key-value map; supports:
  - Fan-out via independent consumers or multiple consumer groups
  - Time-based range queries (ID = timestamp-seq)
  - Consumer groups with group offsets and pending lists
- Storage models:
  - Kafka: disk-first, sequential I/O; strong durability
  - Redis: memory-first; optional durability (RDB/AOF); ultra-low latency
 
# Comparison

| Aspect | Redis Streams | Kafka |  
|---|---|---|  
| Storage | In-memory by default; optional persistence (RDB/AOF) | Disk-backed; sequential I/O; replicated |  
| Latency | Very low | Low, optimized for throughput |  
| Durability | Optional; weaker by default | Strong by default |  
| Partitioning | Multiple streams (keys) | Native partitions per topic |  
| Fan-out | Multiple consumer groups or independent consumers | Multiple consumer groups |  
| Consumer groups | Group offset + PEL; reads = writes (master-only) | Partition assignment; rebalancing on changes |  
| Rebalancing | Not required; manage pending via XCLAIM | Required on membership change |  
| Range queries | By ID (timestamp-seq) with XRANGE | By offset per partition; no time index by default |  
| Read scaling | Group reads must hit master | Consumers scale across brokers/partitions |  
| Best for | Low-latency queues, simple stream apps | High-throughput, durable event logs |  

# When to Choose Which
- Prefer Redis Streams if:
  - Sub-ms to low-ms latency is critical
  - Workload resembles message queues with moderate throughput
  - Operational simplicity, lightweight footprint desired
  - Short/managed retention is acceptable
- Prefer Kafka if:
  - You need very high throughput and long-term retention
  - Strong durability/replication guarantees are required
  - Many partitions and consumer scaling across brokers are needed
  - Ecosystem integrations (Connect, Streams, schema registry) matter


# Glossary
- Append-only log: A log where entries are only appended; ideal for sequential I/O.
- Consumer group: A virtual consumer composed of multiple instances; each message delivered to only one instance within the group.
- PEL (Pending Entries List): Delivered-but-not-acknowledged messages per consumer.
- Group offset: Stored position from which group reads continue in Redis Streams.
