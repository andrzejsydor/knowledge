---
tags:
  - Kafka
  - RabbitMQ
  - SystemDesign
  - Interview
---

# Kafka vs RabbitMQ: System Design Interview Guide

---
- [Quick Decision Tree](#quick-decision-tree)
- [Core Concepts](#core-concepts)
  - [Kafka Ordering Guarantee](#1-kafka-ordering-guarantee)
  - [RabbitMQ Acknowledgment Model](#2-rabbitmq-acknowledgment-model)
  - [Exactly-Once Semantics (Hard Problem)](#3-exactly-once-semantics-hard-problem)
- [Enterprise Patterns](#enterprise-patterns)
  - [Outbox Pattern (Producer Side)](#outbox-pattern-producer-side)
  - [Inbox Pattern (Consumer Side)](#inbox-pattern-consumer-side)
  - [Event Sourcing (Context)](#event-sourcing-context)
- [Real Scenario: 5,000 Orders/Second E-Commerce](#real-scenario-5000-orderssecond-e-commerce)
- [Cleanup Strategy: Inbox Entries](#cleanup-strategy-inbox-entries)
- [Interview Questions to Practice](#interview-questions-to-practice)
- [Kafka Offset Management](#kafka-offset-management)
- [Key Takeaways](#key-takeaways)
- [Further Deep Dives](#further-deep-dives)
- [Related notes](#related-notes)
---

## Quick Decision Tree

**Choose Kafka if:**
- Sustained high throughput (hundreds of thousands msg/sec and beyond)
- Ordering per key is critical
- Event replay and audit trail required
- Multiple consumers reading same events
- Long-term data retention needed

**Choose RabbitMQ if:**
- Throughput fits a task-queue profile (RabbitMQ comfortably handles tens of thousands msg/sec — the difference shows at sustained stream scale)
- Simple point-to-point messaging
- Built-in failure handling with acks/nacks
- Consumers are transient (don't need history)
- Operational simplicity is priority

---

## Core Concepts

### 1. Kafka Ordering Guarantee

**How it works:**
- Events with same KEY go to same PARTITION
- Within a partition, order is guaranteed
- Consumer reads sequentially from partition

**Example - Payment System:**

```
Customer A (Key: CUST_123) → Orders → Partition 0
Customer B (Key: CUST_456) → Orders → Partition 1
Customer C (Key: CUST_789) → Orders → Partition 2
```

Result: all of Customer A's orders process in order, but Customer A and B can process in parallel.

**Critical Implementation Detail:**

```java
ProducerRecord<String, OrderEvent> record = new ProducerRecord<>(
    "orders-topic",
    customerId,  // KEY - determines partition
    orderEvent   // VALUE
);
```

Note: increasing the partition count later changes key → partition mapping for new messages, so plan partitions up front if per-key ordering matters.

---

### 2. RabbitMQ Acknowledgment Model

**How it works:**
- Consumer receives message
- Consumer processes message
- Consumer sends ACK (acknowledgment)
- If the consumer's channel/connection closes without an ACK (crash, disconnect, or exceeding the broker's `consumer_timeout`, default 30 min), the message is requeued and redelivered

**Failure Scenario:**

1. Inventory service receives order event
2. Updates database (partial)
3. Connection dies before ACK
4. RabbitMQ requeues and redelivers to next consumer
5. Next consumer completes the work

**Problem: Not Idempotent**
- Same message processed twice = double deduction
- Requires business logic to prevent duplicates

---

### 3. Exactly-Once Semantics (Hard Problem)

**The Challenge — duplicates arise on both sides:**

```
Producer sends to broker
  ↓
Network fails before the ack comes back
  ↓
Producer retries → same event written twice

Consumer processes event
  ↓
Crashes before committing offset / sending ACK
  ↓
Event redelivered → processed twice
```

**Solution: Idempotency**
- Consumer must detect and skip duplicate processing
- Requires inbox pattern (see below)
- Cannot be solved by the message broker alone for end-to-end flows

Note: Kafka does offer exactly-once semantics *within* the Kafka ecosystem (idempotent producer + transactions, e.g. Kafka Streams), but once a consumer touches an external system (database, HTTP call), you're back to needing idempotency on the consumer side.

---

## Enterprise Patterns

### Outbox Pattern (Producer Side)

**Problem it solves:**
Order created in database, but event never sent to Kafka → downstream services miss the order.

**How it works:**

1. Order Service receives order
2. `BEGIN TRANSACTION`
3. `INSERT` order into `orders` table
4. `INSERT` order event into `outbox` table
5. `COMMIT` (both succeed or both fail)
6. Outbox poller reads `outbox` table periodically
7. Publishes events to Kafka
8. Marks events as published
9. Cleanup job deletes published events after retention

**Database Schema:**

```sql
CREATE TABLE outbox (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    event_id VARCHAR(255) UNIQUE NOT NULL,
    aggregate_type VARCHAR(100) NOT NULL,
    aggregate_id VARCHAR(255) NOT NULL,
    event_type VARCHAR(100) NOT NULL,
    payload JSON NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    published_at TIMESTAMP NULL
);

CREATE INDEX idx_published ON outbox(published_at);
```

**Key Benefit:**
Atomic write to database + outbox. If order fails, outbox entry fails. If order succeeds, outbox entry succeeds. No orphaned events.

Alternative: Change Data Capture (e.g. Debezium) can tail the outbox table (or the main table) instead of polling.

---

### Inbox Pattern (Consumer Side)

**Problem it solves:**
Consumer processes event, crashes before acknowledging → event replays, processed twice.

**How it works:**

1. Consumer receives `order_created` event (ID: `evt_789`)
2. Query inbox: `SELECT * FROM inbox WHERE event_id = 'evt_789'`
3. If found: already processed, return success (idempotent)
4. If not found:
   1. `BEGIN TRANSACTION`
   2. Process order (update inventory, etc.)
   3. `INSERT INTO inbox (event_id, processed_at)`
   4. `COMMIT`
5. Acknowledge message / commit offset

**Database Schema:**

```sql
CREATE TABLE inbox (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    event_id VARCHAR(255) UNIQUE NOT NULL,
    event_type VARCHAR(100) NOT NULL,
    payload JSON NOT NULL,
    processed_at TIMESTAMP DEFAULT NOW(),
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_event_id ON inbox(event_id);
```

**Key Benefit:**
Even if event replays 10 times, business logic only executes once. Inbox prevents duplicate processing.

---

### Event Sourcing (Context)

**What it is:**
Store every state change as an event, not just current state.

**Example:**

```
Traditional: orders table has status = 'SHIPPED'
             (we don't know when/why it changed)

Event Sourcing:
  order_created      {customer_id: 123, amount: 50}
  payment_captured   {order_id: 1, amount: 50}
  inventory_reserved {order_id: 1, items: [prod_A]}
  order_shipped      {order_id: 1, tracking: 'ABC123'}
```

Current state derived by replaying all events. Full audit trail. Supports time travel.

**Why it matters for Kafka:**
Kafka is an immutable log, naturally aligns with event sourcing philosophy. Good fit.

---

## Real Scenario: 5,000 Orders/Second E-Commerce

### Architecture Decision

**Why Kafka:**

```
Throughput: 5,000 orders/sec sustained
  → within RabbitMQ's reach, but Kafka handles it with headroom

Ordering: per-customer ordering required
  → partition by customer_id, guaranteed in-order per partition

Replay: analytics needs to reprocess all orders
  → Kafka retention allows replaying from the beginning

Multiple consumers: Inventory, Shipping, Notification, Analytics
  → each consumer group independently reads all events
```

The decisive factors here are replay and independent fan-out, not raw throughput.

### Full Design

```
Order Service (Producer)
  ↓
[outbox table] ← same transaction as order creation
  ↓
Outbox Poller → Kafka Topic: "orders" (partitioned by customer_id)
  ↓
Consumer Group 1: Inventory Service     (with inbox pattern)
Consumer Group 2: Shipping Service      (with inbox pattern)
Consumer Group 3: Notification Service  (with inbox pattern)
Consumer Group 4: Analytics             (no inbox — reprocessing is harmless)
  ↓
Each service processes, checks inbox, updates business state
```

### Failure Scenarios Handled

| Scenario | Kafka Handles? | Pattern Handles? |
|----------|---|---|
| Order created but event never sent | No | Outbox pattern ✓ |
| Consumer crashes mid-processing | No | Inbox pattern ✓ |
| Kafka rebalances, consumer restarts | No | Inbox pattern ✓ |
| Consumer processes event twice | No | Inbox pattern ✓ |
| Analytics needs all events from 2025 | Yes | Retention ✓ |
| Ordering per customer violated | Yes | Partitioning ✓ |

---

## Cleanup Strategy: Inbox Entries

### The Problem

Inbox table grows forever. Eventually:
- Slow queries on inbox
- Storage costs
- Recovery time from disk backups

### The Solution

**Retention-based cleanup:**

```sql
-- Run daily at 2 AM
DELETE FROM inbox
WHERE processed_at < NOW() - INTERVAL 7 DAY
  AND event_type NOT IN ('payment_confirmed', 'refund_initiated');
```

**Why 7 days?**
- Duplicates arrive shortly after the original (retries, rebalances); Kafka topic retention bounds how old a replayed event can be
- If a consumer is down longer than that, manual intervention is needed anyway
- Balance between safety and storage

Rule of thumb: inbox retention ≥ the topic's retention (or the maximum realistic replay window), otherwise a replay after cleanup will be processed as new.

**Why keep certain event types longer?**
- Financial events (payments, refunds) = longer retention for audit
- Transient events (notifications) = shorter retention

**Alternative: Partitioning by date**

```sql
-- Inbox table partitioned by processed_at month
inbox_2026_01
inbox_2026_02
inbox_2026_03

-- Drop old partitions without DELETE overhead
ALTER TABLE inbox DROP PARTITION inbox_2025_12;
```

---

## Interview Questions to Practice

1. **"Design order processing for 10k orders/sec, must guarantee no double-charging."**
   - Answer: Kafka + outbox + inbox + idempotency key

2. **"RabbitMQ vs Kafka for this use case?"**
   - Answer: Depends on throughput, ordering, replay needs. Articulate tradeoffs.

3. **"What if inventory service crashes after processing but before committing the offset?"**
   - Answer: Kafka offset not committed, event replays. Inbox pattern prevents double-deduction.

4. **"How do you prevent inbox table from becoming a bottleneck?"**
   - Answer: Partitioning, TTL-based cleanup, eventual deletion after retention window.

5. **"Can you use Kafka without outbox pattern?"**
   - Answer: Yes, but you risk losing events if the producer crashes between the DB commit and the publish. Outbox is the safety net.

6. **"What's the difference between inbox and idempotency key?"**
   - Answer: Inbox is a persistent record of processed events. Idempotency key is the unique identifier in the event itself (e.g., event_id) that the inbox is keyed on. The key enables the inbox check.

---

## Kafka Offset Management

**Consumer group offset tracking:**

```
Topic: orders
Partition 0: [msg0, msg1, msg2, msg3, msg4]
                            ↑
             consumer processed up to msg2
             commits offset = 3 (next message to read)

If consumer crashes:
  on restart, reads from offset 3 (msg3)
  never reprocesses msg0, msg1, msg2
```

**Risk:** committing the offset *before* processing means a crash mid-processing loses the event — that is at-most-once behavior.

**Mitigation:** commit offset AFTER processing + inbox insert succeed (at-least-once + idempotency = effectively exactly-once).

```
DO NOT:
1. Consume
2. Commit offset
3. Process (might fail → event lost)

DO THIS:
1. Consume
2. Process + check inbox + update database
3. Commit offset (only if step 2 succeeds)
```

---

## Key Takeaways

- ✅ **Kafka** = high throughput, per-key ordering, long retention + replay, but requires an idempotency layer
- ✅ **RabbitMQ** = simpler failure model (ack/nack, DLQ), fine for tens of thousands msg/sec; classic queues have no replay (RabbitMQ Streams add it)
- ✅ **Outbox** = guarantee events leave the producer
- ✅ **Inbox** = guarantee events are processed effectively once
- ✅ **Event Sourcing** = immutable audit trail, natural fit with Kafka
- ✅ **Idempotency** = non-negotiable for distributed systems
- ✅ **Cleanup** = delete old inbox entries after a TTL ≥ the realistic replay window

---

## Further Deep Dives

- [ ] Saga pattern (distributed transactions across services) → see [Saga](../design-pattern/Saga.md)
- [ ] Compensation transactions (how to undo failed operations)
- [ ] Change Data Capture (CDC) as alternative to outbox (Debezium)
- [ ] Schema registry (Avro/Protobuf for event evolution)
- [ ] Consumer lag monitoring and alerting
- [ ] Exactly-once vs At-least-once vs At-most-once semantics

## Related notes

- [Kafka vs RabbitMQ — comparison & cheat sheet](Kafka_vs_RabbitMQ.md)
- [Kafka vs Redis Streams](Kafka_vs_Redis_Streams.md)
- [Kafka](Kafka.md)
- [Messaging Patterns](../design-pattern/Messaging_Patterns.md)
- [Saga](../design-pattern/Saga.md)
