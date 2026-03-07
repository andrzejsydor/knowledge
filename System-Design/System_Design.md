# System Design Interview Guide

A comprehensive guide to preparing for and passing system design interviews at the **Senior** and **Staff Engineer** level.

---

## Table of Contents

1. [What Is a System Design Interview?](#what-is-a-system-design-interview)
2. [Why It Matters at Senior / Staff Level](#why-it-matters-at-senior--staff-level)
3. [Interview Format](#interview-format)
4. [The Framework: How to Approach Any Problem](#the-framework-how-to-approach-any-problem)
5. [Core Building Blocks](#core-building-blocks)
6. [Key Concepts You Must Know](#key-concepts-you-must-know)
7. [Back-of-the-Envelope Estimation](#back-of-the-envelope-estimation)
8. [Common Interview Questions by Difficulty](#common-interview-questions-by-difficulty)
9. [What Interviewers Look For at Each Level](#what-interviewers-look-for-at-each-level)
10. [Study Plan](#study-plan)
11. [Recommended Resources](#recommended-resources)
12. [Solutions](#solutions)
    - [Solution 1: Design a Monitoring and Alerting System (Grafana + Prometheus)](#solution-1-design-a-monitoring-and-alerting-system-grafana--prometheus)

---

## What Is a System Design Interview?

A system design interview is an open-ended conversation where you are asked to design a large-scale distributed system. There is no single correct answer. The interviewer evaluates your **thought process**, **trade-off analysis**, and **ability to communicate** a coherent architecture under constraints.

Examples of prompts:

- "Design a URL shortener like bit.ly"
- "Design a news feed system like Twitter"
- "Design a distributed message queue like Kafka"
- "Design a monitoring and alerting system like Grafana + Prometheus"

The interview typically lasts **45-60 minutes** and is almost entirely driven by you.

---

## Why It Matters at Senior / Staff Level

| Level | Expectation |
|-------|-------------|
| **Mid-level** | Can implement a well-defined component within a system |
| **Senior** | Can design an end-to-end system, identify trade-offs, and justify decisions |
| **Staff** | Can design cross-team systems, anticipate failure modes, drive technical strategy, and align architecture with business goals |

At the senior and staff level, interviewers are **not** looking for textbook answers. They are looking for:

- Structured thinking under ambiguity
- Depth in at least one area (storage, networking, distributed systems)
- Breadth across the full stack
- Clear articulation of trade-offs (not just "use Redis")
- Awareness of operational concerns: monitoring, deployment, failure recovery

---

## Interview Format

A typical 45-60 minute system design interview follows this rough timeline:

| Phase | Duration | What Happens |
|-------|----------|--------------|
| **1. Requirements Clarification** | 5-7 min | Ask questions, define scope, agree on functional and non-functional requirements |
| **2. High-Level Design** | 10-15 min | Draw the major components and data flow |
| **3. Deep Dive** | 15-20 min | Drill into 1-2 components in detail (the interviewer often guides this) |
| **4. Scaling & Trade-offs** | 5-10 min | Discuss bottlenecks, scaling strategies, failure handling |
| **5. Wrap-up / Q&A** | 5 min | Summarize, discuss extensions, ask questions |

---

## The Framework: How to Approach Any Problem

Use this repeatable framework for every system design question.

### Step 1 — Clarify Requirements

Never start designing immediately. Ask questions to reduce ambiguity.

**Functional Requirements** — What does the system do?

- What are the core use cases?
- Who are the users (end users, internal services, APIs)?
- What are the input/output contracts?

**Non-Functional Requirements** — How well must it do it?

- Scale: How many users? Requests per second? Data volume?
- Latency: What is the acceptable response time?
- Availability: What is the uptime target (99.9%? 99.99%)?
- Consistency: Is strong consistency required, or is eventual consistency acceptable?
- Durability: Can we afford to lose data?

**Constraints & Assumptions**

- Budget, team size, timeline
- Existing infrastructure
- Regulatory requirements (GDPR, HIPAA)

### Step 2 — Back-of-the-Envelope Estimation

Estimate the scale to guide your design decisions:

- Daily active users (DAU)
- Read-to-write ratio
- Storage requirements over 1 year / 5 years
- Bandwidth requirements
- QPS (queries per second) at peak

### Step 3 — High-Level Design

Sketch the major components:

```
Client → Load Balancer → API Gateway → Service Layer → Database
                                            ↓
                                        Cache Layer
                                            ↓
                                      Message Queue → Workers
```

Define:

- API endpoints (REST, gRPC, GraphQL)
- Data model and schema
- Major services and their responsibilities
- Data flow for the primary use cases

### Step 4 — Deep Dive

Pick 1-2 areas that are most critical or interesting and go deep:

- Database schema design and indexing strategy
- Caching strategy (what to cache, invalidation policy)
- How to handle hot partitions or hot keys
- Consistency model and conflict resolution
- Search or ranking algorithms

### Step 5 — Address Scaling, Reliability, and Operations

- Horizontal scaling strategies
- Database sharding and replication
- Failure modes and mitigation (circuit breakers, retries, fallbacks)
- Monitoring, alerting, and observability
- Deployment strategy (blue-green, canary)
- Security considerations

---

## Core Building Blocks

These are the fundamental components you should be able to discuss fluently.

### Networking & Communication

| Component | Purpose | When to Use |
|-----------|---------|-------------|
| **Load Balancer** | Distribute traffic across servers | Almost every design |
| **API Gateway** | Single entry point, rate limiting, auth | Microservice architectures |
| **CDN** | Cache static content at the edge | Media-heavy, global user base |
| **DNS** | Domain name resolution, geo-routing | Global services |

### Compute

| Component | Purpose | When to Use |
|-----------|---------|-------------|
| **Web Servers** | Handle HTTP requests | Stateless request processing |
| **Workers** | Async background processing | Email, image processing, ETL |
| **Serverless** | Event-driven, auto-scaling compute | Bursty, unpredictable workloads |

### Storage

| Component | Purpose | When to Use |
|-----------|---------|-------------|
| **Relational DB (PostgreSQL, MySQL)** | Structured data with ACID transactions | Financial data, user accounts |
| **NoSQL Document (MongoDB)** | Flexible schema, denormalized data | Product catalogs, CMS |
| **Key-Value Store (Redis, DynamoDB)** | Fast lookups, caching, sessions | Caching, leaderboards, rate limiting |
| **Wide-Column (Cassandra, HBase)** | Time-series, high write throughput | IoT data, activity logs |
| **Search Engine (Elasticsearch)** | Full-text search, analytics | Log search, product search |
| **Object Storage (S3)** | Unstructured blobs (images, videos) | Media storage, backups |
| **Graph DB (Neo4j)** | Relationship-heavy queries | Social networks, recommendations |

### Messaging & Streaming

| Component | Purpose | When to Use |
|-----------|---------|-------------|
| **Message Queue (RabbitMQ, SQS)** | Async task processing, decoupling | Order processing, notifications |
| **Event Streaming (Kafka)** | High-throughput event log | Activity feeds, data pipelines |
| **Pub/Sub** | Fan-out messaging | Real-time notifications |

### Caching

| Layer | Example | Use Case |
|-------|---------|----------|
| **Client-side** | Browser cache, HTTP headers | Static assets |
| **CDN** | CloudFront, Akamai | Geographically distributed content |
| **Application** | Redis, Memcached | Database query results, sessions |
| **Database** | Query cache, materialized views | Expensive aggregations |

---

## Key Concepts You Must Know

### CAP Theorem

In a distributed system, you can only guarantee **two out of three**:

- **C**onsistency — Every read returns the most recent write
- **A**vailability — Every request receives a response
- **P**artition Tolerance — The system operates despite network failures

In practice, partitions are unavoidable, so the real trade-off is **CP vs AP**.

### Consistency Models

| Model | Description | Example |
|-------|-------------|---------|
| **Strong** | Reads always see the latest write | Banking transactions |
| **Eventual** | Reads may be stale but will converge | Social media likes count |
| **Causal** | Preserves cause-and-effect ordering | Chat messages |

### Database Scaling

- **Vertical Scaling** — Bigger machine (limited ceiling)
- **Horizontal Scaling** — More machines (requires sharding)
- **Read Replicas** — Offload reads to replicas
- **Sharding** — Partition data across databases by a shard key
  - Range-based: simple but can create hot spots
  - Hash-based: even distribution but range queries are expensive
  - Directory-based: flexible but the lookup table is a single point of failure

### Rate Limiting

Algorithms to know:

- **Token Bucket** — Allows bursts, smooth average rate
- **Sliding Window** — More precise, higher memory
- **Fixed Window** — Simple, but spikes at window boundaries

### Consistent Hashing

Used for distributing data across nodes (caches, databases) while minimizing redistribution when nodes are added or removed. Essential for cache clusters and distributed storage.

### Leader Election & Consensus

- **Raft** — Understandable consensus protocol
- **Paxos** — Foundational, harder to implement
- **ZooKeeper** — Coordination service for distributed systems

### Idempotency

Ensuring that performing the same operation multiple times has the same effect as performing it once. Critical for:

- Payment processing
- Message delivery (at-least-once semantics)
- API retry safety

---

## Back-of-the-Envelope Estimation

### Numbers Every Engineer Should Know

| Operation | Latency |
|-----------|---------|
| L1 cache reference | 0.5 ns |
| L2 cache reference | 7 ns |
| Main memory reference | 100 ns |
| SSD random read | 150 μs |
| HDD random read | 10 ms |
| Send 1 KB over 1 Gbps network | 10 μs |
| Read 1 MB sequentially from memory | 250 μs |
| Read 1 MB sequentially from SSD | 1 ms |
| Read 1 MB sequentially from HDD | 20 ms |
| Round trip within same datacenter | 500 μs |
| Round trip CA to Netherlands | 150 ms |

### Quick Reference for Scale

| Metric | Value |
|--------|-------|
| Seconds in a day | ~86,400 (~100K for estimation) |
| Seconds in a month | ~2.5 million |
| Seconds in a year | ~31.5 million |
| 1 million requests/day | ~12 QPS |
| 1 billion requests/day | ~12,000 QPS |
| 1 KB * 1 billion | ~1 TB |

### Example: URL Shortener

- 100M new URLs/month → ~40 URLs/second (write QPS)
- Read-to-write ratio 100:1 → ~4,000 read QPS
- Average URL: 100 bytes → 100M * 100 bytes = 10 GB/month → 600 GB over 5 years
- With metadata and indexes: ~1-2 TB over 5 years → fits on a single machine with replication

---

## Common Interview Questions by Difficulty

### Intermediate (Senior Warm-Up)

1. Design a URL shortener (bit.ly)
2. Design a rate limiter
3. Design a key-value store
4. Design a notification system
5. Design a paste service (Pastebin)

### Advanced (Senior Level)

6. Design a news feed system (Twitter / Facebook)
7. Design a chat system (WhatsApp / Slack)
8. Design a web crawler
9. Design a video streaming platform (YouTube / Netflix)
10. Design a search autocomplete system
11. Design a ride-sharing service (Uber / Lyft)
12. Design a file storage service (Google Drive / Dropbox)

### Expert (Staff Level)

13. Design a distributed message queue (Kafka)
14. Design a monitoring and alerting system (Grafana + Prometheus)
15. Design a distributed task scheduler
16. Design a global-scale payment system
17. Design a multi-tenant SaaS platform
18. Design a real-time collaborative editor (Google Docs)
19. Design a distributed cache (Memcached / Redis Cluster)
20. Design a search engine (Google-scale)

---

## What Interviewers Look For at Each Level

### Senior Engineer

| Signal | What It Looks Like |
|--------|--------------------|
| **Structured approach** | Follows a clear framework; doesn't jump to solutions |
| **Requirements gathering** | Asks clarifying questions before designing |
| **Solid fundamentals** | Correctly uses databases, caches, queues, load balancers |
| **Trade-off awareness** | Can explain why they chose SQL over NoSQL, or AP over CP |
| **Depth in one area** | Can go deep on at least one component (e.g., database sharding) |
| **Scalability thinking** | Identifies bottlenecks and proposes solutions |

### Staff Engineer

Everything above, plus:

| Signal | What It Looks Like |
|--------|--------------------|
| **End-to-end ownership** | Considers deployment, monitoring, security, cost |
| **Cross-system thinking** | Designs for how the system interacts with other systems |
| **Failure mode analysis** | Proactively discusses what can go wrong and how to recover |
| **Business alignment** | Connects technical decisions to business impact |
| **Evolution & migration** | Discusses how the system evolves over time, migration strategies |
| **Influencing without authority** | Explains decisions clearly enough to convince stakeholders |
| **Operational maturity** | Discusses SLOs, SLIs, runbooks, incident response |

---

## Study Plan

### Weeks 1-2: Foundations

- [ ] Review core distributed systems concepts (CAP, consistency, replication)
- [ ] Study the building blocks table above until you can discuss each one fluently
- [ ] Practice back-of-the-envelope estimation with 3-5 problems
- [ ] Read "Designing Data-Intensive Applications" chapters 1-5

### Weeks 3-4: Practice Core Problems

- [ ] Design a URL shortener (practice the framework end to end)
- [ ] Design a rate limiter
- [ ] Design a news feed system
- [ ] Design a chat system
- [ ] For each: write out the full design in a document, time yourself to 45 min

### Weeks 5-6: Advanced Problems & Depth

- [ ] Design a distributed message queue
- [ ] Design a monitoring and alerting system
- [ ] Design a search autocomplete system
- [ ] Go deep on your area of expertise (storage, streaming, networking)
- [ ] Practice with a partner or use mock interview platforms

### Weeks 7-8: Polish & Mock Interviews

- [ ] Do at least 3 mock interviews with peers or on platforms like Pramp, Interviewing.io
- [ ] Review your weak areas based on mock feedback
- [ ] Practice articulating trade-offs out loud (not just in your head)
- [ ] Prepare your "go-to" examples from real work experience
- [ ] Review operational concerns: monitoring, alerting, deployment, incident response

---

## Recommended Resources

### Books

- **"Designing Data-Intensive Applications"** by Martin Kleppmann — The single most important book for system design interviews
- **"System Design Interview"** by Alex Xu (Vol. 1 & 2) — Structured walkthroughs of common problems
- **"Understanding Distributed Systems"** by Roberto Vitillo — Concise and practical

### Online

- **[System Design Primer (GitHub)](https://github.com/donnemartin/system-design-primer)** — Comprehensive open-source guide
- **[ByteByteGo](https://bytebytego.com)** — Alex Xu's visual system design content
- **[Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview)** — Structured course with common problems
- **[High Scalability Blog](http://highscalability.com)** — Real-world architecture case studies

### Practice Platforms

- **[Interviewing.io](https://interviewing.io)** — Anonymous mock interviews with engineers from top companies
- **[Pramp](https://pramp.com)** — Free peer-to-peer mock interviews
- **[Exponent](https://tryexponent.com)** — System design mock interview videos and practice

### Engineering Blogs (Real-World Architectures)

- [Netflix Tech Blog](https://netflixtechblog.com)
- [Uber Engineering](https://eng.uber.com)
- [Meta Engineering](https://engineering.fb.com)
- [Grafana Labs Blog](https://grafana.com/blog/engineering)
- [Airbnb Engineering](https://medium.com/airbnb-engineering)
- [Stripe Engineering](https://stripe.com/blog/engineering)

---

## Quick Reference: The Golden Rules

1. **Never skip requirements clarification** — It is the #1 mistake candidates make.
2. **Drive the conversation** — The interviewer wants to see you lead, not be led.
3. **State trade-offs explicitly** — "I'm choosing X over Y because of Z constraint."
4. **Start simple, then iterate** — Begin with a single-server design, then scale it.
5. **Use numbers** — Back up claims with rough estimation.
6. **Think out loud** — The interviewer can only evaluate what they hear.
7. **Know when to go deep vs. wide** — Read the interviewer's signals.
8. **Connect to real experience** — "In my previous role, we faced a similar problem..."
9. **Consider operational concerns** — Monitoring, deployment, failure recovery.
10. **Practice under time pressure** — 45 minutes goes fast.

---

## Solutions

### Solution 1: Design a Monitoring and Alerting System (Grafana + Prometheus)

> **Difficulty:** Staff Level
> **Time allocation:** 45-60 minutes
> **Real-world references:** Grafana, Prometheus, Datadog, New Relic, Splunk, AWS CloudWatch

---

#### Opening Statement (What to Say First)

> "Before I start designing, I want to make sure I understand the scope. A monitoring and alerting system is a broad domain — it can cover infrastructure metrics, application metrics, logs, traces, and alerting. Let me ask a few questions to narrow things down."

This immediately signals structured thinking and prevents the common mistake of jumping straight into a solution.

---

#### Step 1 — Requirements Clarification (5-7 min)

**Questions to ask the interviewer:**

1. What types of data are we monitoring? (metrics, logs, traces, or all three?)
2. What is the scale — how many hosts/services are we collecting from?
3. Who are the users — SREs, developers, business analysts?
4. Do we need real-time dashboards, or is near-real-time acceptable?
5. What is the alerting latency requirement — how fast must we detect and notify?
6. Do we need multi-tenancy (multiple teams / orgs)?
7. How long do we retain data?

**Agreed-upon scope for this solution:**

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Metric collection** | Collect time-series metrics from thousands of services and infrastructure components |
| FR2 | **Metric storage** | Store metrics efficiently with configurable retention (hot: 15 days, warm: 90 days, cold: 1+ year) |
| FR3 | **Querying** | Users can query metrics using a flexible query language (e.g., PromQL-like) |
| FR4 | **Dashboards** | Users can create, save, and share real-time dashboards with graphs, tables, gauges |
| FR5 | **Alerting rules** | Users define alert rules (e.g., "CPU > 90% for 5 minutes") that are continuously evaluated |
| FR6 | **Alert notifications** | When alerts fire, notifications are sent via multiple channels (email, Slack, PagerDuty, webhook) |
| FR7 | **Alert management** | Silencing, grouping, deduplication, and escalation of alerts |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Write throughput** | 10 million data points per second |
| NFR2 | **Query latency** | Dashboard queries return in < 2 seconds (p99) for recent data |
| NFR3 | **Alert latency** | From metric ingestion to notification delivery in < 60 seconds |
| NFR4 | **Availability** | 99.9% uptime for ingestion path; alerting must be highly available |
| NFR5 | **Retention** | Hot data (15 days), warm (90 days), cold archival (1+ year) |
| NFR6 | **Multi-tenancy** | Isolate data and queries between teams/organizations |
| NFR7 | **Durability** | No data loss for ingested metrics (replicated storage) |

---

#### Step 2 — Back-of-the-Envelope Estimation (3-5 min)

##### Scale Assumptions

| Metric | Value |
|--------|-------|
| Monitored services | 10,000 |
| Metrics per service | ~500 unique time series |
| Total active time series | 5 million |
| Scrape interval | 15 seconds |
| Data points per second (write) | 5M / 15 = ~333K samples/sec per scraper (with multiple scrapers: ~10M/sec total) |
| Average data point size | ~16 bytes (timestamp 8B + float64 value 8B) + labels ~100 bytes on first write |
| Compressed sample size | ~1-2 bytes/sample (with Gorilla-style compression) |

##### Storage Estimation

| Duration | Calculation | Size |
|----------|-------------|------|
| 1 hour | 10M samples/sec * 3600 sec * 2 bytes | ~72 GB |
| 1 day | 72 GB * 24 | ~1.7 TB |
| 15 days (hot) | 1.7 TB * 15 | ~26 TB |
| 90 days (warm) | 1.7 TB * 90 | ~155 TB (further downsampled → ~30 TB) |
| 1 year (cold) | Downsampled (5-min resolution) | ~10 TB |

##### Query Load

| Metric | Value |
|--------|-------|
| Concurrent dashboard users | ~1,000 |
| Queries per dashboard load | ~10-20 |
| Query QPS (peak) | ~5,000 |
| Alert rules evaluated | ~50,000 rules every 30-60 seconds |

**Design implications:**

- Write-heavy workload — need a storage engine optimized for time-series append
- Compression is critical — Gorilla / double-delta encoding makes this feasible
- Tiered storage is necessary — can't keep a year of data at full resolution
- Alert evaluation is a continuous background workload — needs dedicated compute

---

#### Step 3 — High-Level Architecture (10-15 min)

```
                        ┌──────────────────────────────────────────────┐
                        │              Monitored Targets               │
                        │  (services, VMs, containers, databases)      │
                        └──────────────┬───────────────────────────────┘
                                       │
                              metrics exposed via
                              /metrics endpoint (pull)
                              or pushed via agent (push)
                                       │
                        ┌──────────────▼───────────────────────────────┐
                        │          Collection Layer                     │
                        │  ┌─────────────┐  ┌─────────────────────┐   │
                        │  │  Scrapers   │  │  Push Gateway /      │   │
                        │  │  (Pull)     │  │  Agents (Push)       │   │
                        │  └──────┬──────┘  └──────────┬──────────┘   │
                        └─────────┼────────────────────┼──────────────┘
                                  │                    │
                                  ▼                    ▼
                        ┌──────────────────────────────────────────────┐
                        │           Ingestion Pipeline                  │
                        │  ┌──────────────────────────────────────┐    │
                        │  │  Write-Ahead Log (WAL) + Distributor │    │
                        │  │  (validate, replicate, route by hash)│    │
                        │  └──────────────────┬───────────────────┘    │
                        └─────────────────────┼────────────────────────┘
                                              │
                    ┌─────────────────────────┼─────────────────────────┐
                    │                         │                         │
                    ▼                         ▼                         ▼
        ┌───────────────────┐   ┌───────────────────┐   ┌──────────────────────┐
        │   Ingester 1      │   │   Ingester 2      │   │   Ingester N         │
        │ (in-memory + WAL) │   │ (in-memory + WAL) │   │ (in-memory + WAL)    │
        └────────┬──────────┘   └────────┬──────────┘   └──────────┬───────────┘
                 │                       │                         │
                 │        flush chunks periodically                │
                 ▼                       ▼                         ▼
        ┌──────────────────────────────────────────────────────────────────────┐
        │                      Long-Term Storage                               │
        │  ┌─────────────┐  ┌──────────────────┐  ┌────────────────────────┐  │
        │  │  Hot Store   │  │  Warm Store       │  │  Cold Store (S3/GCS)  │  │
        │  │  (SSD-backed │  │  (Downsampled,    │  │  (Heavily downsampled │  │
        │  │   recent)    │  │   HDD/Object)     │  │   archival)           │  │
        │  └─────────────┘  └──────────────────┘  └────────────────────────┘  │
        └──────────────────────────────┬───────────────────────────────────────┘
                                       │
                    ┌──────────────────┼────────────────────────┐
                    │                  │                        │
                    ▼                  ▼                        ▼
        ┌────────────────┐  ┌──────────────────┐  ┌──────────────────────────┐
        │  Query Engine  │  │  Alert Evaluator │  │  Compactor /             │
        │  (PromQL-like) │  │  (Rule Engine)   │  │  Downsampler             │
        └───────┬────────┘  └────────┬─────────┘  └──────────────────────────┘
                │                    │
                ▼                    ▼
        ┌────────────────┐  ┌──────────────────────────────────────────────┐
        │  Dashboard /   │  │           Alert Manager                      │
        │  API Server    │  │  (dedup, group, silence, route, escalate)    │
        │  (Grafana-like)│  └────────┬──────────┬──────────┬───────────────┘
        └───────┬────────┘           │          │          │
                │                    ▼          ▼          ▼
                ▼              ┌────────┐ ┌────────┐ ┌──────────┐
        ┌────────────────┐    │ Email  │ │ Slack  │ │PagerDuty │
        │  Web UI        │    └────────┘ └────────┘ └──────────┘
        │  (Dashboards)  │
        └────────────────┘
```

##### Key Components

| Component | Responsibility |
|-----------|----------------|
| **Scrapers / Agents** | Pull metrics from targets at regular intervals (pull model) or receive pushed metrics (push model) |
| **Distributor** | Receives incoming samples, validates them, and routes to the correct ingester using consistent hashing on the metric's label set |
| **Ingester** | Holds recent data in memory (backed by WAL for durability), organizes samples into chunks, periodically flushes to long-term storage |
| **Long-Term Storage** | Tiered storage: hot (SSD, full resolution), warm (downsampled), cold (object storage like S3) |
| **Query Engine** | Parses and executes queries (like PromQL), fans out to ingesters (recent data) and long-term storage (older data), merges results |
| **Alert Evaluator** | Continuously evaluates alert rules against the query engine at defined intervals |
| **Alert Manager** | Receives fired alerts from the evaluator; handles deduplication, grouping, silencing, routing, and escalation |
| **Dashboard / API Server** | Serves the web UI, manages dashboard definitions, user auth, and data source configuration |
| **Compactor / Downsampler** | Background process that merges small chunks into larger ones and creates downsampled views for older data |

---

#### Step 4 — Deep Dives (15-20 min)

Pick 2-3 of the following depending on where the interviewer wants to go.

---

##### Deep Dive A: Time-Series Storage Engine

This is the heart of the system and the most technically interesting component.

**The challenge:** We need to write millions of data points per second while supporting fast range queries over time.

**Data Model:**

```
metric_name{label1="value1", label2="value2"} <float64_value> <timestamp>

Example:
http_requests_total{method="GET", handler="/api/v1/query", status="200"} 1027 1700000000
```

Each unique combination of metric name + labels = one **time series**. A time series is an append-only sequence of (timestamp, value) pairs.

**Storage Format — Chunked Time-Series:**

```
┌─────────────────────────────────────────────────┐
│ Time Series: http_requests_total{method="GET"}  │
├─────────────────────────────────────────────────┤
│  Chunk 1 (2h block)     │  Chunk 2 (2h block)  │
│  ┌─────────────────┐    │  ┌─────────────────┐  │
│  │ t0, v0          │    │  │ t480, v480      │  │
│  │ t1, v1          │    │  │ t481, v481      │  │
│  │ ...             │    │  │ ...             │  │
│  │ t479, v479      │    │  │ t959, v959      │  │
│  └─────────────────┘    │  └─────────────────┘  │
└─────────────────────────────────────────────────┘
```

**Compression — Gorilla Encoding (from Facebook's paper):**

Time-series data is highly compressible because:
- Timestamps arrive at regular intervals → store **delta-of-deltas** (usually 0)
- Values change slowly → XOR consecutive values, leading bits are often zero

Result: **~1.37 bytes per sample** (down from 16 bytes), a ~12x compression ratio.

**Inverted Index for Labels:**

To efficiently answer queries like `http_requests_total{status="500", region="us-east"}`, maintain an inverted index:

```
Label: status="500"    → [series_id_17, series_id_42, series_id_89, ...]
Label: region="us-east" → [series_id_17, series_id_23, series_id_42, ...]

Intersection → [series_id_17, series_id_42]
```

Implementation: sorted posting lists with intersection via merge join.

**Write Path:**

```
1. Sample arrives at Distributor
2. Distributor hashes the series labels → determines target Ingester(s)
3. Ingester appends sample to in-memory head chunk + WAL
4. When head chunk reaches 2 hours of data, it is "cut" and compressed
5. Compressed chunk is flushed to long-term object storage (S3/GCS)
6. Compactor periodically merges small blocks into larger ones
```

**Read Path:**

```
1. Query arrives at Query Frontend (optional caching/splitting layer)
2. Query Frontend splits the time range into sub-queries
3. Recent data (last few hours) → fetched from Ingesters (in-memory)
4. Older data → fetched from Long-Term Storage (block files on S3)
5. Results are merged, deduplicated, and returned
```

**Trade-off discussion points:**

| Decision | Trade-off |
|----------|-----------|
| **Pull vs Push model** | Pull is simpler for the target (just expose /metrics), easier to detect if a target is down. Push is necessary for short-lived jobs and firewalled environments. Support both. |
| **In-memory ingesters** | Fast writes and queries for recent data, but ingesters are stateful. Mitigate with WAL + replication (write to N ingesters). |
| **Object storage for long-term** | Infinitely scalable and cheap, but higher query latency. Mitigate with an index cache and chunk cache (memcached/Redis). |
| **2-hour block size** | Balances between write amplification (too small = too many files) and recovery time (too large = slow WAL replay). |

---

##### Deep Dive B: Alerting Pipeline

**Requirements for alerting:**

- Alert rules are evaluated at regular intervals (every 15-60 seconds)
- An alert transitions through states: `inactive → pending → firing → resolved`
- "Pending" requires the condition to be true for a **configurable duration** (the "for" clause) before firing — avoids flapping
- Fired alerts must be deduplicated, grouped, and routed to the right notification channel

**Alert Rule Example:**

```yaml
alert: HighErrorRate
expr: rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m]) > 0.05
for: 10m
labels:
  severity: critical
annotations:
  summary: "Error rate above 5% for 10 minutes on {{ $labels.instance }}"
```

**Alert Evaluation Architecture:**

```
┌────────────────────────────────────────────────────────────┐
│                    Alert Evaluator                          │
│                                                            │
│  ┌──────────────────┐    ┌──────────────────────────────┐  │
│  │  Rule Scheduler  │───▶│  Rule Group 1 (every 30s)    │  │
│  │  (per-group      │    │  - HighCPU                   │  │
│  │   intervals)     │    │  - HighMemory                │  │
│  │                  │    │  - DiskAlmostFull            │  │
│  │                  │    └──────────────────────────────┘  │
│  │                  │    ┌──────────────────────────────┐  │
│  │                  │───▶│  Rule Group 2 (every 60s)    │  │
│  │                  │    │  - HighErrorRate              │  │
│  │                  │    │  - HighLatency                │  │
│  │                  │    └──────────────────────────────┘  │
│  └──────────────────┘                                      │
│           │                                                │
│           ▼  fires query against Query Engine               │
│  ┌──────────────────┐                                      │
│  │  State Tracker   │  (tracks inactive/pending/firing)    │
│  └────────┬─────────┘                                      │
│           │  when state = firing                            │
│           ▼                                                │
│  ┌──────────────────┐                                      │
│  │  Send to Alert   │                                      │
│  │  Manager         │                                      │
│  └──────────────────┘                                      │
└────────────────────────────────────────────────────────────┘
```

**Alert Manager Pipeline:**

```
Incoming Fired Alert
        │
        ▼
┌───────────────┐     Alerts with same group_by labels are
│   Grouping    │     batched together (e.g., group by cluster, namespace)
└───────┬───────┘
        │
        ▼
┌───────────────┐     Suppress if matches an active silence rule
│   Silencing   │     (e.g., during maintenance window)
└───────┬───────┘
        │
        ▼
┌───────────────┐     Don't re-send if same alert already sent recently
│ Deduplication │     (inhibition: critical alert suppresses warning for same issue)
└───────┬───────┘
        │
        ▼
┌───────────────┐     Route to correct channel based on labels:
│   Routing     │     severity=critical → PagerDuty
│               │     severity=warning  → Slack #alerts-warning
│               │     team=payments     → Slack #payments-oncall
└───────┬───────┘
        │
        ▼
┌───────────────┐     Wait (e.g., 5 min) to batch related alerts
│   Throttling  │     into a single notification
└───────┬───────┘
        │
        ▼
┌───────────────┐
│  Notification │──▶  Email, Slack, PagerDuty, Webhook, OpsGenie
│  Dispatcher   │
└───────────────┘
```

**High availability for alerting:**

Alerting must be the most reliable part of the system. If dashboards are down, engineers lose visibility. If alerting is down, incidents go undetected.

| Strategy | Implementation |
|----------|----------------|
| **Replicated evaluation** | Run N alert evaluator replicas, each evaluating all rules. The Alert Manager deduplicates identical alerts from different replicas. |
| **Alert Manager clustering** | Run Alert Manager in a cluster using a gossip protocol (like Memberlist) to synchronize silences and notification state. Any node can receive alerts; the cluster ensures only one notification is sent. |
| **Separate failure domain** | Alert evaluation should not depend on the dashboard/API server. If the UI is down, alerts must still fire. |
| **Watchdog alert** | A "dead man's switch" alert that is always firing. If PagerDuty stops receiving this alert, it means the alerting pipeline itself is broken. |

---

##### Deep Dive C: Query Engine and Dashboard Performance

**Query Language (PromQL-like):**

```
# Rate of HTTP errors over the last 5 minutes
rate(http_requests_total{status=~"5.."}[5m])

# 99th percentile latency by handler
histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m]))

# Ratio of errors to total requests
sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m]))
```

**Query execution flow:**

```
1. Parse query → AST (Abstract Syntax Tree)
2. Plan execution: identify time range, resolution (step), and needed series
3. Fetch series data:
   a. Recent data → query Ingesters via gRPC
   b. Historical data → query Store Gateway (reads blocks from object storage)
4. Evaluate functions (rate, sum, histogram_quantile) on fetched data
5. Return result matrix to caller
```

**Optimizations for dashboard performance:**

| Optimization | Description |
|--------------|-------------|
| **Query Frontend** | A stateless proxy in front of the query engine that splits long-range queries into smaller sub-queries, caches results, and enforces query limits |
| **Results caching** | Cache query results keyed by (query, time_range, step). For aligned time ranges, reuse cached results and only compute the delta |
| **Query parallelism** | Split a query across time ranges or label shards and execute sub-queries in parallel |
| **Index caching** | Cache the series label index in memcached to avoid repeated reads from object storage |
| **Chunk caching** | Cache recently accessed data chunks in memcached |
| **Query limits** | Enforce maximum series fetched, maximum time range, and maximum query execution time to prevent a single query from starving others |
| **Recording rules** | Pre-compute expensive queries on a schedule and store the result as a new time series. Dashboards query the pre-computed series instead of running the expensive query on every load. |

**Trade-off: Recording rules vs. query-time computation:**

Recording rules reduce dashboard latency but introduce staleness (data is only as fresh as the last evaluation) and increase storage (one more time series to store). They are essential for high-cardinality aggregations that are queried frequently.

---

##### Deep Dive D: Multi-Tenancy

For a SaaS monitoring product (like Grafana Cloud or Datadog), multi-tenancy is a first-class concern.

**Tenant isolation layers:**

| Layer | How |
|-------|-----|
| **Ingestion** | Every write request includes a tenant ID in the HTTP header. The distributor validates and routes accordingly. |
| **Storage** | Option A: Shared storage with tenant ID as a prefix in the object key (e.g., `s3://metrics/{tenant_id}/block_001`). Option B: Separate storage accounts per tenant (stronger isolation, higher operational cost). |
| **Query** | Query engine injects a tenant filter on every query — a tenant can never read another tenant's data. |
| **Limits** | Per-tenant rate limits, series limits, query concurrency limits, and retention policies. |
| **Fair scheduling** | Use a per-tenant queue with fair scheduling to prevent a noisy tenant from monopolizing query resources. |

---

#### Step 5 — Scaling, Reliability, and Operational Concerns (5-10 min)

##### Scaling Strategy

| Component | Scaling Approach |
|-----------|-----------------|
| **Scrapers** | Stateless; horizontally scale by assigning target subsets (use hashmod or service discovery) |
| **Distributor** | Stateless; scale behind a load balancer |
| **Ingester** | Stateful (in-memory data); scale via consistent hash ring. Adding/removing ingesters triggers a rebalance of series ownership |
| **Long-term storage** | Object storage (S3/GCS) is infinitely scalable |
| **Query engine** | Stateless; scale horizontally. Add query frontend for caching and splitting |
| **Alert evaluator** | Shard rules across evaluator instances (by rule group) or run fully replicated (simpler, with dedup at Alert Manager) |
| **Alert Manager** | Cluster mode with gossip protocol; scale to 3-5 nodes for HA |

##### Failure Modes and Mitigations

| Failure | Impact | Mitigation |
|---------|--------|------------|
| **Ingester crash** | Loss of in-memory data since last flush | WAL replay on restart; replicate writes to N ingesters (typically N=3, read quorum=2) |
| **Object storage unavailable** | Cannot query historical data, cannot flush chunks | Ingesters buffer in memory and extend chunk duration; query recent data from ingesters. Retry flush with exponential backoff |
| **Scrape target down** | Missing data points | `up` metric reports 0 → triggers alert. Stale marker written for disappeared series |
| **Query overload** | Slow dashboards, cascading timeouts | Query frontend rate limiting, per-tenant concurrency limits, circuit breakers, query timeouts |
| **Alert evaluator down** | Missed alerts | Run replicated evaluators; dead man's switch (watchdog alert) detects pipeline failure |
| **Network partition** | Split-brain between ingesters | Consistent hashing with replication; read from quorum to tolerate minority failures |
| **Cardinality explosion** | A bad label (e.g., user_id as a label) creates millions of series, exhausting memory | Per-tenant series limits, label validation at ingestion, cardinality analysis tooling, active series tracking |

##### Monitoring the Monitoring System (Meta-Monitoring)

This is a critical operational concern and a great staff-level talking point.

- **Separate meta-monitoring cluster:** A small, independent monitoring instance that watches the primary monitoring system. It should be as simple as possible (minimal dependencies).
- **Watchdog alerts:** An alert that always fires. If the on-call stops receiving it, the pipeline is broken.
- **Internal metrics:** The monitoring system exposes its own metrics (ingestion rate, query latency, WAL size, flush failures) that are scraped by the meta-monitoring cluster.
- **Out-of-band health checks:** External synthetic checks (e.g., from a different cloud provider) that write a known metric and verify it can be queried.

##### SLOs for the Monitoring System

| SLI (Indicator) | SLO (Objective) |
|-----------------|-----------------|
| Ingestion success rate | 99.9% of samples successfully stored |
| Query latency (p99) | < 2 seconds for queries over last 24h |
| Alert evaluation latency | Alert fires within 60 seconds of condition being true |
| Dashboard load time (p95) | < 3 seconds |
| Notification delivery | 99.95% of notifications delivered within 5 minutes |

---

#### Data Model

##### Metric Storage (Time-Series Database)

```
Series Key (hash of labels):
  __name__="http_requests_total", method="GET", handler="/api", status="200"

Chunks:
  [t0..t0+2h]: compressed (timestamp[], value[])  →  stored as block in S3
  [t0+2h..t0+4h]: compressed (timestamp[], value[])
  ...
```

##### Inverted Index

```
Table: series_index
┌───────────────┬────────────────────┬────────────────────────────┐
│ label_pair    │ series_ids (sorted)│ block_id                   │
├───────────────┼────────────────────┼────────────────────────────┤
│ status="200"  │ [1, 4, 7, 12, ...] │ block_2024_01_15_00_02    │
│ method="GET"  │ [1, 2, 4, 7, ...]  │ block_2024_01_15_00_02    │
└───────────────┴────────────────────┴────────────────────────────┘
```

##### Alert Rule Storage (Relational DB — PostgreSQL)

```sql
CREATE TABLE alert_rules (
    id              UUID PRIMARY KEY,
    tenant_id       UUID NOT NULL,
    name            VARCHAR(255) NOT NULL,
    expression      TEXT NOT NULL,           -- PromQL expression
    for_duration    INTERVAL NOT NULL,       -- e.g., '10 minutes'
    severity        VARCHAR(50) NOT NULL,    -- critical, warning, info
    labels          JSONB,                   -- additional routing labels
    annotations     JSONB,                   -- summary, description templates
    evaluation_interval INTERVAL DEFAULT '30 seconds',
    enabled         BOOLEAN DEFAULT TRUE,
    created_at      TIMESTAMPTZ NOT NULL,
    updated_at      TIMESTAMPTZ NOT NULL
);

CREATE TABLE alert_states (
    rule_id         UUID REFERENCES alert_rules(id),
    series_hash     BIGINT NOT NULL,         -- identifies which series triggered
    state           VARCHAR(20) NOT NULL,    -- inactive, pending, firing
    started_at      TIMESTAMPTZ,
    fired_at        TIMESTAMPTZ,
    resolved_at     TIMESTAMPTZ,
    last_eval_at    TIMESTAMPTZ NOT NULL,
    PRIMARY KEY (rule_id, series_hash)
);

CREATE TABLE notification_channels (
    id              UUID PRIMARY KEY,
    tenant_id       UUID NOT NULL,
    name            VARCHAR(255) NOT NULL,
    type            VARCHAR(50) NOT NULL,    -- slack, email, pagerduty, webhook
    config          JSONB NOT NULL,          -- channel-specific configuration
    created_at      TIMESTAMPTZ NOT NULL
);

CREATE TABLE silences (
    id              UUID PRIMARY KEY,
    tenant_id       UUID NOT NULL,
    matchers        JSONB NOT NULL,          -- label matchers to silence
    starts_at       TIMESTAMPTZ NOT NULL,
    ends_at         TIMESTAMPTZ NOT NULL,
    created_by      VARCHAR(255) NOT NULL,
    comment         TEXT,
    created_at      TIMESTAMPTZ NOT NULL
);
```

##### Dashboard Storage (PostgreSQL)

```sql
CREATE TABLE dashboards (
    id              UUID PRIMARY KEY,
    tenant_id       UUID NOT NULL,
    uid             VARCHAR(40) UNIQUE NOT NULL,   -- human-friendly unique ID
    title           VARCHAR(255) NOT NULL,
    description     TEXT,
    panels          JSONB NOT NULL,                -- array of panel definitions
    variables       JSONB,                         -- template variables
    time_range      JSONB,                         -- default time range
    refresh_interval VARCHAR(20),
    version         INTEGER NOT NULL DEFAULT 1,
    created_by      UUID NOT NULL,
    updated_at      TIMESTAMPTZ NOT NULL,
    created_at      TIMESTAMPTZ NOT NULL
);
```

---

#### API Design

##### Metric Ingestion

```
POST /api/v1/push
Headers:
  X-Tenant-ID: tenant_123
  Content-Type: application/x-protobuf
  Content-Encoding: snappy

Body: Protobuf-encoded WriteRequest {
  timeseries: [
    {
      labels: [{name: "__name__", value: "http_requests_total"}, {name: "method", value: "GET"}],
      samples: [{timestamp_ms: 1700000000000, value: 1027.0}]
    }
  ]
}

Response: 200 OK  (or 429 Too Many Requests if rate limited)
```

##### Scrape (Pull Model)

```
GET /metrics  (on the target)

Response (text/plain, Prometheus exposition format):
  # HELP http_requests_total Total HTTP requests
  # TYPE http_requests_total counter
  http_requests_total{method="GET",status="200"} 1027
  http_requests_total{method="POST",status="500"} 3
```

##### Query

```
GET /api/v1/query_range
Params:
  query=rate(http_requests_total{status="500"}[5m])
  start=2024-01-15T00:00:00Z
  end=2024-01-15T01:00:00Z
  step=15s

Response: {
  "status": "success",
  "data": {
    "resultType": "matrix",
    "result": [
      {
        "metric": {"__name__": "http_requests_total", "status": "500", "method": "GET"},
        "values": [[1705276800, "0.5"], [1705276815, "0.7"], ...]
      }
    ]
  }
}
```

##### Alert Rules CRUD

```
POST   /api/v1/rules           -- create alert rule
GET    /api/v1/rules           -- list alert rules
GET    /api/v1/rules/{id}      -- get alert rule
PUT    /api/v1/rules/{id}      -- update alert rule
DELETE /api/v1/rules/{id}      -- delete alert rule
GET    /api/v1/alerts          -- list currently firing alerts
POST   /api/v1/silences        -- create silence
DELETE /api/v1/silences/{id}   -- remove silence
```

---

#### Summary: What Makes This a Staff-Level Answer

| Signal | How This Solution Demonstrates It |
|--------|-----------------------------------|
| **Structured approach** | Followed the framework: requirements → estimation → architecture → deep dives → operations |
| **Trade-off articulation** | Pull vs push, recording rules vs query-time, shared vs isolated tenant storage, replication factor trade-offs |
| **Depth** | Detailed time-series storage engine with compression, inverted index, chunking strategy |
| **Breadth** | Covered ingestion, storage, querying, alerting, dashboards, multi-tenancy |
| **Operational maturity** | Meta-monitoring, watchdog alerts, SLOs/SLIs, cardinality explosion handling |
| **Failure mode analysis** | Addressed ingester crash, storage unavailability, cardinality bombs, query overload |
| **Business alignment** | Multi-tenancy design, per-tenant limits, cost-efficient tiered storage |
| **Evolution** | Tiered storage allows independent scaling of hot/warm/cold; recording rules allow trading freshness for performance |

---

#### Common Follow-Up Questions

| Question | Key Points to Discuss |
|----------|-----------------------|
| "How would you add log aggregation?" | Separate ingestion path optimized for text (inverted index on log fields, Elasticsearch or ClickHouse for storage). Correlate logs with metrics using shared labels (service, instance, trace_id). |
| "How would you add distributed tracing?" | Ingest spans (trace_id, span_id, parent_span_id, service, operation, duration). Store in a column-oriented database. Link traces to metrics via exemplars (a metric sample annotated with a trace_id). |
| "How would you handle 100x the current scale?" | Shard ingesters more aggressively, use dedicated read/write paths, heavier use of recording rules, aggressive downsampling, query result caching. |
| "How would you migrate from a monolithic Prometheus to this distributed system?" | Run both in parallel, dual-write (or remote-write from Prometheus to the new system), gradually shift dashboards and alert rules, validate parity before cutover. |
| "How do you prevent alert storms?" | Alert Manager grouping (batch related alerts), inhibition rules (critical suppresses warnings for same issue), rate limiting notifications per channel, escalation policies with increasing delay. |

---

*Good system design is not about memorizing architectures. It is about developing the judgment to make the right trade-offs for the right context.*
