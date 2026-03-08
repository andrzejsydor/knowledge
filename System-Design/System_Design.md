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
    - [Solution 2: Design a URL Shortener (bit.ly)](#solution-2-design-a-url-shortener-bitly)
    - [Solution 3: Design a Rate Limiter](#solution-3-design-a-rate-limiter)
    - [Solution 4: Design a Key-Value Store](#solution-4-design-a-key-value-store)
    - [Solution 5: Design a Notification System](#solution-5-design-a-notification-system)
    - [Solution 6: Design a Paste Service (Pastebin)](#solution-6-design-a-paste-service-pastebin)

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

### Solution 2: Design a URL Shortener (bit.ly)

> **Difficulty:** Intermediate (Senior Warm-Up)
> **Time allocation:** 45 minutes
> **Real-world references:** bit.ly, TinyURL, t.co, goo.gl

---

#### Opening Statement

> "A URL shortener seems simple on the surface, but it touches on several interesting design decisions: ID generation at scale, redirect latency optimization, and analytics. Let me clarify the scope before designing."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Shorten URL** | Given a long URL, generate a unique short URL |
| FR2 | **Redirect** | When a user visits the short URL, redirect to the original long URL |
| FR3 | **Custom aliases** | Optionally allow users to pick a custom short code |
| FR4 | **Expiration** | URLs can have an optional TTL (time-to-live) |
| FR5 | **Analytics** | Track click count, referrer, geo, device for each short URL |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Read-heavy** | 100:1 read-to-write ratio |
| NFR2 | **Low latency** | Redirects in < 50ms (p99) |
| NFR3 | **High availability** | 99.99% uptime (redirects must never go down) |
| NFR4 | **Scale** | 100M new URLs/month, 10B redirects/month |
| NFR5 | **Durability** | Once created, a short URL must always work (until it expires) |

---

#### Step 2 — Back-of-the-Envelope Estimation

| Metric | Calculation | Value |
|--------|-------------|-------|
| Write QPS | 100M / (30 * 86,400) | ~40 URLs/sec |
| Read QPS (average) | 40 * 100 | ~4,000 redirects/sec |
| Read QPS (peak) | 4,000 * 5 | ~20,000 redirects/sec |
| Storage per URL | short code (7B) + long URL (avg 200B) + metadata (100B) = ~300 bytes | |
| Storage / year | 100M * 12 * 300 bytes | ~360 GB/year |
| Storage / 5 years | | ~1.8 TB |
| Short code keyspace | Base62, 7 characters → 62^7 | ~3.5 trillion (more than enough) |

**Design implication:** Reads dominate. The system must be optimized for ultra-fast lookups. Data fits comfortably in a single database with replication, but caching is critical for latency.

---

#### Step 3 — High-Level Architecture

```
┌──────────┐       ┌──────────────┐       ┌──────────────────┐
│  Client   │──────▶│  Load        │──────▶│  API Service     │
│ (Browser) │◀──────│  Balancer    │◀──────│  (Stateless)     │
└──────────┘       └──────────────┘       └────────┬─────────┘
   301/302                                         │
   redirect                         ┌──────────────┼──────────────┐
                                    │              │              │
                                    ▼              ▼              ▼
                              ┌──────────┐  ┌──────────┐  ┌─────────────┐
                              │  Cache   │  │ Database │  │  Analytics  │
                              │  (Redis) │  │ (NoSQL)  │  │  Queue      │
                              └──────────┘  └──────────┘  │  (Kafka)    │
                                                          └──────┬──────┘
                                                                 │
                                                                 ▼
                                                          ┌─────────────┐
                                                          │  Analytics  │
                                                          │  DB / OLAP  │
                                                          └─────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Short Code Generation

This is the core design challenge. Three main approaches:

**Approach 1: Hash + Truncate**

```
long_url → MD5/SHA256 → take first 7 characters (Base62 encoded)
```

| Pros | Cons |
|------|------|
| Deterministic: same URL always gets same code | Hash collisions (must check DB and retry with salt) |
| No coordination needed | Not truly random — if input is predictable, output is guessable |

**Approach 2: Counter-Based (Pre-generated IDs)**

```
Auto-increment counter → Base62 encode → short code
Counter: 1000000001 → Base62: "1LY7VK"
```

| Pros | Cons |
|------|------|
| No collisions guaranteed | Single counter = single point of failure |
| Predictable, sequential codes | Sequential codes are guessable (security concern) |

Scale solution: Use **range-based ID allocation**. A coordination service (ZooKeeper or a DB row) assigns ID ranges to each application server:

```
Server A gets range [1M, 2M)
Server B gets range [2M, 3M)
...each server increments locally, no coordination on each write
```

**Approach 3: Snowflake-style ID (Recommended)**

```
┌──────────┬───────────┬──────────┬──────────┐
│ timestamp │ machine_id│ sequence │          │
│ (41 bits) │ (10 bits) │ (12 bits)│          │
└──────────┴───────────┴──────────┴──────────┘
64-bit ID → Base62 encode → 7-character short code
```

| Pros | Cons |
|------|------|
| No collision, no coordination | Slightly more complex to implement |
| Roughly time-sorted | IDs are still somewhat guessable |
| Distributed by design | Requires clock synchronization |

**Recommendation:** Use Snowflake-style IDs for the default path and allow custom aliases (validated for uniqueness) as an optional feature.

##### Deep Dive B: Read Path (Redirect Optimization)

The redirect path is called 100x more than the write path. Every millisecond matters.

```
GET /abc1234

1. Check local in-process cache (LRU) → hit? return 301
2. Check Redis cache → hit? return 301, populate local cache
3. Check database → found? return 301, populate Redis + local cache
4. Not found → return 404
```

**301 vs 302 redirect:**

| Code | Meaning | Implication |
|------|---------|-------------|
| 301 | Permanent redirect | Browser caches it — fewer requests to our servers, but we lose analytics visibility |
| 302 | Temporary redirect | Browser always comes back to us — we can track every click |

**Recommendation:** Use 302 if analytics matter; 301 if you want to minimize server load. Most URL shorteners use 302.

**Cache strategy:**

- **Cache-aside** pattern: check cache first, fall back to DB, write result to cache
- TTL on cache entries: 24 hours (most clicks happen in the first few hours after sharing)
- Hot URLs (viral content) will naturally stay in cache via LRU
- Cache hit ratio target: > 95% (the Pareto principle — a small % of URLs get most traffic)

##### Deep Dive C: Database Choice

| Option | Pros | Cons |
|--------|------|------|
| **DynamoDB / Cassandra** | Fast key-value lookups, horizontally scalable, high write throughput | No complex queries (fine for this use case) |
| **PostgreSQL** | ACID, flexible queries for analytics | Harder to scale horizontally |
| **MongoDB** | Flexible schema, decent performance | Less predictable latency at extreme scale |

**Recommendation:** DynamoDB or Cassandra. The access pattern is purely key-value (short_code → long_url). No joins, no complex queries needed.

**Schema:**

```
Table: urls
┌──────────────┬───────────────────────────────────────────┬─────────┬─────────────┬──────────┐
│ short_code   │ long_url                                  │ user_id │ created_at  │ expires_at│
│ (PK)         │                                           │         │             │ (TTL)    │
├──────────────┼───────────────────────────────────────────┼─────────┼─────────────┼──────────┤
│ "abc1234"    │ "https://example.com/very/long/path?..."  │ "u_123" │ 2024-01-15  │ null     │
│ "xyz9876"    │ "https://another.com/article/..."         │ "u_456" │ 2024-01-15  │ 2024-07  │
└──────────────┴───────────────────────────────────────────┴─────────┴─────────────┴──────────┘
```

For detecting duplicate long URLs (optional), add a **secondary index** or a separate lookup table:

```
Table: url_lookup
┌──────────────────────┬──────────────┐
│ url_hash (PK)        │ short_code   │
│ SHA256(long_url)     │              │
├──────────────────────┼──────────────┤
│ "a1b2c3d4..."        │ "abc1234"    │
└──────────────────────┴──────────────┘
```

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **Read scaling** | Redis cluster + read replicas of the database |
| **Write scaling** | Range-based ID allocation eliminates write contention; DynamoDB auto-scales |
| **Hot URLs** | Redis handles hot keys naturally; add CDN for extremely viral URLs |
| **Expiration cleanup** | DynamoDB TTL auto-deletes expired items; or a nightly batch job |
| **Availability** | Multi-AZ deployment; redirect path depends only on cache + DB (no external deps) |
| **Analytics pipeline** | Async: log clicks to Kafka → consume into ClickHouse/BigQuery for analytics dashboards |
| **Abuse prevention** | Rate limit URL creation per user/IP; scan URLs against blocklists for malicious content |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How do you handle a URL that goes viral (billions of clicks)?" | CDN caching the redirect at the edge; Redis handles the rest. The short code doesn't change, so caching is trivial. |
| "How would you support custom domains?" | Store domain + short code as the composite key. Tenant provides a CNAME record pointing to our infrastructure. |
| "How would you prevent abuse?" | Rate limiting, URL content scanning (Google Safe Browsing API), CAPTCHA on creation, abuse reporting. |
| "What if the database goes down?" | Cache serves reads (high hit ratio). Writes can be buffered in a queue. Alert and recover. |

---

### Solution 3: Design a Rate Limiter

> **Difficulty:** Intermediate (Senior Warm-Up)
> **Time allocation:** 45 minutes
> **Real-world references:** API gateways (Kong, Envoy), Cloudflare, Stripe API rate limiting

---

#### Opening Statement

> "Rate limiting is a critical defense mechanism for any API-facing service. It protects against abuse, prevents resource exhaustion, and ensures fair usage. Let me clarify what kind of rate limiter we're designing — is this a standalone service, middleware, or built into an API gateway?"

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Limit requests** | Throttle requests that exceed a configured rate (e.g., 100 req/min per user) |
| FR2 | **Multiple rules** | Support different limits per endpoint, per user tier, per API key |
| FR3 | **Informative response** | Return HTTP 429 with headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset` |
| FR4 | **Rule configuration** | Rules can be updated without restarting the service |
| FR5 | **Distributed** | Rate limiting must work across multiple API server instances (not per-server) |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Latency** | < 1ms overhead per request (rate limiting must be nearly invisible) |
| NFR2 | **Accuracy** | Small over-allowance is acceptable; must never severely under-limit |
| NFR3 | **Availability** | If the rate limiter is unavailable, fail open (allow requests) rather than blocking all traffic |
| NFR4 | **Scale** | Handle 1M+ requests per second across all API servers |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| API servers | 100 instances |
| Total QPS | 1,000,000 |
| Unique rate limit keys (users/API keys) | 10 million |
| Memory per counter | ~100 bytes (key + counter + window metadata) |
| Total memory for all counters | 10M * 100B = ~1 GB |

**Design implication:** The entire counter set fits in memory. Redis is a natural choice for the centralized counter store.

---

#### Step 3 — High-Level Architecture

```
┌──────────┐       ┌────────────────────────────────────────────────┐
│  Client   │──────▶│  API Server                                    │
└──────────┘       │  ┌──────────────────────────────────────────┐  │
                   │  │  Rate Limiter Middleware                  │  │
                   │  │  (check before processing the request)   │  │
                   │  └──────────────┬───────────────────────────┘  │
                   │                 │                               │
                   │         ┌───────▼────────┐                     │
                   │         │  Local Cache   │ (optional,          │
                   │         │  (in-process)  │  for recently       │
                   │         └───────┬────────┘  denied keys)       │
                   │                 │                               │
                   └─────────────────┼───────────────────────────────┘
                                     │
                              ┌──────▼──────┐
                              │   Redis     │  (centralized
                              │   Cluster   │   counter store)
                              └──────┬──────┘
                                     │
                              ┌──────▼──────┐
                              │   Rules     │  (rate limit
                              │   Config DB │   configuration)
                              └─────────────┘
```

**Where to place the rate limiter:**

| Option | Pros | Cons |
|--------|------|------|
| **API Gateway / Load Balancer** | Centralized, catches traffic early | Less flexibility for per-endpoint rules |
| **Middleware in each API server** | Fine-grained control per route, easy to customize | Every server must coordinate via shared store |
| **Standalone sidecar service** | Decoupled, reusable across services | Extra network hop |

**Recommendation:** Middleware approach with Redis as the shared backend. This gives per-route flexibility while keeping the architecture simple.

---

#### Step 4 — Deep Dives

##### Deep Dive A: Rate Limiting Algorithms

**Algorithm 1: Token Bucket (Recommended for most use cases)**

```
Bucket: { tokens: float, last_refill: timestamp, capacity: int, refill_rate: float }

On each request:
  1. Calculate tokens to add since last_refill:
     tokens_to_add = (now - last_refill) * refill_rate
  2. tokens = min(capacity, tokens + tokens_to_add)
  3. last_refill = now
  4. If tokens >= 1: tokens -= 1, ALLOW
     Else: REJECT (429)
```

| Pros | Cons |
|------|------|
| Allows short bursts up to bucket capacity | Requires storing two values per key (tokens + timestamp) |
| Smooth average rate over time | Slightly more complex than fixed window |
| Memory efficient | |

**Algorithm 2: Fixed Window Counter**

```
Key: "user_123:2024-01-15T10:05" (1-minute window)
On each request:
  1. INCR key
  2. If count > limit: REJECT
  3. Set TTL on key = window_size
```

| Pros | Cons |
|------|------|
| Simple to implement (single Redis INCR) | Boundary spike: 100 requests at 10:04:59 + 100 at 10:05:00 = 200 in 2 seconds |
| Low memory | Uneven distribution near window boundaries |

**Algorithm 3: Sliding Window Log**

```
Store timestamp of every request in a sorted set:
  1. Remove all entries older than (now - window_size)
  2. Count remaining entries
  3. If count < limit: add new entry, ALLOW
     Else: REJECT
```

| Pros | Cons |
|------|------|
| Most accurate — no boundary issues | High memory: storing every request timestamp |
| Perfectly smooth window | Expensive: O(N) cleanup per check |

**Algorithm 4: Sliding Window Counter (Best balance)**

```
Weighted count from two adjacent fixed windows:
  current_window_count * weight + previous_window_count * (1 - weight)
  weight = elapsed_time_in_current_window / window_size

Example (limit=100/min, checking at 10:05:40):
  Previous window (10:04-10:05): 80 requests
  Current window  (10:05-10:06): 30 requests so far
  Weight = 40/60 = 0.67
  Estimate = 30 * 0.67 + 80 * (1 - 0.67) = 20.1 + 26.4 = 46.5
  46.5 < 100 → ALLOW
```

| Pros | Cons |
|------|------|
| Good accuracy without storing every timestamp | Approximate (but very close in practice) |
| Only 2 counters per key per window | Slightly more complex than fixed window |
| Memory efficient | |

**Recommendation:** Token Bucket for general API rate limiting (allows bursts); Sliding Window Counter when you need strict per-window limits.

##### Deep Dive B: Distributed Rate Limiting with Redis

**Atomic operations are critical.** A naive check-then-increment has a race condition:

```
BAD (race condition):
  count = GET key           ← two servers read "99"
  if count < 100:
    INCR key                ← both increment to 100, but 101 requests were allowed
```

**Solution: Redis Lua script for atomicity**

```lua
-- Token Bucket implemented as atomic Redis Lua script
local key = KEYS[1]
local capacity = tonumber(ARGV[1])
local refill_rate = tonumber(ARGV[2])  -- tokens per second
local now = tonumber(ARGV[3])

local bucket = redis.call('HMGET', key, 'tokens', 'last_refill')
local tokens = tonumber(bucket[1]) or capacity
local last_refill = tonumber(bucket[2]) or now

-- Refill tokens
local elapsed = now - last_refill
tokens = math.min(capacity, tokens + elapsed * refill_rate)

if tokens >= 1 then
    tokens = tokens - 1
    redis.call('HMSET', key, 'tokens', tokens, 'last_refill', now)
    redis.call('EXPIRE', key, math.ceil(capacity / refill_rate) * 2)
    return {1, tokens}  -- allowed, remaining
else
    redis.call('HMSET', key, 'tokens', tokens, 'last_refill', now)
    return {0, 0}       -- rejected, remaining
end
```

This entire script executes atomically in Redis — no race conditions.

##### Deep Dive C: Handling Failures

| Failure | Behavior |
|---------|----------|
| **Redis is down** | **Fail open** — allow all requests. Better to temporarily lose rate limiting than to block all users. Alert the team. |
| **Redis is slow** | Set a strict timeout (5-10ms). On timeout, fall back to a local in-process counter (approximate, per-server) |
| **Clock skew between servers** | Use Redis server time (`redis.call('TIME')`) inside the Lua script instead of client-provided time |
| **Network partition to Redis** | Same as Redis down — fail open with local fallback |

##### Deep Dive D: Multi-Tier Rate Limiting

Real-world systems apply rate limits at multiple levels:

```
┌─────────────────────────────────────────────────────────┐
│ Tier 1: Global rate limit (per IP)       1000 req/min  │
│ Tier 2: Per-user rate limit              100 req/min   │
│ Tier 3: Per-endpoint rate limit          20 req/min    │
│ Tier 4: Per-user-per-endpoint            10 req/min    │
└─────────────────────────────────────────────────────────┘
Request must pass ALL tiers to be allowed.
```

**Rule storage:**

```yaml
rules:
  - key: "ip:{client_ip}"
    limit: 1000
    window: 60s
  - key: "user:{user_id}"
    limit: 100
    window: 60s
  - key: "endpoint:{method}:{path}"
    limit: 5000
    window: 60s
  - key: "user_endpoint:{user_id}:{method}:{path}"
    limit: 10
    window: 60s
```

---

#### Step 5 — API and Response Headers

```
HTTP/1.1 429 Too Many Requests
Content-Type: application/json
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1705276860
Retry-After: 30

{
  "error": {
    "code": "rate_limit_exceeded",
    "message": "Rate limit of 100 requests per minute exceeded. Try again in 30 seconds."
  }
}
```

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How do you rate limit in a multi-region setup?" | Option A: Global Redis (high latency). Option B: Per-region rate limiting (faster, but total across regions can exceed the limit). Option C: Eventual consistency — sync counters between regions asynchronously. |
| "How do you handle rate limiting for a free vs paid tier?" | Look up the user's tier, apply different rule sets. Cache the tier in the token/session to avoid a DB lookup per request. |
| "What if a single user uses multiple API keys?" | Rate limit per user ID (not per API key). Also apply per-IP limits as a secondary defense. |
| "How would you do rate limiting without Redis?" | Local in-memory counters with a token bucket per server. Divide the global limit by server count. Less accurate but zero network dependency. |

---

### Solution 4: Design a Key-Value Store

> **Difficulty:** Intermediate (Senior Warm-Up)
> **Time allocation:** 45 minutes
> **Real-world references:** Redis, Memcached, DynamoDB, etcd, Riak

---

#### Opening Statement

> "A key-value store is one of the most fundamental building blocks in distributed systems. The design space is huge — it ranges from a simple in-memory cache to a fully distributed, persistent, strongly consistent store. Let me understand what part of that spectrum we're targeting."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **put(key, value)** | Store a key-value pair |
| FR2 | **get(key)** | Retrieve the value for a given key |
| FR3 | **delete(key)** | Remove a key-value pair |
| FR4 | **TTL support** | Keys can have an optional expiration time |
| FR5 | **Replicated** | Data is replicated across multiple nodes for durability |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Latency** | < 10ms for get/put (p99) |
| NFR2 | **Throughput** | 100K+ operations per second per node |
| NFR3 | **Availability** | AP system — favor availability over strong consistency (tunable) |
| NFR4 | **Scale** | Data size exceeds a single machine (must partition/shard) |
| NFR5 | **Durability** | Data persisted to disk; survives node crashes |
| NFR6 | **Key size** | Up to 256 bytes |
| NFR7 | **Value size** | Up to 1 MB |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Total data size | 10 TB |
| Average key-value pair | 1 KB |
| Total entries | ~10 billion |
| Nodes (16 GB RAM each) | ~50 nodes (data + replicas) |
| Replication factor | 3 |
| Read/Write QPS per node | 100K |

---

#### Step 3 — High-Level Architecture

```
┌──────────┐      ┌────────────────────────────────────────────────────┐
│  Client   │─────▶│  Client Library / SDK                              │
│           │◀─────│  (consistent hashing, handles routing + retries)   │
└──────────┘      └──────────────────┬─────────────────────────────────┘
                                     │
                    Knows which node owns the key
                    via consistent hash ring
                                     │
                  ┌──────────────────┼──────────────────┐
                  │                  │                  │
                  ▼                  ▼                  ▼
           ┌────────────┐    ┌────────────┐    ┌────────────┐
           │   Node A   │    │   Node B   │    │   Node C   │
           │            │    │            │    │            │
           │ ┌────────┐ │    │ ┌────────┐ │    │ ┌────────┐ │
           │ │MemTable│ │    │ │MemTable│ │    │ │MemTable│ │
           │ └───┬────┘ │    │ └───┬────┘ │    │ └───┬────┘ │
           │     │      │    │     │      │    │     │      │
           │ ┌───▼────┐ │    │ ┌───▼────┐ │    │ ┌───▼────┐ │
           │ │  WAL   │ │    │ │  WAL   │ │    │ │  WAL   │ │
           │ └───┬────┘ │    │ └───┬────┘ │    │ └───┬────┘ │
           │     │      │    │     │      │    │     │      │
           │ ┌───▼────┐ │    │ ┌───▼────┐ │    │ ┌───▼────┐ │
           │ │SSTables│ │    │ │SSTables│ │    │ │SSTables│ │
           │ │ (disk) │ │    │ │ (disk) │ │    │ │ (disk) │ │
           │ └────────┘ │    │ └────────┘ │    │ └────────┘ │
           └────────────┘    └────────────┘    └────────────┘
                  │                  │                  │
                  └──────── Replication (gossip) ───────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Data Partitioning — Consistent Hashing

**Problem:** We have 10 TB of data and need to distribute it across ~50 nodes. When nodes join or leave, we want to move as little data as possible.

**Consistent hashing with virtual nodes:**

```
Hash ring: 0 ────────────────────────────────── 2^128

Physical nodes mapped to multiple virtual nodes:
  Node A: hash("A-vn0")=100, hash("A-vn1")=4500, hash("A-vn2")=8900, ...
  Node B: hash("B-vn0")=350, hash("B-vn1")=5100, hash("B-vn2")=7200, ...
  Node C: hash("C-vn0")=800, hash("C-vn1")=3200, hash("C-vn2")=9500, ...

Key "user_123" → hash = 420 → walks clockwise → lands on Node B (vn0 at 5100)
```

Each physical node has 100-200 virtual nodes for even distribution. When a node is added, it only takes over a fraction of keys from its neighbors.

**Replication:** For replication factor N=3, a key is stored on the N nodes found by walking clockwise from its position. Ensure the N nodes are on different physical machines (skip virtual nodes on the same physical host).

##### Deep Dive B: Storage Engine — LSM Tree

The Log-Structured Merge Tree (LSM) is used by Cassandra, RocksDB, LevelDB, and HBase.

**Write path (optimized for fast writes):**

```
put("user_123", "{name: 'Alice'}")
        │
        ▼
┌──────────────┐
│ 1. Write to  │  (append-only log on disk, for crash recovery)
│    WAL       │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ 2. Write to  │  (sorted in-memory balanced tree: red-black or skip list)
│    MemTable  │
└──────┬───────┘
       │  when MemTable reaches threshold (e.g., 64 MB)
       ▼
┌──────────────┐
│ 3. Flush to  │  (immutable sorted file on disk)
│    SSTable   │
└──────────────┘
```

**Read path:**

```
get("user_123")
        │
        ▼
┌──────────────┐
│ 1. MemTable  │  → found? return
└──────┬───────┘
       │ miss
       ▼
┌──────────────┐
│ 2. Bloom     │  → for each SSTable (newest first):
│    Filters   │     bloom filter says "definitely not here" → skip
└──────┬───────┘     bloom filter says "maybe here" → check SSTable
       │
       ▼
┌──────────────┐
│ 3. SSTable   │  → binary search in the sparse index, read data block
│    Lookup    │
└──────────────┘
```

**Compaction:** Background process that merges SSTables, removes deleted keys (tombstones), and reduces read amplification.

| Compaction Strategy | Description | Best For |
|---------------------|-------------|----------|
| **Size-tiered** | Merge SSTables of similar size | Write-heavy workloads |
| **Leveled** | Organize SSTables into levels with size limits | Read-heavy workloads (fewer files to check) |

##### Deep Dive C: Replication and Consistency

**Tunable consistency with quorum:**

```
N = number of replicas (e.g., 3)
W = write quorum (how many replicas must acknowledge a write)
R = read quorum (how many replicas must respond to a read)

Strong consistency: W + R > N
  Example: N=3, W=2, R=2 → at least one node has the latest value

Eventual consistency: W=1, R=1
  Fastest, but may read stale data
```

| Configuration | Consistency | Latency | Use Case |
|---------------|-------------|---------|----------|
| W=3, R=1 | Strong for reads after write confirms | Slow writes, fast reads | Read-heavy, consistency matters |
| W=1, R=3 | Reads always get latest | Fast writes, slow reads | Write-heavy |
| W=2, R=2 | Balanced | Moderate | General purpose |
| W=1, R=1 | Eventual | Fastest | Caching, non-critical data |

**Conflict resolution (when writes happen concurrently to different replicas):**

| Strategy | How It Works |
|----------|-------------|
| **Last-write-wins (LWW)** | Highest timestamp wins. Simple but may lose data. Used by Cassandra. |
| **Vector clocks** | Track causal history per key. Detect conflicts and let the application resolve. Used by Riak/DynamoDB. |
| **CRDTs** | Conflict-free data types that merge automatically (counters, sets). No conflicts by design. |

##### Deep Dive D: Failure Detection and Handling

**Gossip protocol** for membership and failure detection:

```
Every 1 second, each node:
  1. Picks a random peer
  2. Sends its membership list (node → heartbeat counter)
  3. Peer merges the list (higher heartbeat wins)
  4. If a node's heartbeat hasn't incremented for T seconds → mark "suspected down"
  5. If confirmed down by multiple nodes → mark "failed"
```

**Handling node failures:**

| Technique | Description |
|-----------|-------------|
| **Hinted handoff** | If a replica is down, another node temporarily stores writes for it. When it comes back, the stored hints are replayed. Handles short outages. |
| **Anti-entropy (Merkle trees)** | Periodically compare Merkle tree hashes of key ranges between replicas. If hashes differ, sync only the diverged ranges. Handles long outages and data drift. |
| **Read repair** | During a read, if replicas return different values, update the stale ones. Passive background consistency. |

---

#### Step 5 — Summary Table

| Component | Design Choice | Rationale |
|-----------|---------------|-----------|
| Partitioning | Consistent hashing + virtual nodes | Even distribution, minimal data movement on scaling |
| Storage engine | LSM tree (WAL + MemTable + SSTables) | Optimized for high write throughput |
| Replication | Quorum-based, tunable N/W/R | Flexible consistency vs latency trade-off |
| Conflict resolution | Vector clocks (or LWW for simplicity) | Detect and resolve concurrent writes |
| Failure detection | Gossip protocol | Decentralized, scalable |
| Failure recovery | Hinted handoff + Merkle tree anti-entropy | Short and long outage recovery |
| Reads | Bloom filters + sparse index + block cache | Minimize disk I/O |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How would you support range queries?" | Ordered partitioning (range-based sharding instead of hash-based). Trade-off: risk of hot partitions. |
| "How is this different from Redis?" | Redis is primarily in-memory; this design uses disk-backed LSM trees for large datasets. Redis offers richer data structures (lists, sets, sorted sets). |
| "How would you add transactions?" | Add a coordination layer (two-phase commit or Raft-based consensus per partition). Significantly increases complexity and latency. |
| "How would you handle a 100 MB value?" | Chunk the value and store chunks separately. Store a manifest under the original key pointing to chunk keys. |

---

### Solution 5: Design a Notification System

> **Difficulty:** Intermediate (Senior Warm-Up)
> **Time allocation:** 45 minutes
> **Real-world references:** Firebase Cloud Messaging, Amazon SNS, Twilio, SendGrid

---

#### Opening Statement

> "A notification system can mean many things — push notifications, SMS, email, in-app notifications. The core challenge is building a reliable, multi-channel delivery system that handles millions of notifications per day with priority, user preferences, and retry logic. Let me clarify the scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Multi-channel delivery** | Push notification (iOS/Android), SMS, email, in-app |
| FR2 | **User preferences** | Users can opt-in/out per channel and per notification type |
| FR3 | **Template system** | Notifications use templates with variable substitution |
| FR4 | **Priority levels** | Critical (OTP, security alerts), high, normal, low |
| FR5 | **Scheduling** | Support "send at" for scheduled notifications |
| FR6 | **Delivery tracking** | Track sent, delivered, opened, failed for each notification |
| FR7 | **Rate limiting** | Don't spam users — limit notifications per user per channel per hour |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Throughput** | 10 million notifications/day (peak: 1,000/sec) |
| NFR2 | **Latency** | Critical notifications delivered in < 5 seconds; normal < 30 seconds |
| NFR3 | **Reliability** | At-least-once delivery — a notification must never be silently dropped |
| NFR4 | **Availability** | 99.9% uptime |
| NFR5 | **Exactly-once from user perspective** | Deduplication to prevent duplicate pushes/SMS |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Notifications/day | 10 million |
| Average QPS | ~115/sec |
| Peak QPS | ~1,000/sec |
| Channel split | Push: 50%, Email: 30%, In-app: 15%, SMS: 5% |
| Notification record size | ~500 bytes |
| Storage/day | 10M * 500B = ~5 GB |
| Storage/year | ~1.8 TB |

---

#### Step 3 — High-Level Architecture

```
┌──────────────────────┐
│  Upstream Services    │  (order service, auth service, marketing, etc.)
│  trigger: "notify     │
│  user X about Y"      │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  Notification API    │  (validate, authenticate, rate limit)
│  Service             │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  Priority Queue      │  (Kafka / SQS with priority topics)
│  ┌────┐ ┌────┐      │
│  │ P0 │ │ P1 │ ...  │  P0 = critical, P1 = high, P2 = normal
│  └────┘ └────┘      │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────────────────────────────────────────────┐
│  Notification Processor (Workers)                            │
│                                                              │
│  1. Fetch user preferences   → skip if opted out             │
│  2. Rate limit check         → defer if over limit           │
│  3. Render template          → fill in variables             │
│  4. Deduplicate              → check if already sent         │
│  5. Route to channel handler                                 │
│                                                              │
│  ┌──────────────┬──────────────┬──────────────┬───────────┐  │
│  │ Push Handler │ Email Handler│ SMS Handler  │ In-App    │  │
│  │              │              │              │ Handler   │  │
│  └──────┬───────┴──────┬───────┴──────┬───────┴─────┬─────┘  │
└─────────┼──────────────┼──────────────┼─────────────┼────────┘
          │              │              │             │
          ▼              ▼              ▼             ▼
   ┌────────────┐ ┌────────────┐ ┌──────────┐ ┌──────────────┐
   │ APNs/FCM   │ │ SendGrid / │ │ Twilio / │ │ Notification │
   │ (Push)     │ │ SES (Email)│ │ Vonage   │ │ Store (DB)   │
   └────────────┘ └────────────┘ └──────────┘ └──────────────┘
                                                     │
                                               ┌─────▼─────┐
                                               │ In-App    │
                                               │ via WS /  │
                                               │ SSE / Poll│
                                               └───────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Reliability — At-Least-Once Delivery

The biggest risk in a notification system is **silently dropping notifications**. A user misses a 2FA code or a fraud alert, and trust is broken.

**Delivery state machine:**

```
CREATED → QUEUED → PROCESSING → SENT → DELIVERED → READ
                        │                    │
                        ▼                    ▼
                     FAILED ──▶ RETRY ──▶ SENT
                        │
                     (after max retries)
                        ▼
                   PERMANENTLY_FAILED
```

**Every state transition is persisted to the database.** If a worker crashes mid-processing, the message is re-delivered from the queue (at-least-once semantics via Kafka consumer offsets or SQS visibility timeout).

**Retry strategy per channel:**

| Channel | Retry Strategy | Max Retries | Backoff |
|---------|----------------|-------------|---------|
| Push (APNs/FCM) | Retry on timeout, 5xx from provider | 3 | 1s, 5s, 30s |
| Email (SES/SendGrid) | Retry on 429, 5xx | 5 | 10s, 30s, 2m, 10m, 1h |
| SMS (Twilio) | Retry on 5xx; do NOT retry on invalid number | 2 | 5s, 30s |
| In-App | Write to DB; client pulls — inherently reliable | N/A | N/A |

##### Deep Dive B: User Preferences and Rate Limiting

**Preferences model:**

```
Table: user_notification_preferences
┌──────────┬─────────────────────┬─────────┬──────────┬──────────┐
│ user_id  │ notification_type   │ push    │ email    │ sms      │
├──────────┼─────────────────────┼─────────┼──────────┼──────────┤
│ u_123    │ order_update        │ true    │ true     │ false    │
│ u_123    │ marketing           │ false   │ true     │ false    │
│ u_123    │ security_alert      │ true    │ true     │ true     │  ← cannot opt out
└──────────┴─────────────────────┴─────────┴──────────┴──────────┘
```

Some notification types are **mandatory** (security alerts, legal notices) — users cannot opt out.

**Rate limiting per user:**

```
Rules:
  - Max 3 push notifications per hour per user
  - Max 10 emails per day per user
  - Max 2 SMS per day per user
  - Critical notifications bypass rate limits

Key in Redis: "ratelimit:push:u_123:2024-01-15T10"
              → INCR, check against limit
```

When rate-limited, the notification is either dropped (marketing) or deferred to the next window (transactional).

##### Deep Dive C: Template Engine

```json
{
  "template_id": "order_shipped",
  "channels": {
    "push": {
      "title": "Your order has shipped!",
      "body": "Order #{{order_id}} is on its way. Track: {{tracking_url}}"
    },
    "email": {
      "subject": "Order #{{order_id}} Shipped",
      "html_template": "order_shipped.html",
      "variables": ["order_id", "tracking_url", "delivery_date", "customer_name"]
    },
    "sms": {
      "body": "Order #{{order_id}} shipped. Track at {{tracking_url}}"
    }
  }
}
```

Templates are stored in a database, cached in-memory, and versioned. Changes are propagated to workers via a config change event.

##### Deep Dive D: In-App Notification Delivery

Unlike push/email/SMS, in-app notifications are pulled by the client.

```
Table: in_app_notifications
┌──────────┬──────────┬─────────────────────────────┬──────────┬──────────┐
│ user_id  │ id       │ content                     │ read     │ created  │
│ (PK)     │ (SK)     │                             │          │          │
├──────────┼──────────┼─────────────────────────────┼──────────┼──────────┤
│ u_123    │ n_001    │ "Your order shipped..."     │ false    │ 10:00    │
│ u_123    │ n_002    │ "New message from Alice"    │ false    │ 10:05    │
└──────────┴──────────┴─────────────────────────────┴──────────┴──────────┘
```

**Real-time delivery options:**

| Method | How | Trade-off |
|--------|-----|-----------|
| **Long polling** | Client polls every N seconds | Simple, but wastes resources if nothing new |
| **Server-Sent Events (SSE)** | Server pushes over HTTP/1.1 | Unidirectional, simple, good for notifications |
| **WebSocket** | Full duplex persistent connection | Best latency, but higher server resource cost |

**Recommendation:** WebSocket for apps that already have a real-time connection (chat, live updates); SSE for simpler notification-only use cases.

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **Throughput** | Kafka partitions (partition by user_id for ordering); scale workers horizontally |
| **Priority handling** | Separate Kafka topics per priority level; critical topics have more consumer instances |
| **Third-party outages** | Circuit breaker per provider; fallback channel (email down → retry email later, don't switch to SMS unless configured) |
| **Duplicate prevention** | Idempotency key per notification (event_type + user_id + entity_id + timestamp window). Check against a dedup store (Redis SET with TTL) before sending |
| **Delivery confirmation** | APNs/FCM provide delivery receipts; email has delivery/bounce webhooks; SMS has delivery reports. Update notification status in DB. |
| **Analytics** | Stream delivery events to a data warehouse for open rates, click rates, channel effectiveness |

---

#### Data Model

```sql
CREATE TABLE notifications (
    id              UUID PRIMARY KEY,
    user_id         UUID NOT NULL,
    type            VARCHAR(50) NOT NULL,
    channel         VARCHAR(20) NOT NULL,
    priority        SMALLINT NOT NULL DEFAULT 2,
    template_id     VARCHAR(100),
    variables       JSONB,
    status          VARCHAR(20) NOT NULL DEFAULT 'created',
    idempotency_key VARCHAR(255) UNIQUE,
    scheduled_at    TIMESTAMPTZ,
    sent_at         TIMESTAMPTZ,
    delivered_at    TIMESTAMPTZ,
    read_at         TIMESTAMPTZ,
    failed_reason   TEXT,
    retry_count     SMALLINT DEFAULT 0,
    created_at      TIMESTAMPTZ NOT NULL
);

CREATE INDEX idx_notifications_user_status ON notifications(user_id, status);
CREATE INDEX idx_notifications_scheduled ON notifications(scheduled_at) WHERE status = 'created';
```

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How do you handle millions of push notifications for a broadcast?" | Fan-out on write: pre-generate a notification per user and enqueue. Or fan-out on read: store once, deliver when the user opens the app. For push, fan-out on write is required (you must call APNs/FCM per device). Batch API calls to FCM (up to 500 per request). |
| "How do you handle timezone-aware scheduling?" | Store `scheduled_at` in UTC. A scheduler job periodically scans for notifications where `scheduled_at <= now()` and enqueues them. |
| "What if we need to recall a sent notification?" | In-app: mark as deleted in DB. Push: can't un-push. Email: can't un-send. Best you can do is send a follow-up correction. For in-app, support a `revoked` status. |
| "How do you measure notification effectiveness?" | Track funnel: sent → delivered → opened → clicked (for email) or sent → delivered → tapped (for push). Report per notification type and channel. |

---

### Solution 6: Design a Paste Service (Pastebin)

> **Difficulty:** Intermediate (Senior Warm-Up)
> **Time allocation:** 45 minutes
> **Real-world references:** Pastebin, GitHub Gist, Hastebin, Ghostbin

---

#### Opening Statement

> "A paste service is conceptually similar to a URL shortener, but instead of redirecting, we're storing and serving content. The interesting design challenges are handling potentially large text content, access control, syntax highlighting, and content moderation. Let me confirm the scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Create paste** | User submits text content, gets a unique URL |
| FR2 | **Read paste** | Anyone with the URL can view the paste (raw or formatted) |
| FR3 | **Syntax highlighting** | Auto-detect or user-specified language for highlighting |
| FR4 | **Expiration** | Pastes can expire after a configurable duration (10 min, 1 hour, 1 day, never) |
| FR5 | **Visibility** | Public (listed), unlisted (accessible via link only), private (requires auth) |
| FR6 | **Edit / Delete** | Authenticated users can edit or delete their own pastes |
| FR7 | **Raw view** | Serve the raw text without formatting (for `curl` / scripting) |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Read-heavy** | 5:1 read-to-write ratio |
| NFR2 | **Low latency** | Read a paste in < 200ms (p99) |
| NFR3 | **Max paste size** | 10 MB |
| NFR4 | **Availability** | 99.9% |
| NFR5 | **Scale** | 1 million new pastes/day |
| NFR6 | **Retention** | Non-expiring pastes stored indefinitely |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| New pastes/day | 1 million |
| Write QPS (avg) | ~12/sec |
| Read QPS (avg) | ~60/sec |
| Read QPS (peak) | ~300/sec |
| Average paste size | 10 KB |
| Storage/day | 1M * 10 KB = ~10 GB |
| Storage/year | ~3.6 TB |
| Storage/5 years | ~18 TB |
| Metadata per paste | ~500 bytes |
| Metadata storage/year | 1M * 365 * 500B = ~183 GB |

**Design implication:** The content (text) should be stored separately from the metadata. Content goes to object storage (cheap, scalable); metadata goes to a database (fast lookups, indexing).

---

#### Step 3 — High-Level Architecture

```
┌──────────┐        ┌──────────────┐        ┌────────────────────┐
│  Client   │───────▶│  Load        │───────▶│  API Service       │
│ (Browser/ │◀───────│  Balancer    │◀───────│  (Stateless)       │
│  curl)    │        └──────────────┘        └────────┬───────────┘
└──────────┘                                          │
                                       ┌──────────────┼──────────────┐
                                       │              │              │
                                       ▼              ▼              ▼
                                ┌──────────┐  ┌──────────────┐  ┌──────────┐
                                │  Cache   │  │  Metadata DB │  │  Object  │
                                │  (Redis) │  │  (Postgres)  │  │  Storage │
                                └──────────┘  └──────────────┘  │  (S3)    │
                                                                └──────────┘
                                                                      │
                                                                ┌─────▼──────┐
                                                                │    CDN     │
                                                                │(CloudFront)│
                                                                └────────────┘

             ┌───────────────────────────────────────────────────────┐
             │  Background Workers                                   │
             │  - Expiration cleanup (delete expired pastes)         │
             │  - Content moderation (scan for malware / abuse)      │
             │  - Syntax detection (auto-detect language)            │
             └───────────────────────────────────────────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Paste ID Generation and Storage Design

**Paste ID:** Same approach as URL shortener — Base62-encoded Snowflake ID or random 8-character string.

```
Paste URL: https://paste.example.com/Xk9mP2aQ
                                      ^^^^^^^^
                                      8-char Base62 ID
                                      62^8 = 218 trillion combinations
```

**Metadata (PostgreSQL):**

```sql
CREATE TABLE pastes (
    id              VARCHAR(12) PRIMARY KEY,
    title           VARCHAR(255),
    language        VARCHAR(50),
    visibility      VARCHAR(10) NOT NULL DEFAULT 'unlisted',
    user_id         UUID,
    content_key     VARCHAR(255) NOT NULL,
    size_bytes      INTEGER NOT NULL,
    expires_at      TIMESTAMPTZ,
    burn_after_read BOOLEAN DEFAULT FALSE,
    created_at      TIMESTAMPTZ NOT NULL,
    updated_at      TIMESTAMPTZ NOT NULL
);

CREATE INDEX idx_pastes_user ON pastes(user_id, created_at DESC);
CREATE INDEX idx_pastes_expires ON pastes(expires_at) WHERE expires_at IS NOT NULL;
CREATE INDEX idx_pastes_public ON pastes(created_at DESC) WHERE visibility = 'public';
```

**Content (S3/Object Storage):**

```
Bucket: paste-content
Key:    {paste_id}/{version}
        e.g., "Xk9mP2aQ/1"

Content-Type: text/plain
Content-Encoding: gzip (compress before storing)
```

**Why separate metadata from content?**

| Concern | Why Separation Helps |
|---------|---------------------|
| **Cost** | Storing 10 MB text blobs in PostgreSQL is expensive and slows queries. S3 is 10-100x cheaper per GB. |
| **Performance** | Metadata queries (list user's pastes, check expiration) don't need to load content. |
| **CDN caching** | Content served from S3 can be fronted by CloudFront. Metadata doesn't need a CDN. |
| **Size limits** | PostgreSQL rows have practical size limits; S3 handles multi-MB objects natively. |

##### Deep Dive B: Read Path Optimization

```
GET /api/pastes/Xk9mP2aQ

1. Check Redis cache (key: "paste:Xk9mP2aQ")
   → Cache HIT: return metadata + content from cache
   → Cache MISS: continue

2. Query PostgreSQL for metadata
   → Not found? → 404
   → Expired? → 410 Gone (and enqueue async cleanup)
   → Burn-after-read? → return content, then delete

3. Fetch content from S3 (via CDN if public)
   → Decompress (gzip)

4. Populate Redis cache:
   - Small pastes (< 100 KB): cache the full content
   - Large pastes (> 100 KB): cache metadata only, content served from S3/CDN

5. Return response
   - Browser request → render with syntax highlighting (server-side or client-side)
   - curl / raw request → return plain text
```

**Content-Type negotiation:**

```
GET /Xk9mP2aQ                    → HTML with syntax-highlighted content
GET /Xk9mP2aQ/raw                → text/plain
GET /api/v1/pastes/Xk9mP2aQ      → JSON metadata + content
```

##### Deep Dive C: Expiration and Cleanup

**Two-phase approach:**

1. **Lazy expiration (on read):** When a paste is read and `expires_at < now()`, return 410 and enqueue a delete task. This handles the common case with zero background cost.

2. **Active cleanup (background job):** A scheduled job runs every few minutes:

```sql
SELECT id, content_key FROM pastes
WHERE expires_at IS NOT NULL
  AND expires_at < NOW() - INTERVAL '1 hour'
LIMIT 1000;
```

For each batch: delete from S3, then delete from PostgreSQL, then invalidate cache.

**Why the 1-hour buffer?** Gives lazy expiration a chance to handle reads first. Avoids races where a user is viewing a paste right at its expiration time.

**Burn-after-read:**

```
1. Acquire a row-level lock: SELECT ... FOR UPDATE
2. Read content from S3
3. Delete from S3
4. Delete from PostgreSQL
5. Invalidate cache
6. Return content to user

(Steps 1-5 ensure exactly-once read even with concurrent requests)
```

##### Deep Dive D: Syntax Highlighting

| Approach | Where | Pros | Cons |
|----------|-------|------|------|
| **Server-side** (Pygments, highlight.js via SSR) | Server renders HTML | Works without JavaScript, better for SEO/sharing | Higher server CPU cost |
| **Client-side** (highlight.js, Prism.js) | Browser renders | Zero server cost, richer interactivity | Requires JavaScript, slower first paint for large files |
| **Hybrid** | Server detects language; client highlights | Best of both: fast detection, rich rendering | More complexity |

**Recommendation:** Client-side highlighting with server-side language detection. The server returns the raw text with a `language` hint; the JavaScript library does the rest.

**Language detection:**

1. User-specified language (highest priority)
2. File extension hint (if provided)
3. Auto-detection using heuristics (shebang line, keywords, statistical analysis)

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **Read scaling** | CDN for public pastes (content from S3 is easily CDN-cacheable). Redis cache for metadata. Read replicas of PostgreSQL. |
| **Write scaling** | Writes are low QPS (~12/sec). Single PostgreSQL primary is sufficient for years. |
| **Large pastes** | S3 handles multi-MB objects. For >10 MB, reject at the API layer (413 Payload Too Large). |
| **Abuse / Spam** | Rate limit paste creation per IP and per user. Content scanning for malware, phishing links, and illegal content (async via a moderation worker). CAPTCHA for anonymous users. |
| **Storage cost** | Compress content (gzip → ~5x reduction for text). Lifecycle policies on S3: move expired content to cheaper storage tiers before deletion. |
| **Availability** | Stateless API servers behind a load balancer. Redis cluster mode. PostgreSQL with streaming replication and automatic failover. S3 is inherently highly available (99.99%). |
| **Backup** | PostgreSQL: daily automated backups. S3: enable versioning and cross-region replication for critical data. |

---

#### API Design

```
POST /api/v1/pastes
Body: {
  "content": "def hello():\n    print('hello world')",
  "title": "My Script",
  "language": "python",
  "visibility": "unlisted",
  "expires_in": "1h"
}
Response: 201 Created
{
  "id": "Xk9mP2aQ",
  "url": "https://paste.example.com/Xk9mP2aQ",
  "raw_url": "https://paste.example.com/Xk9mP2aQ/raw",
  "expires_at": "2024-01-15T11:00:00Z"
}

GET /api/v1/pastes/{id}
Response: 200 OK
{
  "id": "Xk9mP2aQ",
  "title": "My Script",
  "content": "def hello():\n    print('hello world')",
  "language": "python",
  "visibility": "unlisted",
  "size_bytes": 42,
  "created_at": "2024-01-15T10:00:00Z",
  "expires_at": "2024-01-15T11:00:00Z"
}

PUT /api/v1/pastes/{id}      (auth required, owner only)
DELETE /api/v1/pastes/{id}   (auth required, owner only)

GET /api/v1/users/{id}/pastes?page=1&limit=20  (list user's pastes)
```

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How would you add paste versioning (like Gist)?" | Store each version as a separate S3 object (`{id}/{version}`). Metadata table tracks versions. Diff computed between versions on read. |
| "How would you add collaborative editing?" | Operational Transformation or CRDTs (like Google Docs). This fundamentally changes the architecture — the paste becomes a real-time document. Significantly more complex. |
| "How would you add search across all public pastes?" | Index public paste content in Elasticsearch. Provide full-text search with language and date filters. Rebuild the index from S3 if needed. |
| "How do you prevent sensitive data leaks?" | Scan pastes for patterns (API keys, passwords, credit card numbers) using regex + ML. Flag or auto-expire matches. Offer client-side encryption for private pastes. |

---

*Good system design is not about memorizing architectures. It is about developing the judgment to make the right trade-offs for the right context.*
