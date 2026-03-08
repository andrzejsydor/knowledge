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
    - [Solution 1: Design a URL Shortener (bit.ly)](#solution-1-design-a-url-shortener-bitly)
    - [Solution 2: Design a Rate Limiter](#solution-2-design-a-rate-limiter)
    - [Solution 3: Design a Key-Value Store](#solution-3-design-a-key-value-store)
    - [Solution 4: Design a Notification System](#solution-4-design-a-notification-system)
    - [Solution 5: Design a Paste Service (Pastebin)](#solution-5-design-a-paste-service-pastebin)
    - [Solution 6: Design a News Feed System (Twitter / Facebook)](#solution-6-design-a-news-feed-system-twitter-facebook)
    - [Solution 7: Design a Chat System (WhatsApp / Slack)](#solution-7-design-a-chat-system-whatsapp-slack)
    - [Solution 8: Design a Web Crawler](#solution-8-design-a-web-crawler)
    - [Solution 9: Design a Video Streaming Platform (YouTube / Netflix)](#solution-9-design-a-video-streaming-platform-youtube-netflix)
    - [Solution 10: Design a Search Autocomplete System](#solution-10-design-a-search-autocomplete-system)
    - [Solution 11: Design a Ride-Sharing Service (Uber / Lyft)](#solution-11-design-a-ride-sharing-service-uber-lyft)
    - [Solution 12: Design a File Storage Service (Google Drive / Dropbox)](#solution-12-design-a-file-storage-service-google-drive-dropbox)
    - [Solution 13: Design a Distributed Message Queue (Kafka)](#solution-13-design-a-distributed-message-queue-kafka)
    - [Solution 14: Design a Monitoring and Alerting System (Grafana + Prometheus)](#solution-14-design-a-monitoring-and-alerting-system-grafana-prometheus)
    - [Solution 15: Design a Distributed Task Scheduler](#solution-15-design-a-distributed-task-scheduler)
    - [Solution 16: Design a Global-Scale Payment System](#solution-16-design-a-global-scale-payment-system)
    - [Solution 17: Design a Multi-Tenant SaaS Platform](#solution-17-design-a-multi-tenant-saas-platform)
    - [Solution 18: Design a Real-Time Collaborative Editor (Google Docs)](#solution-18-design-a-real-time-collaborative-editor-google-docs)
    - [Solution 19: Design a Distributed Cache (Redis Cluster)](#solution-19-design-a-distributed-cache-redis-cluster)
    - [Solution 20: Design a Search Engine (Google-scale)](#solution-20-design-a-search-engine-google-scale)

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

### Solution 1: Design a URL Shortener (bit.ly)

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

### Solution 2: Design a Rate Limiter

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

### Solution 3: Design a Key-Value Store

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

### Solution 4: Design a Notification System

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

### Solution 5: Design a Paste Service (Pastebin)

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

### Solution 6: Design a News Feed System (Twitter / Facebook)

> **Difficulty:** Advanced (Senior Level)
> **Time allocation:** 45 minutes
> **Real-world references:** Twitter timeline, Facebook News Feed, Instagram feed, LinkedIn feed

---

#### Opening Statement

> "A news feed is the core engagement surface for any social platform. The fundamental challenge is assembling a personalized, real-time feed for each user from the posts of all the people they follow — at massive scale. The key architectural decision is fan-out-on-write vs fan-out-on-read. Let me clarify scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Publish post** | A user creates a text/image/video post |
| FR2 | **Follow / Unfollow** | Users can follow other users |
| FR3 | **News feed** | A user sees a feed of posts from people they follow, in reverse-chronological or ranked order |
| FR4 | **Likes / Comments** | Users can like and comment on posts |
| FR5 | **Feed refresh** | New posts appear in real time (or near-real-time) |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Scale** | 500M DAU, average user follows 200 people |
| NFR2 | **Feed latency** | < 500ms to render the feed |
| NFR3 | **Publish latency** | A post is visible to followers within 5 seconds |
| NFR4 | **Availability** | 99.99% |
| NFR5 | **Read-heavy** | Read-to-write ratio ~100:1 |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| DAU | 500 million |
| Posts created/day | 200 million |
| Feed reads/day | 5 billion (user opens app ~10 times/day) |
| Feed read QPS (peak) | ~100,000 |
| Avg followers per user | 200 |
| Celebrity followers | up to 100 million |
| Post size (metadata) | ~1 KB |

---

#### Step 3 — High-Level Architecture

```
┌──────────┐         ┌─────────────────────────────────────────────┐
│   User   │────────▶│  API Gateway / Load Balancer                │
└──────────┘         └────────────┬────────────┬───────────────────┘
                                  │            │
                    ┌─────────────▼──┐  ┌──────▼──────────────────┐
                    │  Post Service  │  │  Feed Service           │
                    │  (create post) │  │  (read feed)            │
                    └──────┬─────────┘  └──────┬──────────────────┘
                           │                   │
                    ┌──────▼─────────┐  ┌──────▼──────────────────┐
                    │  Fan-out       │  │  Feed Cache             │
                    │  Service       │  │  (Redis: per-user feed) │
                    └──────┬─────────┘  └──────┬──────────────────┘
                           │                   │ (cache miss)
                   ┌───────┼───────┐           │
                   ▼       ▼       ▼    ┌──────▼──────────────────┐
             Write to   Write to  ...   │  Feed Generator         │
             follower   follower        │  (on-demand assembly)   │
             A's cache  B's cache       └─────────────────────────┘
                                               │
                              ┌─────────────────┼────────────────┐
                              ▼                 ▼                ▼
                        ┌──────────┐     ┌───────────┐    ┌──────────┐
                        │  Posts   │     │  Social   │    │  User    │
                        │  Store   │     │  Graph    │    │  Store   │
                        │(Cassandra│     │  (who     │    │(profiles)│
                        │ / MySQL) │     │  follows  │    │          │
                        └──────────┘     │  whom)    │    └──────────┘
                                         └───────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Fan-Out Strategy — The Core Decision

**Fan-out-on-write (push model):**

```
User A publishes a post
  → Fan-out service looks up A's followers: [B, C, D, ...]
  → Writes (post_id, timestamp) to each follower's feed cache
  → When B opens the app, the feed is pre-computed — just read from cache
```

| Pros | Cons |
|------|------|
| Feed reads are extremely fast (pre-computed) | High write amplification: celebrity with 100M followers = 100M writes |
| Simple read path | Wasted work if a follower is inactive |
| Feed is always ready | Slow fan-out for users with millions of followers |

**Fan-out-on-read (pull model):**

```
User B opens the app
  → Feed service fetches B's follow list: [A, C, E, ...]
  → For each followed user, fetch their recent posts
  → Merge, sort by time, return top N
```

| Pros | Cons |
|------|------|
| No write amplification | Slow: must query many sources in real time |
| No wasted work | Feed assembly is expensive at read time |
| Simple write path | Latency depends on how many users B follows |

**Hybrid approach (recommended — this is what Twitter and Facebook do):**

```
┌───────────────────────────────────────────────────────┐
│  Regular users (< 10K followers):  Fan-out-on-write  │
│  Celebrity users (> 10K followers): Fan-out-on-read   │
└───────────────────────────────────────────────────────┘

When B reads their feed:
  1. Fetch pre-computed feed from cache (posts from regular users)
  2. Fetch recent posts from celebrities B follows (fan-out-on-read)
  3. Merge both, rank, return top N
```

This avoids the "celebrity problem" while keeping reads fast for the vast majority of posts.

##### Deep Dive B: Feed Cache Design

```
Per-user feed in Redis (sorted set):

Key: feed:{user_id}
Value: sorted set of (post_id, timestamp_score)

ZADD feed:user_B 1705276800 post_123
ZADD feed:user_B 1705276900 post_456

Read feed: ZREVRANGEBYSCORE feed:user_B +inf -inf LIMIT 0 20
  → Returns 20 most recent post IDs

Fetch full post content: MGET post:123 post:456 ...
  (posts are cached separately by post_id)
```

**Cache size management:**
- Keep only the latest 500-1000 post IDs per user
- When the list grows beyond 1000, trim the oldest entries (`ZREMRANGEBYRANK`)
- Inactive users (haven't logged in for 30 days): evict their feed cache entirely. Rebuild on-demand when they return.

##### Deep Dive C: Post Storage and Social Graph

**Posts (Cassandra or sharded MySQL):**

```sql
CREATE TABLE posts (
    post_id     UUID PRIMARY KEY,
    user_id     UUID NOT NULL,
    content     TEXT,
    media_urls  TEXT[],
    like_count  INT DEFAULT 0,
    created_at  TIMESTAMPTZ NOT NULL
);

-- User timeline (all posts by a user, for profile view):
CREATE TABLE user_posts (
    user_id     UUID,
    created_at  TIMESTAMPTZ,
    post_id     UUID,
    PRIMARY KEY (user_id, created_at)
) WITH CLUSTERING ORDER BY (created_at DESC);
```

**Social graph (who follows whom):**

```sql
CREATE TABLE follows (
    follower_id  UUID,
    followee_id  UUID,
    created_at   TIMESTAMPTZ,
    PRIMARY KEY (follower_id, followee_id)
);

-- Reverse index for fan-out:
CREATE TABLE followers (
    followee_id  UUID,
    follower_id  UUID,
    PRIMARY KEY (followee_id, follower_id)
);
```

##### Deep Dive D: Feed Ranking (Beyond Chronological)

Modern feeds use ML-based ranking instead of pure reverse-chronological order.

```
For each candidate post in the feed:
  Score = ML_model(features)

Features:
  - Post age (freshness)
  - Author engagement score (how often B interacts with A's posts)
  - Post type (image/video posts get higher engagement)
  - Like/comment velocity (trending posts)
  - Content relevance (NLP-based topic matching)
  - Negative signals (B previously hid similar posts)

Top-K by score → returned as the feed
```

The ranking model is retrained periodically on engagement data (clicks, likes, time spent reading).

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **Fan-out storm** | Rate-limit fan-out per post. Use Kafka to buffer and process fan-outs asynchronously. |
| **Celebrity fan-out** | Hybrid model: don't fan-out for celebrities. Merge their posts at read time. |
| **Cache stampede** | When a popular user's feed cache is evicted, thousands of requests hit the DB. Use cache locking (one request rebuilds, others wait). |
| **Feed freshness** | WebSocket or long-poll for real-time updates. Client periodically polls for new posts count badge. |
| **Post deletions** | Remove from the posts store. Async job removes from all feed caches (eventually consistent — acceptable). |
| **Geo-distribution** | Replicate feed caches to multiple regions. Posts store uses cross-region replication with eventual consistency. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How do you handle a user with 100M followers posting?" | Don't fan-out. Mark user as "celebrity." Their posts are merged at read time for each follower. |
| "How do you handle real-time feed updates?" | WebSocket connection from client. When a fan-out writes to B's feed cache, push a notification to B's WebSocket. Client fetches new posts. |
| "How would you add ads to the feed?" | Ad server returns ad candidates. Feed service inserts ads at specific positions (e.g., every 5th item). Ads are ranked by bid price * relevance score. |
| "How would you add stories (ephemeral content)?" | Separate data path: stories expire after 24h. Store in Redis with TTL. Display in a separate UI carousel, not mixed into the main feed. |

---

### Solution 7: Design a Chat System (WhatsApp / Slack)

> **Difficulty:** Advanced (Senior Level)
> **Time allocation:** 45 minutes
> **Real-world references:** WhatsApp, Slack, Telegram, Discord, Facebook Messenger

---

#### Opening Statement

> "A chat system is fundamentally a real-time message delivery system. The key challenges are maintaining persistent connections with millions of users, guaranteeing message delivery and ordering, and supporting both 1:1 and group chat. Let me clarify the scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **1:1 messaging** | Two users can exchange messages in real time |
| FR2 | **Group chat** | Groups with up to 500 members |
| FR3 | **Online presence** | See if a contact is online/offline/last seen |
| FR4 | **Message status** | Sent, delivered, read indicators (like WhatsApp ticks) |
| FR5 | **Push notifications** | Notify offline users of new messages |
| FR6 | **Media** | Send images, videos, files |
| FR7 | **Message history** | Persistent chat history synced across devices |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Latency** | < 200ms message delivery (same region, both online) |
| NFR2 | **Scale** | 500M DAU, 50M concurrent connections |
| NFR3 | **Reliability** | Messages must never be lost (at-least-once delivery) |
| NFR4 | **Ordering** | Messages in a conversation are strictly ordered |
| NFR5 | **Availability** | 99.99% |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| DAU | 500 million |
| Concurrent connections | 50 million |
| Messages/day | 50 billion |
| Message QPS (average) | ~600,000 |
| Average message size | 200 bytes (text) |
| Storage/day | 50B * 200B = ~10 TB |
| WebSocket servers (50K connections each) | ~1,000 |

---

#### Step 3 — High-Level Architecture

```
┌─────────────┐   ┌─────────────┐
│  User A     │   │  User B     │
│  (sender)   │   │  (receiver) │
└──────┬──────┘   └──────▲──────┘
       │                 │
       │ WebSocket       │ WebSocket
       │                 │
┌──────▼──────┐   ┌──────┴──────┐
│  WS Server  │   │  WS Server  │
│  (holds A's │   │  (holds B's │
│  connection)│   │  connection)│
└──────┬──────┘   └──────▲──────┘
       │                 │
       ▼                 │
┌──────────────────────────────────────────────────────┐
│  Chat Service (Stateless)                            │
│                                                      │
│  1. Validate message                                 │
│  2. Persist to message store                         │
│  3. Look up B's connection → which WS server?        │
│  4. Route message to B's WS server                   │
│  5. If B offline → push notification                 │
└──────────┬──────────────────────┬────────────────────┘
           │                      │
    ┌──────▼──────┐       ┌──────▼──────────┐
    │  Message    │       │  Session /       │
    │  Store      │       │  Presence        │
    │  (Cassandra)│       │  Service (Redis) │
    └─────────────┘       └─────────────────┘
           │
    ┌──────▼──────────┐
    │  Message Queue  │  (for cross-server routing,
    │  (Kafka)        │   offline message buffering)
    └─────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Message Delivery Flow

**Both users online (happy path):**

```
1. User A sends message via WebSocket to WS Server 1
2. WS Server 1 → Chat Service:
   a. Generate message_id (Snowflake ID: globally unique, time-ordered)
   b. Persist to Cassandra: messages table
   c. Lookup: where is User B connected? → Redis: session:B → WS Server 3
   d. Route message to WS Server 3 (via internal messaging: Kafka or gRPC)
3. WS Server 3 → pushes message to User B via WebSocket
4. User B's client sends ACK → Chat Service updates status to 'delivered'
5. User B opens the conversation → read receipt sent → status updated to 'read'
```

**Receiver offline:**

```
3. Lookup: User B not connected (no session in Redis)
4. Store message in Cassandra (it's already there from step 2b)
5. Send push notification via APNs/FCM
6. When User B comes online:
   a. Client requests: "Give me all messages since my last sync timestamp"
   b. Chat Service queries Cassandra for undelivered messages
   c. Delivers messages via WebSocket
   d. Updates delivery status
```

##### Deep Dive B: Message Ordering

**Challenge:** With distributed servers and async delivery, messages can arrive out of order.

**Solution: Per-conversation sequence numbers**

```sql
-- Each conversation has a monotonically increasing sequence number
-- Server assigns the sequence number when it persists the message

Message: {
  message_id: "msg_abc",
  conversation_id: "conv_123",
  sequence_num: 42,          ← assigned by the server, strictly ordered
  sender_id: "user_A",
  content: "Hello!",
  timestamp: 1705276800
}
```

The client always renders messages sorted by `sequence_num` within a conversation. Even if delivery is out of order, the client buffers and re-sorts.

**For group chats:** The server serializes all messages for a conversation through a single partition (in Kafka, partition by conversation_id), ensuring a total order.

##### Deep Dive C: Message Storage

```sql
-- Cassandra schema (optimized for "fetch recent messages in a conversation")

CREATE TABLE messages (
    conversation_id  UUID,
    sequence_num     BIGINT,
    message_id       UUID,
    sender_id        UUID,
    content          TEXT,
    media_url        TEXT,
    message_type     TEXT,       -- text, image, video, file
    status           TEXT,       -- sent, delivered, read
    created_at       TIMESTAMP,
    PRIMARY KEY (conversation_id, sequence_num)
) WITH CLUSTERING ORDER BY (sequence_num DESC);

-- Query: last 50 messages in a conversation
SELECT * FROM messages
WHERE conversation_id = ?
ORDER BY sequence_num DESC
LIMIT 50;

-- Pagination: messages before sequence_num X
SELECT * FROM messages
WHERE conversation_id = ?
  AND sequence_num < ?
ORDER BY sequence_num DESC
LIMIT 50;
```

**Why Cassandra?**
- Write-optimized (LSM tree) — handles 600K msg/sec easily
- Partition by conversation_id — all messages for a chat are co-located
- Time-ordered clustering — efficient range scans for message history
- Horizontally scalable — add nodes as data grows

##### Deep Dive D: Group Chat

```
Group "Engineering" has 200 members.

User A sends a message to the group:
  1. Chat Service persists message to messages table (conversation_id = group_id)
  2. Fan-out to all online members:
     a. Fetch group members: [B, C, D, ..., 200 users]
     b. For each online member: look up WS server, route message
     c. For offline members: queue push notification
  3. Optimization: batch the fan-out through Kafka
     → One Kafka message per group message
     → Consumer reads group membership and fans out
```

**Scaling group chat:**
- Small groups (< 100): fan-out to all members individually
- Large groups (100-500): batch notifications, paginate delivery
- Channels (Slack-style, 10K+ members): don't fan-out. Members pull messages when they open the channel. Only send push notifications to users who have the channel unmuted.

##### Deep Dive E: Presence (Online/Offline Status)

**Naive approach:** User sends heartbeat every 5 seconds → Redis key with TTL.

```
On connect:  SET presence:user_A "online" EX 30
Heartbeat:   EXPIRE presence:user_A 30
On disconnect: DEL presence:user_A
```

**Problem at scale:** 50M concurrent users * heartbeat every 5 seconds = 10M Redis writes/sec.

**Optimized approach:**
- Only broadcast presence changes (online ↔ offline), not continuous heartbeats
- Use a presence service that tracks connections per WS server
- When a WS server detects a disconnect (or heartbeat timeout), it publishes a presence change event
- Only push presence updates to users who have the changed user in their contact list and are currently viewing the chat list

```
User A's contacts: [B, C, D]
A goes offline → Presence Service publishes event
→ Only delivered to B, C, D's WS servers (if they're online and have the contact list visible)
```

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **WebSocket scaling** | Shard connections across 1000+ WS servers. Use consistent hashing on user_id to assign server. |
| **WS server failure** | Client auto-reconnects to a different server. Pending messages fetched from Cassandra on reconnect. |
| **Message ordering** | Kafka partition per conversation ensures total order. Server assigns sequence numbers. |
| **End-to-end encryption** | Client encrypts message with recipient's public key. Server stores ciphertext. Only recipient's device can decrypt. Signal Protocol. |
| **Multi-device sync** | Store messages per conversation, not per device. Each device tracks its own "last synced sequence_num." On opening the app, fetch messages since last sync. |
| **Media messages** | Upload media to S3/CDN, get a URL, send the URL as the message content. Recipient's client downloads media separately. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How does end-to-end encryption work?" | Signal Protocol: each user has a public/private key pair. Sender encrypts with recipient's public key. Server never sees plaintext. Key exchange happens on first contact. |
| "How would you implement message search?" | Index messages in Elasticsearch (per user, for their own messages). For E2E encrypted chats, search must happen client-side (server has no plaintext). |
| "How would you add typing indicators?" | Ephemeral signal: sender's client sends "user_A is typing" via WebSocket. Server routes to recipient. No persistence needed. Use debouncing (send once per 3 seconds). |
| "How would you support message reactions?" | Store as a sub-table: `(message_id, user_id, emoji)`. Broadcast reaction events to the conversation like any other update. |

---

### Solution 8: Design a Web Crawler

> **Difficulty:** Advanced (Senior Level)
> **Time allocation:** 45 minutes
> **Real-world references:** Googlebot, Bingbot, Scrapy (at scale), Common Crawl

---

#### Opening Statement

> "A web crawler systematically fetches web pages by following links. At scale, the challenges are politeness (not overloading websites), deduplication (not re-crawling the same content), prioritization (crawling important pages first), and handling the sheer volume of the web. Let me scope this."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Seed URLs** | Start from a set of seed URLs and discover new pages by following links |
| FR2 | **Politeness** | Respect robots.txt and rate-limit requests per domain |
| FR3 | **Content extraction** | Download pages and extract text, links, and metadata |
| FR4 | **Deduplication** | Avoid re-crawling identical or near-identical content |
| FR5 | **Prioritization** | Crawl important/popular pages first and more frequently |
| FR6 | **Recrawl** | Periodically revisit pages to detect changes |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Throughput** | 1 billion pages/week (~1,650 pages/sec) |
| NFR2 | **Scalability** | Horizontally scalable crawl fleet |
| NFR3 | **Robustness** | Handle malformed HTML, timeouts, spider traps, infinite calendars |
| NFR4 | **Storage** | Store crawled content for downstream processing |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Pages/week | 1 billion |
| Pages/second | ~1,650 |
| Average page size (raw HTML) | 500 KB |
| Bandwidth | 1,650 * 500 KB = ~825 MB/sec = ~6.6 Gbps |
| Storage/week | 1B * 500 KB = ~500 TB (raw); ~100 TB compressed |
| Unique domains | ~50 million |
| URLs discovered (total) | 100+ billion |

---

#### Step 3 — High-Level Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│  URL Frontier (Priority Queue + Politeness Queue)                    │
│                                                                      │
│  ┌────────────────────────┐  ┌────────────────────────────────────┐  │
│  │ Priority Queues        │  │ Per-Domain Politeness Queues       │  │
│  │ P0: news (every 5m)   │  │ example.com: next_ok=10:00:03     │  │
│  │ P1: popular (hourly)  │──▶│ blog.org:    next_ok=10:00:05     │  │
│  │ P2: normal (daily)    │  │ shop.net:    next_ok=10:00:01     │  │
│  │ P3: long-tail (weekly)│  └────────────────────────────────────┘  │
│  └────────────────────────┘                                          │
└────────────────────────────────────┬─────────────────────────────────┘
                                     │
                                     ▼
┌────────────────────────────────────────────────────────────────────┐
│  Crawler Workers (distributed fleet, 100s of machines)             │
│                                                                    │
│  For each URL:                                                     │
│  1. DNS resolve (with cache)                                       │
│  2. Check robots.txt (cached per domain)                           │
│  3. HTTP GET with timeout + user-agent                             │
│  4. Handle redirects (limit: 5 hops)                               │
│  5. Detect content type (skip binary/media)                        │
│  6. Send raw HTML to Content Processor                             │
└──────────────────────┬─────────────────────────────────────────────┘
                       │
                       ▼
┌────────────────────────────────────────────────────────────────────┐
│  Content Processor Pipeline                                        │
│                                                                    │
│  1. Parse HTML → extract text + links                              │
│  2. URL normalization (remove fragments, decode, canonical form)    │
│  3. URL dedup: has this URL been seen before? (Bloom filter)       │
│  4. Content dedup: is this content a duplicate? (SimHash)          │
│  5. New URLs → URL Frontier                                        │
│  6. Store content → Content Store (S3 / HDFS)                      │
│  7. Feed to downstream: indexer, search engine, analytics          │
└────────────────────────────────────────────────────────────────────┘

Supporting Services:
  ┌─────────────┐  ┌──────────────┐  ┌─────────────────────────┐
  │ DNS Cache   │  │ robots.txt   │  │ URL Dedup Store          │
  │ (local +    │  │ Cache        │  │ (Bloom filter or        │
  │  shared)    │  │ (per domain) │  │  Redis set, 100B URLs)  │
  └─────────────┘  └──────────────┘  └─────────────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: URL Frontier — Priority + Politeness

The frontier is the brain of the crawler. It must balance two goals:
1. **Priority:** Crawl important pages first
2. **Politeness:** Don't overwhelm any single domain

```
Architecture: two-level queue system

Level 1 — Priority (front queues):
  URLs assigned to priority buckets based on:
  - PageRank / domain authority
  - Content type (news → higher priority)
  - Change frequency (frequently updated → higher priority)

Level 2 — Politeness (back queues):
  Each domain has its own queue.
  A per-domain timer enforces minimum delay between requests.
  Worker picks the domain queue whose timer has expired.
```

```
URL arrives → assign priority → front queue Pk
Front queue Pk → dequeue → route to back queue for domain D
Back queue for domain D → wait until next_allowed_time → dequeue to worker
```

This ensures that even if many high-priority URLs come from the same domain, we never flood that domain.

##### Deep Dive B: Deduplication

**URL deduplication (have we seen this URL before?):**

```
Normalize URL:
  - Remove fragment (#section)
  - Lowercase scheme and host
  - Remove default ports (:80, :443)
  - Sort query parameters
  - Remove tracking parameters (utm_source, etc.)
  - Resolve relative URLs to absolute

Dedup check: Bloom filter with 100B URLs
  - 100 billion entries, 0.1% false positive rate
  - Size: ~114 GB (fits in memory across a small cluster)
  - False positive: we skip a URL we haven't seen (acceptable — we'll catch it on recrawl)
  - False negative: none (Bloom filters never have false negatives)
```

**Content deduplication (same content, different URL):**

Many pages have identical or near-identical content (mirrors, syndication, scraped copies).

```
SimHash algorithm:
  1. Extract features (word n-grams) from the page
  2. Hash each feature
  3. Combine hashes into a single 64-bit fingerprint
  4. Two pages are near-duplicates if their SimHash differs by ≤ 3 bits
     (Hamming distance ≤ 3)

Store fingerprints in a lookup table. For each new page:
  → Compute SimHash
  → Check for near-duplicates (using bit-manipulation tricks for fast lookup)
  → If duplicate: skip or deprioritize
```

##### Deep Dive C: Robustness — Handling the Hostile Web

| Trap | How to Detect | Mitigation |
|------|---------------|------------|
| **Spider traps** (infinite URLs) | URL depth > 15, same domain generates unbounded URLs | Max URL depth limit; max URLs per domain per crawl cycle |
| **Infinite calendars** | URLs like `/calendar/2025/01`, `/calendar/2025/02`, ... | Detect repeating URL patterns; limit per-path-pattern count |
| **Soft 404s** | Server returns 200 but with "not found" content | Content-based detection: check for "page not found" text patterns |
| **Very slow servers** | Connection or read timeout > 30 seconds | Strict timeouts (connect: 5s, read: 30s); abort and move on |
| **Enormous pages** | 100 MB HTML file | Max download size (10 MB); stream and truncate |
| **JavaScript-rendered content** | Content only loads via JS (SPA) | Headless browser rendering (Puppeteer) for high-priority pages; expensive, use selectively |

##### Deep Dive D: Recrawl Strategy

Not all pages need to be recrawled at the same frequency.

```
Adaptive recrawl based on change frequency:

1. Track last_crawled, last_changed for each URL
2. If a page changed on last 3 visits → increase crawl frequency
3. If a page hasn't changed in 5 visits → decrease crawl frequency
4. Formula: next_crawl = last_crawled + base_interval * decay_factor

Priority overrides:
  - News homepages: every 5 minutes (regardless of change rate)
  - Sitemaps: check daily for new URLs
  - Low-value pages: monthly at most
```

---

#### Step 5 — Scaling

| Concern | Solution |
|---------|----------|
| **Crawl throughput** | Distribute crawling across 100+ worker machines. Partition domains across workers. |
| **DNS bottleneck** | Local DNS cache per worker + shared DNS cache (Redis). DNS resolution can be 100ms — caching is critical. |
| **Network bandwidth** | 6.6 Gbps is significant. Co-locate crawlers in a datacenter with fat network pipes. Compress stored content. |
| **Frontier at scale** | Distribute the frontier: partition front queues and back queues across multiple machines by domain hash. |
| **Monitoring** | Track: pages/sec, error rates by type (timeout, 4xx, 5xx), queue depth, unique domains crawled, duplicate rate. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How would you crawl the dark web or pages behind logins?" | Dark web: use Tor proxy. Login-required pages: generally skip (ethical/legal concerns). For authorized crawling (e.g., your own SaaS), use session cookies. |
| "How would you handle JavaScript-heavy pages?" | Headless browser (Puppeteer, Playwright). Very expensive (10x slower than simple HTTP fetch). Use selectively for high-priority pages where content is JS-rendered. |
| "How would you distribute the crawler globally?" | Deploy crawlers in multiple regions. Assign domains to the geographically closest crawler region (reduces latency, respects data locality). |
| "How would you detect and handle content changes?" | Compare content hashes between crawls. Store previous hash per URL. If hash differs → content changed → re-index. Also use HTTP `If-Modified-Since` / `ETag` headers. |

---

### Solution 9: Design a Video Streaming Platform (YouTube / Netflix)

> **Difficulty:** Advanced (Senior Level)
> **Time allocation:** 45 minutes
> **Real-world references:** YouTube, Netflix, Twitch, Vimeo, Disney+

---

#### Opening Statement

> "Video streaming is one of the most bandwidth-intensive applications on the internet — Netflix alone accounts for ~15% of global internet traffic. The key challenges are efficient video encoding, content delivery at global scale, and adaptive bitrate streaming. Let me understand whether we're designing a user-generated content platform like YouTube or a curated streaming service like Netflix, as the upload pipeline differs significantly."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Upload** | Users upload videos (up to 4K resolution, up to 2 hours long) |
| FR2 | **Transcode** | Convert uploaded video into multiple resolutions and formats |
| FR3 | **Stream** | Users watch videos with adaptive bitrate streaming |
| FR4 | **Search** | Search videos by title, description, tags |
| FR5 | **Recommendations** | Personalized video recommendations on the home page |
| FR6 | **Engagement** | Likes, comments, view count, watch history |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Scale** | 1 billion DAU, 500M videos |
| NFR2 | **Start latency** | Video starts playing in < 2 seconds |
| NFR3 | **Buffering** | < 1% rebuffer rate during playback |
| NFR4 | **Upload processing** | Video available for viewing within 10-30 minutes of upload |
| NFR5 | **Availability** | 99.99% for the streaming path |
| NFR6 | **Global** | Low latency streaming worldwide |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Total videos | 500 million |
| New uploads/day | 500,000 |
| Average video duration | 10 minutes |
| Average raw upload size | 1 GB (1080p, 10 min) |
| Transcoded versions | 6 resolutions × 2 codecs = 12 versions per video |
| Storage per video (all versions) | ~5 GB |
| Total storage | 500M * 5 GB = ~2.5 EB (exabytes) |
| Concurrent viewers | 100 million |
| Average streaming bitrate | 5 Mbps |
| Total bandwidth | 100M * 5 Mbps = 500 Tbps (served from CDN edge) |

---

#### Step 3 — High-Level Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│  UPLOAD PIPELINE (offline, async)                                    │
│                                                                      │
│  ┌──────────┐   ┌──────────────┐   ┌─────────────┐   ┌───────────┐ │
│  │  Upload   │──▶│  Object      │──▶│  Transcode  │──▶│  CDN      │ │
│  │  Service  │   │  Storage     │   │  Workers    │   │  Origin   │ │
│  │           │   │  (S3 - raw)  │   │  (encode    │   │  (S3 -    │ │
│  │  (chunked │   │              │   │   multiple  │   │  encoded) │ │
│  │   upload) │   │              │   │   formats)  │   │           │ │
│  └──────────┘   └──────────────┘   └─────────────┘   └───────────┘ │
│                                          │                          │
│                                    ┌─────▼─────────┐                │
│                                    │  Metadata      │                │
│                                    │  Service       │                │
│                                    │  (title, tags, │                │
│                                    │  thumbnails)   │                │
│                                    └───────────────┘                │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STREAMING PIPELINE (online, real-time)                              │
│                                                                      │
│  ┌──────────┐   ┌──────────────┐   ┌──────────────┐                │
│  │  User    │──▶│  API Gateway │──▶│  Video       │                │
│  │  (player)│   │              │   │  Service     │                │
│  │          │   │              │   │  (metadata,  │                │
│  │          │   │              │   │   manifest)  │                │
│  │          │◀──│──────────────│───│──────────────│──┐             │
│  └──────────┘   └──────────────┘   └──────────────┘  │             │
│       ▲                                               │             │
│       │         ┌────────────────────────────────┐    │             │
│       │         │  CDN Edge Servers               │    │             │
│       └─────────│  (serve video segments from    │◀───┘             │
│                 │   edge cache, fall back to      │                  │
│                 │   origin S3)                    │                  │
│                 └────────────────────────────────┘                  │
└──────────────────────────────────────────────────────────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Video Upload and Transcoding Pipeline

**Chunked upload:**

```
1. Client splits video into 5 MB chunks
2. Each chunk uploaded independently (resumable on failure)
3. Server reassembles chunks → stores raw file in S3
4. Triggers transcoding job
```

**Transcoding (the most compute-intensive part):**

```
Raw upload (1080p, H.264, 1 GB)
        │
        ▼
┌───────────────────────────────────────────────────────────────┐
│  Transcode Worker                                             │
│                                                               │
│  Output renditions:                                           │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ Resolution │  Bitrate  │  Codec   │  File Size (10min) │   │
│  ├────────────┼───────────┼──────────┼────────────────────┤   │
│  │ 2160p (4K) │ 15 Mbps  │ H.265    │ ~1.1 GB            │   │
│  │ 1080p      │ 5 Mbps   │ H.264    │ ~375 MB            │   │
│  │ 720p       │ 2.5 Mbps │ H.264    │ ~190 MB            │   │
│  │ 480p       │ 1 Mbps   │ H.264    │ ~75 MB             │   │
│  │ 360p       │ 500 Kbps │ H.264    │ ~37 MB             │   │
│  │ 240p       │ 250 Kbps │ H.264    │ ~19 MB             │   │
│  └────────────┴───────────┴──────────┴────────────────────┘   │
│                                                               │
│  Also generates:                                              │
│  - Thumbnail sprites (for timeline preview)                   │
│  - Audio-only track (for background play)                     │
│  - Subtitle extraction / transcription                        │
└───────────────────────────────────────────────────────────────┘
```

**Parallelizing transcoding:** Split the raw video into segments (e.g., 10-second chunks). Transcode segments in parallel across multiple workers. Stitch the results. A 10-minute video can be transcoded in < 2 minutes with sufficient parallelism.

##### Deep Dive B: Adaptive Bitrate Streaming (ABR)

The player dynamically switches quality based on the user's bandwidth.

**How it works (HLS / DASH):**

```
1. Video is split into small segments (2-10 seconds each)
2. Each segment is encoded at multiple bitrates
3. A manifest file (.m3u8 for HLS, .mpd for DASH) lists all available streams

Manifest example (HLS):
  #EXT-X-STREAM-INF:BANDWIDTH=5000000,RESOLUTION=1920x1080
  1080p/playlist.m3u8
  #EXT-X-STREAM-INF:BANDWIDTH=2500000,RESOLUTION=1280x720
  720p/playlist.m3u8
  #EXT-X-STREAM-INF:BANDWIDTH=1000000,RESOLUTION=854x480
  480p/playlist.m3u8

4. Player starts with a conservative quality (e.g., 480p)
5. Measures download speed of each segment
6. If bandwidth > next quality threshold → switch UP
7. If buffer is depleting → switch DOWN immediately
```

**Segment structure on storage:**

```
s3://video-encoded/video_abc/
  ├── manifest.m3u8
  ├── 1080p/
  │   ├── segment_000.ts (2 seconds)
  │   ├── segment_001.ts
  │   └── ...
  ├── 720p/
  │   ├── segment_000.ts
  │   └── ...
  └── 480p/
      ├── segment_000.ts
      └── ...
```

##### Deep Dive C: Content Delivery Network (CDN)

With 500 Tbps of total bandwidth, CDN is non-negotiable.

```
Streaming flow:
  1. Player requests manifest from API
  2. API returns manifest with CDN URLs for segments
  3. Player fetches segments from nearest CDN edge
  4. CDN edge:
     a. Cache HIT → serve immediately (< 50ms)
     b. Cache MISS → fetch from origin (S3), cache, serve

Popular videos: cached at virtually every edge location
Long-tail videos: may require origin fetch (higher latency for first viewer)
```

**CDN optimization strategies:**

| Strategy | Description |
|----------|-------------|
| **Pre-warm popular content** | Push trending videos to edge caches proactively |
| **Tiered caching** | Edge → Regional → Origin. Regional cache reduces origin load for mid-popularity content |
| **Geo-routing** | DNS or Anycast routes users to nearest edge POP |
| **Multi-CDN** | Use multiple CDN providers (Akamai, CloudFront, Fastly). Route to the best-performing one per region. |

##### Deep Dive D: Video Metadata and Search

```sql
CREATE TABLE videos (
    id              UUID PRIMARY KEY,
    user_id         UUID NOT NULL,
    title           VARCHAR(500) NOT NULL,
    description     TEXT,
    tags            TEXT[],
    duration_sec    INTEGER,
    status          VARCHAR(20),
    view_count      BIGINT DEFAULT 0,
    like_count      BIGINT DEFAULT 0,
    manifest_url    TEXT,
    thumbnail_url   TEXT,
    uploaded_at     TIMESTAMPTZ,
    published_at    TIMESTAMPTZ
);
```

**View count at scale:**
- Don't increment on every view (too much write contention)
- Buffer view events in Kafka → aggregate in a batch job every few minutes
- Display approximate counts ("1.2M views") — exactness not needed

**Search:** Index video titles, descriptions, and tags in Elasticsearch. Boost by view count, recency, and relevance score.

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **Storage cost** | Tiered: hot videos on SSD-backed origin, cold videos (< 10 views/month) on cheaper storage (S3 IA / Glacier). |
| **Transcode cost** | Spot/preemptible instances for non-urgent transcoding. Priority queue for popular channels. |
| **DRM / Content protection** | Widevine (Google), FairPlay (Apple), PlayReady (Microsoft). Encrypt segments; player obtains decryption key from a license server. |
| **Copyright detection** | Content ID system: fingerprint uploaded videos and compare against a database of copyrighted content (audio + visual fingerprints). |
| **Live streaming** | Ingest RTMP stream from broadcaster. Transcode in real-time. Deliver via low-latency HLS (< 3s latency). Separate architecture from VOD. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How does Netflix differ from YouTube architecturally?" | Netflix pre-encodes its catalog (known content). YouTube must transcode user uploads on-the-fly. Netflix pushes content to Open Connect appliances inside ISPs. YouTube relies on Google's global CDN. |
| "How would you minimize time-to-first-frame?" | Pre-load the manifest and first few segments. Start with lowest quality. Use HTTP/2 or HTTP/3 for faster connection. CDN edge proximity. |
| "How do you handle live streaming?" | RTMP ingest → real-time transcode → segment into 1-2 second chunks → push to CDN. Low-Latency HLS or WebRTC for sub-second latency. |
| "How would you build the recommendation engine?" | Collaborative filtering (users who watched X also watched Y) + content-based (similar genres, tags). Deep learning models trained on watch history, completion rate, and engagement signals. |

---

### Solution 10: Design a Search Autocomplete System

> **Difficulty:** Advanced (Senior Level)
> **Time allocation:** 45 minutes
> **Real-world references:** Google Search autocomplete, Amazon product search, Algolia, Elasticsearch suggest

---

#### Opening Statement

> "Search autocomplete must return suggestions in under 100ms as the user types — every keystroke triggers a query. The core challenge is building a prefix-matching system that's fast enough for real-time interaction and smart enough to rank suggestions by popularity and relevance. Let me clarify scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Prefix matching** | Given a prefix like "how to m...", return top suggestions |
| FR2 | **Ranked suggestions** | Results ranked by popularity, recency, and personalization |
| FR3 | **Top-K results** | Return 5-10 suggestions per keystroke |
| FR4 | **Multi-language** | Support queries in multiple languages |
| FR5 | **Freshness** | Trending topics appear in suggestions within minutes |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Latency** | < 100ms (p99) — must feel instant |
| NFR2 | **QPS** | 100,000 (every keystroke from every active user) |
| NFR3 | **Availability** | 99.99% |
| NFR4 | **Scale** | 10 billion unique queries in the suggestion database |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Search QPS | 100,000 |
| Average query length | 4 words, ~20 characters → ~20 autocomplete requests per full query |
| Autocomplete QPS | ~2,000,000 (with client-side debouncing at 100ms → ~500K actual) |
| Unique suggestions | 10 billion |
| Average suggestion size | 30 bytes |
| Total data | 10B * 30B = ~300 GB |

---

#### Step 3 — High-Level Architecture

```
┌──────────┐         ┌──────────────┐         ┌──────────────────────┐
│  User    │────────▶│  API Gateway │────────▶│  Autocomplete        │
│  (types  │         │  (CDN edge   │         │  Service             │
│  "how")  │◀────────│   caching)   │◀────────│                      │
└──────────┘         └──────────────┘         │  ┌────────────────┐  │
                                              │  │ Trie Lookup /  │  │
                                              │  │ Prefix Search  │  │
                                              │  └───────┬────────┘  │
                                              │          │           │
                                              │  ┌───────▼────────┐  │
                                              │  │  Ranking       │  │
                                              │  │  (popularity + │  │
                                              │  │   freshness)   │  │
                                              │  └────────────────┘  │
                                              └──────────┬───────────┘
                                                         │
                                    ┌────────────────────┼──────────────┐
                                    │                    │              │
                                    ▼                    ▼              ▼
                              ┌──────────┐       ┌────────────┐  ┌──────────┐
                              │  Trie    │       │  Trending  │  │  User    │
                              │  Store   │       │  Counter   │  │  History │
                              │  (in-mem │       │  (Redis)   │  │  (per-   │
                              │   sharded│       │            │  │   user)  │
                              │   nodes) │       └────────────┘  └──────────┘
                              └──────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  Offline Pipeline (updates trie periodically)                        │
│                                                                      │
│  Search Query Logs → Aggregate (MapReduce) → Compute popularity     │
│  → Build new trie → Swap into serving nodes                          │
└──────────────────────────────────────────────────────────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Trie Data Structure

A trie (prefix tree) is the natural data structure for prefix matching.

```
Root
 ├── h
 │   ├── o
 │   │   ├── w [score:5000] "how"
 │   │   │   ├── _ (space)
 │   │   │   │   ├── t
 │   │   │   │   │   ├── o [score:4500] "how to"
 │   │   │   │   │   │   ├── _ (space)
 │   │   │   │   │   │   │   ├── m
 │   │   │   │   │   │   │   │   ├── a
 │   │   │   │   │   │   │   │   │   ├── k
 │   │   │   │   │   │   │   │   │   │   ├── e [score:3200] "how to make"
 │   │   │   │   │   │   │   │   │   ...
```

**Optimization: store top-K suggestions at each node**

Instead of traversing the subtree to find top suggestions, pre-compute and cache the top-K at every node:

```
Node "how to" stores:
  top_5: [
    ("how to make money", 3200),
    ("how to lose weight", 2800),
    ("how to cook rice", 2100),
    ("how to tie a tie", 1900),
    ("how to screenshot", 1700)
  ]
```

Lookup is O(1) — just read the node. No subtree traversal needed.

**Trade-off:** More memory (every node stores K suggestions), but queries are instant. Memory increase is manageable because most nodes share many of the same top suggestions.

##### Deep Dive B: Trie Sharding and Serving

With 10 billion suggestions, the trie doesn't fit on a single machine.

**Sharding by prefix:**

```
Shard A: prefixes a-f
Shard B: prefixes g-m
Shard C: prefixes n-s
Shard D: prefixes t-z

Query "how" → first char 'h' → route to Shard B
```

**Or shard by hash** (more even distribution but requires a routing table).

**Each shard:**
- Multiple replicas for availability and read scaling
- Trie loaded entirely in memory (fast lookups)
- Served behind a load balancer

**Trie updates (new build every few hours):**

```
1. Offline pipeline aggregates query logs → computes frequency
2. Builds a new trie from scratch
3. Serializes trie to a compact binary format
4. Pushes to shard servers
5. Shard servers load new trie into memory
6. Atomic swap: old trie → new trie (zero-downtime)
```

##### Deep Dive C: Ranking Suggestions

**Scoring formula:**

```
score(suggestion) = w1 * popularity + w2 * freshness + w3 * personalization

Where:
  popularity  = log(search_count_last_30_days)
  freshness   = trending_boost if query volume spiked recently
  personalization = boost if the user has searched this before or it matches their interests
```

**Trending detection:**

```
Compare recent query volume (last 1 hour) to baseline (last 30 days):
  trending_score = volume_last_hour / (avg_hourly_volume_30d + epsilon)
  
If trending_score > 5 → flag as trending → boost in suggestions

Example: During a sports event, "world cup score" spikes 100x → immediately boosted.
```

**Personalization:**

```
Per-user recent queries (stored in Redis, TTL 30 days):
  user:u_123:recent_queries → ["python tutorial", "machine learning", ...]

If the user types "py", their personal history is checked first:
  → "python tutorial" gets a personalization boost
```

##### Deep Dive D: Performance Optimizations

| Optimization | Impact |
|--------------|--------|
| **Client-side debouncing** | Wait 100-200ms after last keystroke before sending request. Reduces QPS by 50-80%. |
| **Client-side caching** | If user types "ho" and gets results, typing "how" only needs to filter locally (if previous results included "how to..." completions). |
| **CDN edge caching** | Popular prefixes ("how", "what", "why") are cached at the CDN edge. TTL = 1 hour for stable queries, 5 minutes for trending. |
| **HTTP/2 multiplexing** | Reuse connection for each keystroke request — eliminates TCP handshake overhead. |
| **Prefix-based routing** | Single-character prefixes are always cached. Multi-character prefixes may require the backend. |

---

#### Step 5 — Scaling

| Concern | Solution |
|---------|----------|
| **QPS** | CDN edge caching handles > 90% of requests. Backend trie serves the rest. |
| **Memory** | Shard the trie across nodes. Compress common prefixes. 300 GB of data × overhead = ~1 TB across the cluster. |
| **Freshness** | Real-time trending counter (Redis) merged with pre-built trie. Trending suggestions injected at query time. |
| **Multi-language** | Separate trie per language (or script). Detect language from user locale or first characters. |
| **Offensive content** | Filter suggestions against a blocklist. ML-based toxicity scoring on new suggestions. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "What if you can't fit the trie in memory?" | Use a compact trie (MARISA-trie, FST) which compresses shared suffixes. Or use Elasticsearch's completion suggester (disk-backed FST). |
| "How do you handle typos in the prefix?" | Edit-distance-based fuzzy matching: for "hwo", check within edit distance 1 → "how". Computationally expensive — limit to short prefixes. |
| "How does Google do this at their scale?" | Likely a combination of: in-memory serving tiers, massive CDN caching, ML-ranked suggestions, and per-user personalization models running at the edge. |
| "How would you support e-commerce autocomplete?" | Index product names, categories, and brands. Rank by sales volume, not search frequency. Include product images in suggestions. Separate "query suggestions" from "product suggestions." |

---

### Solution 11: Design a Ride-Sharing Service (Uber / Lyft)

> **Difficulty:** Advanced (Senior Level)
> **Time allocation:** 45 minutes
> **Real-world references:** Uber, Lyft, DiDi, Grab, Bolt

---

#### Opening Statement

> "A ride-sharing service is a real-time marketplace matching riders with nearby drivers. The key challenges are real-time geospatial indexing (finding nearby drivers in milliseconds), efficient matching algorithms, dynamic pricing, and managing a constantly moving fleet. Let me clarify scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Request ride** | Rider enters pickup and destination, requests a ride |
| FR2 | **Match driver** | System finds the best available driver nearby |
| FR3 | **Real-time tracking** | Both rider and driver see each other's location in real time |
| FR4 | **Trip management** | Start trip, navigate, end trip |
| FR5 | **Pricing** | Estimated fare upfront, final fare on completion, surge pricing |
| FR6 | **Payments** | Automatic charge at trip end |
| FR7 | **Ratings** | Rider and driver rate each other |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Matching latency** | < 10 seconds from request to driver match |
| NFR2 | **Location updates** | Drivers report location every 3 seconds |
| NFR3 | **Scale** | 1 million active drivers, 10 million rides/day |
| NFR4 | **Availability** | 99.99% for the ride request path |
| NFR5 | **Global** | Operates in 100+ cities worldwide |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Active drivers | 1 million |
| Location updates/sec | 1M / 3 = ~333,000 |
| Rides/day | 10 million |
| Ride requests/sec (peak) | ~200 |
| Location record size | ~100 bytes (lat, lng, driver_id, timestamp, status) |
| Location update write QPS | 333,000 |

---

#### Step 3 — High-Level Architecture

```
┌──────────────┐     ┌──────────────┐
│  Rider App   │     │  Driver App  │
└──────┬───────┘     └──────┬───────┘
       │                    │
       │ REST / WS          │ WS (location stream)
       │                    │
┌──────▼────────────────────▼────────────────────────────────────────┐
│  API Gateway / Load Balancer                                       │
└──────┬─────────────────┬──────────────────┬────────────────────────┘
       │                 │                  │
       ▼                 ▼                  ▼
┌────────────┐   ┌──────────────┐   ┌──────────────────────────────┐
│  Ride      │   │  Location    │   │  Matching Service            │
│  Service   │   │  Service     │   │  (find nearest available     │
│  (CRUD for │   │  (ingest +   │   │   driver + dispatch)         │
│   trips)   │   │   store      │   └──────────────────────────────┘
└────────────┘   │   driver     │
                 │   locations) │   ┌──────────────────────────────┐
                 └──────┬───────┘   │  Pricing Service             │
                        │           │  (ETA, fare, surge)          │
                        ▼           └──────────────────────────────┘
                 ┌──────────────┐
                 │  Geospatial  │   ┌──────────────────────────────┐
                 │  Index       │   │  Payment Service             │
                 │  (find nearby│   └──────────────────────────────┘
                 │   drivers)   │
                 └──────────────┘   ┌──────────────────────────────┐
                                    │  Notification Service        │
                                    └──────────────────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Geospatial Index — Finding Nearby Drivers

This is the core technical challenge. We need to answer: "Which available drivers are within 3 km of this location?" — in under 50ms, with 1M drivers being updated every 3 seconds.

**Approach 1: Geohash + Redis**

```
Geohash: encode (latitude, longitude) into a string prefix
  (37.7749, -122.4194) → "9q8yyz"

Precision:
  4 chars: ~39 km × 19.5 km cell
  5 chars: ~4.9 km × 4.9 km cell  ← good for nearby search
  6 chars: ~1.2 km × 0.6 km cell

Redis Sorted Set per geohash prefix:
  Key: "drivers:geo:9q8yy"
  Members: {driver_123: score, driver_456: score, ...}
  Score: encoded lat/lng for fine-grained distance calculation

To find nearby drivers:
  1. Compute the geohash of the rider's location
  2. Query this cell AND the 8 neighboring cells (to handle edge effects)
  3. Filter by distance and availability status
  4. Return top N closest
```

**Approach 2: Quadtree / S2 Geometry**

```
S2 Geometry (used by Google/Uber):
  - Divides Earth's surface into hierarchical cells
  - Each cell has a unique 64-bit ID
  - Cells at the same level are roughly equal area
  
Query: "All drivers within 3km of (lat, lng)"
  1. Compute S2 covering: set of S2 cells that cover the 3km radius circle
  2. For each cell: fetch drivers from index
  3. Filter by exact distance
```

| Approach | Pros | Cons |
|----------|------|------|
| **Geohash + Redis** | Simple, fast (in-memory), easy to implement | Edge effects at geohash boundaries; uneven cell sizes near poles |
| **S2 / H3** | Equal-area cells, better coverage queries | More complex to implement |
| **PostGIS** | Full SQL + geospatial (ST_DWithin) | Harder to scale for 333K writes/sec |

**Recommendation:** Geohash + Redis for simplicity and speed. Uber uses H3 (hexagonal grid), which is similar in concept.

##### Deep Dive B: Driver Location Updates

```
Driver app → WebSocket → Location Service → Geospatial Index

Every 3 seconds:
{
  "driver_id": "d_123",
  "lat": 37.7749,
  "lng": -122.4194,
  "heading": 90,
  "speed": 35,
  "status": "available",
  "timestamp": 1705276800
}

Location Service:
  1. Validate and sanitize
  2. Update geospatial index:
     a. Remove driver from old geohash cell (if moved)
     b. Add driver to new geohash cell
  3. If driver is on an active trip:
     → Push location to rider via WebSocket (real-time tracking)
  4. Store in time-series DB for analytics / trip replay
```

**Handling 333K writes/sec:**
- Redis handles this easily (100K+ ops/sec per node)
- Shard by city / region (each city has its own Redis cluster)
- Location updates are fire-and-forget (losing one update out of 20 per minute is fine)

##### Deep Dive C: Ride Matching Algorithm

```
Rider requests a ride at (lat, lng):

1. Query geospatial index: find available drivers within 3 km
   → Returns: [d_45 (0.5km), d_12 (1.1km), d_89 (1.8km), d_67 (2.4km)]

2. For each candidate, compute ETA using routing engine:
   d_45: 3 min ETA
   d_12: 5 min ETA
   d_89: 4 min ETA (closer road route than d_12)
   d_67: 8 min ETA

3. Score each driver:
   score = w1 * (1/ETA) + w2 * driver_rating + w3 * trip_completion_rate

4. Dispatch to highest-scoring driver:
   → Send ride offer to d_45
   → d_45 has 15 seconds to accept
   → If declined/timeout → offer to d_89
   → If no one accepts within 60s → notify rider "no drivers available"
```

**Matching optimizations:**

| Optimization | Description |
|--------------|-------------|
| **Batched matching** | Instead of matching one ride at a time, batch pending requests and solve a global optimization (minimize total wait time). |
| **Supply positioning** | Predict demand hotspots (ML on historical data + events) and nudge idle drivers toward high-demand areas. |
| **Shared rides** | Match multiple riders heading in the same direction to one driver (UberPool/Lyft Shared). Requires a routing algorithm that minimizes detour for existing passengers. |

##### Deep Dive D: Dynamic Pricing (Surge)

```
Surge pricing triggers when demand > supply in a geographic area.

For each pricing zone (e.g., 2km × 2km grid cell):
  supply = available_drivers_in_zone
  demand = ride_requests_in_last_5_minutes

  surge_multiplier = f(demand / supply)

  If demand/supply > 1.5 → surge 1.5x
  If demand/supply > 3.0 → surge 2.5x
  If demand/supply > 5.0 → surge 4.0x (capped)

Fare = base_fare + (per_km * distance) + (per_min * duration) * surge_multiplier
```

**Surge pricing serves two purposes:**
1. **Rations demand:** Higher prices reduce ride requests
2. **Increases supply:** Higher earnings attract more drivers to the area

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **City-level sharding** | Each city is an independent deployment. No cross-city coordination needed. |
| **Location service HA** | Redis Cluster with replicas per city. If a node fails, minimal data loss (just a few seconds of location data). |
| **Matching service HA** | Stateless; multiple instances behind a load balancer. If an instance crashes, pending matches are re-dispatched. |
| **Trip state durability** | Trip data stored in PostgreSQL with synchronous replication. A trip can never be lost. |
| **Geo-routing** | Integrate with Google Maps / OSRM for ETA calculation. Cache popular routes. |
| **Fraud detection** | Detect GPS spoofing (impossible speed, teleporting). Flag fraudulent fare disputes using trip replay data. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How would you handle airport queues?" | Virtual queue (FIFO) for drivers at designated pickup zones. Riders are matched with the next driver in queue, not nearest by GPS. |
| "How would you add scheduled rides?" | Store scheduled ride in a database. Scheduler triggers matching 10-15 minutes before scheduled time. Assign a driver and notify both parties. |
| "How would you estimate ETA?" | Graph-based routing (Dijkstra / A*) on a road network graph. Augment edge weights with real-time traffic data. ML model trained on historical trip durations. |
| "How would you handle the system during a major event (concert, sports game)?" | Pre-position drivers near the venue. Increase surge pricing gradually. Designate pickup/dropoff zones. Pre-compute optimal routes from the venue. |

---

### Solution 12: Design a File Storage Service (Google Drive / Dropbox)

> **Difficulty:** Advanced (Senior Level)
> **Time allocation:** 45 minutes
> **Real-world references:** Google Drive, Dropbox, OneDrive, iCloud Drive, Box

---

#### Opening Statement

> "A cloud file storage service lets users store, sync, and share files across devices. The core challenges are efficient file sync (especially for large files), conflict resolution when multiple devices edit the same file, and managing storage at massive scale. Let me understand the scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Upload / Download** | Store and retrieve files of any type (docs, images, videos) |
| FR2 | **Sync across devices** | Changes on one device automatically appear on all other devices |
| FR3 | **File versioning** | Keep revision history; restore previous versions |
| FR4 | **Sharing** | Share files/folders with other users via link or explicit permission |
| FR5 | **Folder structure** | Hierarchical folders with move, rename, delete |
| FR6 | **Offline access** | Mark files for offline availability; sync when back online |
| FR7 | **Large file support** | Handle files up to 50 GB |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Scale** | 500 million users, 100 billion files |
| NFR2 | **Sync latency** | Changes propagated to other devices within 5 seconds |
| NFR3 | **Upload efficiency** | Only transfer changed bytes, not the whole file (delta sync) |
| NFR4 | **Durability** | 99.999999999% (11 nines) — files must never be lost |
| NFR5 | **Availability** | 99.9% |
| NFR6 | **Storage efficiency** | Deduplication across users (many users store the same popular files) |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Total users | 500 million |
| Average files per user | 200 |
| Total files | 100 billion |
| Average file size | 500 KB |
| Total logical storage | 100B * 500 KB = 50 PB |
| With dedup (~40% savings) | ~30 PB |
| With replication (3x) | ~90 PB |
| File uploads/day | 1 billion |
| Upload QPS (peak) | ~20,000 |
| Metadata record size | 500 bytes |
| Metadata storage | 100B * 500B = 50 TB |

---

#### Step 3 — High-Level Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│  Desktop/Mobile Client                                           │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │  File Watcher → Chunker → Delta Engine → Upload Queue     │  │
│  │                                                            │  │
│  │  Sync Engine ← Notification Listener ← Download Queue     │  │
│  └────────────────────────────────────────────────────────────┘  │
└──────────────────────────────┬───────────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│  API Gateway                                                     │
└───────┬──────────────────┬─────────────────┬─────────────────────┘
        │                  │                 │
        ▼                  ▼                 ▼
┌──────────────┐   ┌──────────────┐   ┌──────────────────────────┐
│  Metadata    │   │  Block       │   │  Notification            │
│  Service     │   │  Service     │   │  Service                 │
│              │   │              │   │  (push sync events       │
│  - File tree │   │  - Upload/   │   │   to other devices)      │
│  - Versions  │   │    download  │   │                          │
│  - Sharing   │   │    chunks   │   │  WebSocket / Long-poll   │
│  - Permissions│  │  - Dedup    │   └──────────────────────────┘
└──────┬───────┘   └──────┬───────┘
       │                  │
       ▼                  ▼
┌──────────────┐   ┌──────────────┐
│  Metadata DB │   │  Block Store │
│  (PostgreSQL │   │  (S3 / GCS)  │
│   sharded)   │   │              │
└──────────────┘   └──────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Chunking and Delta Sync

The most important optimization. Without it, editing a single byte in a 1 GB file would require re-uploading the entire file.

**Content-defined chunking (CDC):**

```
Instead of splitting the file at fixed boundaries (every 4 MB),
use a rolling hash (Rabin fingerprint) to find natural chunk boundaries:

File:  [.......chunk1........][.......chunk2......][....chunk3.....]

Rolling hash: slide a window across the file.
When hash(window) % avg_chunk_size == 0 → chunk boundary.

Average chunk size: 4 MB (configurable)
Min chunk: 1 MB, Max chunk: 8 MB
```

**Why content-defined over fixed-size?**

If you insert 1 byte at the start of a file:
- **Fixed-size chunks:** Every chunk shifts by 1 byte → ALL chunks are different → re-upload entire file
- **Content-defined chunks:** Only the first chunk changes → upload only 1 chunk

**Delta sync flow:**

```
1. File modified on Device A
2. Client re-chunks the file using rolling hash
3. Compare chunk hashes with the previous version:
   Old: [hash_A, hash_B, hash_C, hash_D]
   New: [hash_A, hash_B, hash_X, hash_D]  ← only chunk C changed
4. Upload only hash_X (the changed chunk)
5. Update metadata: file now points to [hash_A, hash_B, hash_X, hash_D]
6. Notify other devices: "file changed, here are the new chunk references"
7. Device B downloads only hash_X, reconstructs the file
```

##### Deep Dive B: File Metadata Model

```sql
CREATE TABLE files (
    id              UUID PRIMARY KEY,
    user_id         UUID NOT NULL,
    parent_id       UUID REFERENCES files(id),
    name            VARCHAR(255) NOT NULL,
    is_folder       BOOLEAN NOT NULL DEFAULT FALSE,
    latest_version  INTEGER NOT NULL DEFAULT 1,
    size_bytes      BIGINT,
    mime_type       VARCHAR(100),
    checksum        VARCHAR(64),
    is_deleted      BOOLEAN DEFAULT FALSE,
    created_at      TIMESTAMPTZ NOT NULL,
    updated_at      TIMESTAMPTZ NOT NULL,
    UNIQUE (user_id, parent_id, name)
);

CREATE TABLE file_versions (
    file_id         UUID REFERENCES files(id),
    version         INTEGER NOT NULL,
    chunk_ids       UUID[] NOT NULL,
    size_bytes      BIGINT NOT NULL,
    checksum        VARCHAR(64) NOT NULL,
    modified_by     UUID NOT NULL,
    device_id       UUID,
    created_at      TIMESTAMPTZ NOT NULL,
    PRIMARY KEY (file_id, version)
);

CREATE TABLE chunks (
    id              UUID PRIMARY KEY,
    hash            VARCHAR(64) UNIQUE NOT NULL,
    size_bytes      INTEGER NOT NULL,
    storage_key     VARCHAR(255) NOT NULL,
    ref_count       INTEGER NOT NULL DEFAULT 1,
    created_at      TIMESTAMPTZ NOT NULL
);
```

**A file's content is defined by its ordered list of chunk IDs.** To reconstruct a file: fetch all chunks in order and concatenate.

##### Deep Dive C: Deduplication

Since chunks are identified by their content hash (SHA-256), identical chunks across users or files are stored only once.

```
User A uploads "report.pdf" → chunks: [hash_1, hash_2, hash_3]
User B uploads same "report.pdf" → chunks: [hash_1, hash_2, hash_3]

All three hashes already exist in the chunk store.
Block Service responds: "I already have these. Upload skipped."
Only the metadata (file record pointing to existing chunks) is created.
```

**Savings:** Common files (OS updates, popular documents, shared attachments) are stored once regardless of how many users have them. Real-world dedup ratio: 30-50%.

**Reference counting:** Each chunk tracks `ref_count`. When a file version is deleted, decrement the ref_count of its chunks. When ref_count reaches 0, the chunk can be garbage collected.

##### Deep Dive D: Sync Protocol and Conflict Resolution

**Sync notification flow:**

```
1. Device A modifies a file → uploads changed chunks → updates metadata
2. Metadata Service publishes event: "file X updated to version 5"
3. Notification Service pushes event to all of user's connected devices
4. Device B receives notification:
   a. Compares local version (4) with server version (5)
   b. Downloads the new chunk list for version 5
   c. Identifies changed chunks → downloads only those
   d. Reconstructs the updated file locally
```

**Conflict resolution (same file edited on two devices while offline):**

```
Device A (offline): edits file → creates version 5A
Device B (offline): edits file → creates version 5B

Both come online and try to push:
  Device A pushes first → version 5 accepted
  Device B pushes → CONFLICT (expected version 4, but server is now at 5)

Resolution:
  Option 1 (Dropbox approach): Save B's version as "file (conflicted copy - Device B).txt"
  Option 2 (Google Drive approach): Last write wins (if files are Google Docs, use OT/CRDT for merge)
  Option 3: Prompt the user to choose which version to keep
```

**Recommendation:** For binary files (images, PDFs), conflicted copy is the safest. For text-based documents, merge is possible with appropriate algorithms.

##### Deep Dive E: Sharing and Permissions

```
Sharing model:
  - File/folder can be shared with specific users (viewer, editor)
  - Or via a shareable link (anyone with the link)

Permission checks:
  1. Direct permission: user_id has role on file_id
  2. Inherited: file's parent folder grants access
  3. Link-based: request includes a valid share token

Table:
CREATE TABLE sharing (
    file_id         UUID REFERENCES files(id),
    shared_with     UUID,
    permission      VARCHAR(20) NOT NULL,
    share_link_token VARCHAR(64) UNIQUE,
    expires_at      TIMESTAMPTZ,
    created_at      TIMESTAMPTZ NOT NULL
);
```

Permissions are evaluated from the file up to the root, checking at each level. Cache the resolved permission per (user, file) to avoid repeated traversal.

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **Metadata scaling** | Shard PostgreSQL by user_id. Each user's file tree is self-contained. |
| **Block storage** | S3 with 11 nines of durability. Cross-region replication for disaster recovery. |
| **Large files (50 GB)** | Chunked upload with resume capability. Each chunk uploaded independently. Parallel upload of chunks. |
| **Sync storms** | When a user adds 10,000 files, batch the sync events. Client processes a batch of changes, not individual notifications. |
| **Storage reclamation** | Deleted files go to trash (30-day retention). After trash expires: decrement chunk ref_counts. Background GC job deletes zero-refcount chunks. |
| **Bandwidth optimization** | Compression before upload (client-side gzip). Delta sync reduces transfer by 80-95% for edits. |
| **Operational** | Monitor: sync lag, upload success rate, chunk dedup ratio, storage growth, conflict rate. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How does Dropbox handle sync so efficiently?" | Content-defined chunking (4 MB avg), delta sync (only changed chunks), client-side dedup check before upload, compression, and a notification service for real-time push. |
| "How would you support collaborative editing?" | For office documents: integrate a real-time collaboration engine (OT/CRDT, like Solution 11). For other files: lock-based editing or conflicted copies. |
| "How would you implement search across files?" | Extract text from files (OCR for images, parsing for PDFs/docs). Index in Elasticsearch per user. Encrypted files can't be searched server-side — need client-side search. |
| "How would you handle malware scanning?" | Scan uploaded chunks asynchronously. Quarantine files flagged as malicious. Notify the user. Block download of quarantined files. |

### Solution 13: Design a Distributed Message Queue (Kafka)

> **Difficulty:** Staff Level
> **Time allocation:** 45-60 minutes
> **Real-world references:** Apache Kafka, Amazon Kinesis, Apache Pulsar, RabbitMQ, Google Pub/Sub

---

#### Opening Statement

> "A distributed message queue is a foundational piece of infrastructure — it decouples producers and consumers, absorbs traffic spikes, and enables event-driven architectures. The design space ranges from a simple task queue (like SQS) to a full distributed commit log (like Kafka). Let me clarify which end of the spectrum we're targeting."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Topics** | Producers publish messages to named topics |
| FR2 | **Publish** | Producers append messages to a topic; messages are ordered within a partition |
| FR3 | **Subscribe** | Consumer groups subscribe to topics; each message is delivered to exactly one consumer per group |
| FR4 | **Replay** | Consumers can seek to any offset and re-read messages (unlike traditional queues that delete on ack) |
| FR5 | **Retention** | Messages are retained for a configurable duration (e.g., 7 days) regardless of consumption |
| FR6 | **Ordering** | Messages are strictly ordered within a partition |
| FR7 | **At-least-once delivery** | Every message is delivered at least once; exactly-once as an optional higher guarantee |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Throughput** | 1 million messages/sec (write), 2 million messages/sec (read) |
| NFR2 | **Latency** | < 10ms end-to-end (producer publish to consumer receive) for p99 |
| NFR3 | **Durability** | Zero message loss for acknowledged writes |
| NFR4 | **Availability** | 99.99% for the write path |
| NFR5 | **Scalability** | Horizontally scalable to thousands of topics and petabytes of data |
| NFR6 | **Message size** | Up to 1 MB per message (configurable) |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Messages/sec (write) | 1,000,000 |
| Average message size | 1 KB |
| Write throughput | 1 GB/sec |
| Replication factor | 3 → 3 GB/sec total disk write |
| Retention | 7 days |
| Storage for 7 days | 1 GB/sec * 86,400 * 7 = ~600 TB (before compression) |
| With compression (4x) | ~150 TB |
| Brokers (10 TB usable each) | ~15 brokers minimum |

---

#### Step 3 — High-Level Architecture

```
┌──────────────────┐     ┌──────────────────┐
│   Producer A     │     │   Producer B     │
└────────┬─────────┘     └────────┬─────────┘
         │                        │
         │    messages partitioned by key
         │    (or round-robin if no key)
         │                        │
         ▼                        ▼
┌────────────────────────────────────────────────────┐
│                   Broker Cluster                    │
│                                                    │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐   │
│  │  Broker 0  │  │  Broker 1  │  │  Broker 2  │   │
│  │            │  │            │  │            │   │
│  │ Topic-A    │  │ Topic-A    │  │ Topic-A    │   │
│  │  Part 0   │  │  Part 1   │  │  Part 2   │   │
│  │  (leader)  │  │  (leader)  │  │  (leader)  │   │
│  │            │  │            │  │            │   │
│  │ Topic-A    │  │ Topic-A    │  │ Topic-A    │   │
│  │  Part 1   │  │  Part 0   │  │  Part 0   │   │
│  │  (replica) │  │  (replica) │  │  (replica) │   │
│  └────────────┘  └────────────┘  └────────────┘   │
│                                                    │
└───────────────────────┬────────────────────────────┘
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ▼             ▼             ▼
   ┌────────────┐┌────────────┐┌────────────┐
   │ Consumer   ││ Consumer   ││ Consumer   │
   │ Group A    ││ Group A    ││ Group B    │
   │ Instance 1 ││ Instance 2 ││ Instance 1 │
   └────────────┘└────────────┘└────────────┘

┌─────────────────────────────────────────────┐
│  Coordination Service (ZooKeeper / KRaft)   │
│  - Broker membership                        │
│  - Partition leader election                │
│  - Consumer group coordination              │
└─────────────────────────────────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: The Commit Log — Core Storage Abstraction

A topic is divided into **partitions**. Each partition is an **ordered, immutable, append-only commit log** stored on disk.

```
Partition 0:
┌──────┬──────┬──────┬──────┬──────┬──────┬──────┬─────┐
│ Off 0│ Off 1│ Off 2│ Off 3│ Off 4│ Off 5│ Off 6│ ... │
│ msg  │ msg  │ msg  │ msg  │ msg  │ msg  │ msg  │     │
└──────┴──────┴──────┴──────┴──────┴──────┴──────┴─────┘
                        ▲                    ▲
                        │                    │
                  Consumer A            Consumer B
                  (offset=3)            (offset=6)
```

**On-disk structure per partition:**

```
/data/topic-A/partition-0/
    00000000000000000000.log        ← segment file (contains messages)
    00000000000000000000.index      ← sparse offset-to-position index
    00000000000000000000.timeindex  ← timestamp-to-offset index
    00000000000005242880.log        ← next segment (after the first fills up)
    00000000000005242880.index
    ...
```

**Why sequential disk I/O is fast enough:**

- Messages are appended sequentially — no random seeks on writes
- The OS page cache handles read caching transparently
- Segment files are read sequentially by consumers
- Sequential disk throughput on modern SSDs: 500 MB/sec+; on HDDs: 100+ MB/sec
- This means disk is not the bottleneck — network usually is

**Zero-copy transfer (sendfile):**

When a consumer reads messages, the broker uses the `sendfile()` system call to transfer data directly from the page cache to the network socket, bypassing user-space memory entirely. This reduces CPU usage and memory copies dramatically.

```
Traditional:  disk → page cache → application buffer → socket buffer → NIC
Zero-copy:    disk → page cache → ────────────────────→ NIC
```

##### Deep Dive B: Partitioning and Ordering

**Partition assignment:**

```
Producer decides the partition:
  1. Key specified:   partition = hash(key) % num_partitions
  2. No key:          round-robin across partitions (or sticky partitioning)
```

**Ordering guarantee:**

- Messages with the same key always go to the same partition → **strictly ordered**
- Messages across partitions have **no ordering guarantee**
- This is a fundamental trade-off: more partitions = more parallelism, but ordering only within a partition

**Example: Order events**

```
Key = order_id = "ORD-123"
  → hash("ORD-123") % 6 = partition 2
  → All events for ORD-123 (created, paid, shipped, delivered) land in partition 2
  → Consumer processes them in order
```

**How many partitions?**

| Factor | Guidance |
|--------|----------|
| Target throughput | If one partition sustains 10 MB/sec and you need 100 MB/sec → at least 10 partitions |
| Consumer parallelism | Number of partitions = max consumers in a group (one partition per consumer) |
| Over-partitioning cost | More partitions = more open file handles, more memory, slower leader elections |
| Rule of thumb | Start with `max(throughput_need / per_partition_throughput, expected_consumer_count)` |

##### Deep Dive C: Replication and Durability

```
Topic-A, Partition 0, Replication Factor = 3:

  Broker 0 (Leader):   [msg0, msg1, msg2, msg3, msg4, msg5]
  Broker 1 (Follower): [msg0, msg1, msg2, msg3, msg4]        ← slightly behind
  Broker 2 (Follower): [msg0, msg1, msg2, msg3, msg4, msg5]  ← caught up

  ISR (In-Sync Replicas) = {Broker 0, Broker 2}
  Broker 1 is lagging → temporarily out of ISR
```

**Key concepts:**

| Concept | Description |
|---------|-------------|
| **Leader** | One broker is the leader for each partition; all reads and writes go through the leader |
| **Followers** | Replicas that continuously fetch from the leader to stay in sync |
| **ISR (In-Sync Replicas)** | The set of replicas that are "caught up" within a configurable lag threshold |
| **High Watermark** | The offset up to which all ISR members have replicated — only messages below this offset are visible to consumers |

**Producer acknowledgment modes:**

| `acks` setting | Behavior | Durability | Latency |
|----------------|----------|------------|---------|
| `acks=0` | Fire and forget; don't wait for any ack | Lowest (may lose data) | Fastest |
| `acks=1` | Wait for leader to write to its local log | Moderate (leader crash before replication = loss) | Low |
| `acks=all` | Wait for all ISR members to acknowledge | Highest (survives any single broker failure) | Highest |

**Leader election on failure:**

```
1. Leader (Broker 0) crashes
2. Controller detects failure (via ZooKeeper session timeout or KRaft heartbeat)
3. Controller selects a new leader from the ISR (e.g., Broker 2)
4. Broker 2 becomes the new leader
5. Producers and consumers are redirected (via metadata refresh)
6. If ISR is empty (all replicas down), either:
   a. Wait for an ISR member to come back (higher availability gap)
   b. Elect any replica, even out-of-sync (risk of data loss) — configurable
```

##### Deep Dive D: Consumer Groups and Offset Management

```
Topic-A has 4 partitions: P0, P1, P2, P3

Consumer Group "order-service" (3 instances):
  Consumer 1 → P0, P1
  Consumer 2 → P2
  Consumer 3 → P3

Consumer Group "analytics" (2 instances):
  Consumer 4 → P0, P1
  Consumer 5 → P2, P3
```

Each consumer group independently tracks its position (offset) per partition. Adding/removing consumers triggers a **rebalance** that reassigns partitions.

**Offset storage:**

Consumers periodically commit their current offset to a special internal topic (`__consumer_offsets`). On restart, the consumer resumes from its last committed offset.

```
Commit strategies:
  - Auto-commit: every N seconds (simple, but may re-process messages after crash)
  - Manual commit after processing: at-least-once (re-process on crash)
  - Manual commit before processing: at-most-once (may lose messages on crash)
```

**Exactly-once semantics (transactional):**

For exactly-once processing (e.g., consume from topic A, process, produce to topic B):

```
1. Producer sends messages in a transaction (idempotent producer + transaction ID)
2. Broker deduplicates using (producer_id, sequence_number)
3. Consumer reads only committed messages (isolation.level=read_committed)
4. Offset commit and output message are part of the same atomic transaction
```

##### Deep Dive E: Eliminating ZooKeeper (KRaft)

Traditional Kafka depends on ZooKeeper for metadata, leader election, and cluster coordination. KRaft replaces ZooKeeper with a Raft-based consensus protocol built into the brokers themselves.

| Aspect | ZooKeeper | KRaft |
|--------|-----------|-------|
| Dependency | External system to operate | Self-contained |
| Metadata storage | ZooKeeper znodes | Internal `__cluster_metadata` topic |
| Max partitions | ~200K (ZooKeeper bottleneck) | Millions |
| Operational complexity | Two systems to manage | One system |
| Leader election speed | Seconds | Sub-second |

---

#### Step 5 — Scaling, Reliability, and Operations

| Concern | Solution |
|---------|----------|
| **Throughput scaling** | Add partitions to a topic (consumers scale linearly). Add brokers and rebalance partition leaders. |
| **Storage scaling** | Add brokers with more disk. Tiered storage: hot data on local SSD, cold data offloaded to S3. |
| **Broker failure** | ISR-based replication; leader election from ISR. No data loss if `acks=all` and `min.insync.replicas >= 2`. |
| **Consumer lag** | Monitor consumer lag (offset delta between latest and committed). Alert if lag grows. Scale consumer instances. |
| **Backpressure** | Producers back off on broker overload (retry with backoff). Consumers process at their own pace (messages are retained). |
| **Multi-datacenter** | MirrorMaker 2 / Cluster Linking to replicate topics across datacenters. Active-passive or active-active. |
| **Monitoring** | Track under-replicated partitions, ISR shrinks, consumer lag, request latency, disk usage. |
| **Data retention** | Time-based (7 days) or size-based (100 GB per partition). Compacted topics for changelog use cases. |

**Log compaction (for changelog/state topics):**

```
Before compaction:
  key=A, val=1  |  key=B, val=2  |  key=A, val=3  |  key=B, val=4  |  key=A, val=5

After compaction:
  key=B, val=4  |  key=A, val=5
  (only the latest value per key is retained)
```

Used for: consumer offset storage, database CDC (change data capture), materialized view state.

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How is this different from RabbitMQ/SQS?" | Traditional queues delete messages on acknowledgment and don't support replay. Kafka retains messages and allows consumers to seek freely. Kafka is an event log; RabbitMQ is a task queue. |
| "How would you implement dead letter queues?" | After N failed processing attempts, publish the message to a DLQ topic. A separate consumer or manual process inspects and reprocesses DLQ messages. |
| "How would you handle message schema evolution?" | Use a Schema Registry (Avro, Protobuf, JSON Schema). Producers register schemas; consumers validate compatibility. Supports backward/forward compatibility. |
| "How would you build event sourcing on top of this?" | Each aggregate's events go to a compacted topic keyed by aggregate ID. Replay the topic to rebuild state. Use Kafka Streams or a consumer to maintain materialized views. |

---

### Solution 14: Design a Monitoring and Alerting System (Grafana + Prometheus)

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

### Solution 15: Design a Distributed Task Scheduler

> **Difficulty:** Staff Level
> **Time allocation:** 45-60 minutes
> **Real-world references:** Airflow, Celery Beat, Kubernetes CronJob, AWS Step Functions, Temporal, Quartz

---

#### Opening Statement

> "A distributed task scheduler orchestrates the execution of tasks — both one-time and recurring — across a fleet of workers. The key challenges are reliable execution guarantees, exactly-once scheduling of cron jobs, distributed locking, and graceful failure handling. Let me nail down the scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **One-time tasks** | Schedule a task to execute at a specific future time |
| FR2 | **Recurring tasks** | Schedule tasks using cron expressions (e.g., "every 5 minutes", "daily at 3 AM UTC") |
| FR3 | **Task priorities** | High-priority tasks are executed before low-priority ones |
| FR4 | **Retry with backoff** | Failed tasks are retried with configurable strategy |
| FR5 | **Task dependencies** | A task can depend on the completion of other tasks (DAG execution) |
| FR6 | **Execution history** | Full log of task executions: status, duration, output, errors |
| FR7 | **Distributed execution** | Tasks are distributed across a pool of workers |
| FR8 | **Cancellation** | Running or scheduled tasks can be cancelled |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **At-least-once execution** | Every scheduled task runs at least once (never silently dropped) |
| NFR2 | **No duplicate cron execution** | A cron job scheduled for 3 AM runs exactly once, even with multiple scheduler instances |
| NFR3 | **Scalability** | Support 100K+ scheduled tasks and 10K concurrent executions |
| NFR4 | **Execution accuracy** | Tasks fire within 1 second of their scheduled time |
| NFR5 | **Availability** | 99.99% — scheduler failures must not permanently lose scheduled tasks |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Scheduled tasks (stored) | 100,000 |
| Tasks triggered per minute (peak) | 10,000 |
| Average task execution time | 30 seconds |
| Concurrent running tasks | 5,000 |
| Workers | 200 (25 tasks each concurrently) |
| Task record size | ~1 KB |
| Execution history/day | 10K/min * 60 * 24 = ~14.4M records/day |

---

#### Step 3 — High-Level Architecture

```
┌──────────────────────────────────────────────────────────────┐
│  Clients (API / UI / CLI)                                    │
│  - Create, update, delete, cancel tasks                      │
│  - View execution history and status                         │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│  API Service (Stateless)                                     │
│  - CRUD for task definitions                                 │
│  - Validates cron expressions and task configs                │
└───────────────────────────┬──────────────────────────────────┘
                            │
              ┌─────────────┼──────────────┐
              │             │              │
              ▼             ▼              ▼
┌──────────────────┐ ┌────────────┐ ┌────────────────────────────┐
│  Task Store      │ │  Lock      │ │  Scheduler Instances       │
│  (PostgreSQL)    │ │  Service   │ │  (N replicas, HA)          │
│                  │ │  (Redis /  │ │                            │
│  - definitions   │ │  etcd)     │ │  ┌──────────────────────┐  │
│  - schedules     │ │            │ │  │  Tick Engine         │  │
│  - exec history  │ └────────────┘ │  │  (polls every 1 sec) │  │
└──────────────────┘                │  └──────────┬───────────┘  │
                                    │             │              │
                                    │  ┌──────────▼───────────┐  │
                                    │  │  Enqueue tasks due   │  │
                                    │  │  for execution       │  │
                                    │  └──────────┬───────────┘  │
                                    └─────────────┼──────────────┘
                                                  │
                                                  ▼
                                    ┌──────────────────────────┐
                                    │  Task Queue              │
                                    │  (Kafka / Redis Streams  │
                                    │   with priority lanes)   │
                                    └─────────────┬────────────┘
                                                  │
                           ┌──────────────────────┼──────────────────┐
                           │                      │                  │
                           ▼                      ▼                  ▼
                    ┌─────────────┐        ┌─────────────┐    ┌─────────────┐
                    │  Worker 1   │        │  Worker 2   │    │  Worker N   │
                    │  - Pull task│        │  - Pull task│    │  - Pull task│
                    │  - Execute  │        │  - Execute  │    │  - Execute  │
                    │  - Report   │        │  - Report   │    │  - Report   │
                    └─────────────┘        └─────────────┘    └─────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: The Tick Engine — Ensuring Tasks Fire On Time

The scheduler must continuously scan for tasks that are due and enqueue them.

**Approach: Database polling with partitioned ownership**

```
Every 1 second, each scheduler instance:

1. Acquire ownership of a shard of tasks (by hash range):
   Scheduler 0 owns tasks where hash(task_id) % N == 0
   Scheduler 1 owns tasks where hash(task_id) % N == 1
   ...

2. Query for tasks due in this shard:
   SELECT * FROM tasks
   WHERE shard = :my_shard
     AND next_run_at <= NOW()
     AND status = 'scheduled'
   ORDER BY priority DESC, next_run_at ASC
   LIMIT 1000
   FOR UPDATE SKIP LOCKED;

3. For each task:
   a. Set status = 'enqueued'
   b. Compute next_run_at (from cron expression) and update
   c. Publish to task queue
```

**`FOR UPDATE SKIP LOCKED`** is critical — it allows multiple scheduler instances to scan concurrently without blocking each other or double-picking.

**Why not a delay queue / timer wheel?**

| Approach | Pros | Cons |
|----------|------|------|
| **DB polling** | Simple, durable, tasks survive restarts | Polling overhead, slightly less precise |
| **Timer wheel (in-memory)** | Very precise, low overhead | State lost on crash; need WAL or replication |
| **Delay queue (Redis ZSET)** | Fast, sorted by due time | Redis memory limits; less durable than DB |

**Recommendation:** Database polling for durability at staff level. Use Redis ZSET as a secondary acceleration layer for sub-second precision when needed.

##### Deep Dive B: Exactly-Once Cron Scheduling

The hardest problem: ensuring a cron job fires exactly once per scheduled interval, even with N scheduler instances.

**Approach: Idempotent schedule expansion**

```
Cron: "0 * * * *" (every hour)
Current time: 14:00:03

1. Scheduler computes: next_run_at for this cron = 14:00:00
2. Creates an execution record with a unique key:
   idempotency_key = "task_123:2024-01-15T14:00:00"
3. INSERT ... ON CONFLICT (idempotency_key) DO NOTHING

If two schedulers try to create the same execution at 14:00 → only one succeeds.
The "losing" scheduler sees a conflict and does nothing.
```

**Schema:**

```sql
CREATE TABLE task_executions (
    id               UUID PRIMARY KEY,
    task_id          UUID NOT NULL REFERENCES tasks(id),
    idempotency_key  VARCHAR(255) UNIQUE NOT NULL,
    status           VARCHAR(20) NOT NULL DEFAULT 'pending',
    scheduled_at     TIMESTAMPTZ NOT NULL,
    started_at       TIMESTAMPTZ,
    completed_at     TIMESTAMPTZ,
    worker_id        VARCHAR(100),
    attempt          INTEGER NOT NULL DEFAULT 1,
    output           TEXT,
    error            TEXT,
    created_at       TIMESTAMPTZ NOT NULL
);
```

##### Deep Dive C: Worker Execution Model

```
Worker lifecycle:

1. Pull task from queue (blocking pop with timeout)
2. Send heartbeat: "I'm working on task X" (every 10 seconds)
3. Execute the task (invoke user function, HTTP call, container, etc.)
4. On success: update status = 'succeeded', report result
5. On failure: update status = 'failed', report error
6. If max retries not reached: re-enqueue with backoff delay
```

**Heartbeats and dead worker detection:**

```
- Each worker writes heartbeats to Redis: SET worker:{id}:task:{exec_id} TTL=30s
- A monitor process scans for expired heartbeats
- If heartbeat expires → worker assumed dead → task re-enqueued
```

**Retry strategy:**

```
retry_delay = base_delay * (2 ^ attempt) + random_jitter

Attempt 1: 1s  + jitter
Attempt 2: 2s  + jitter
Attempt 3: 4s  + jitter
Attempt 4: 8s  + jitter
...
Max retries: configurable per task (default: 3)
After max retries: mark as 'permanently_failed', alert owner
```

##### Deep Dive D: Task Dependencies (DAG Execution)

For complex workflows, tasks can form a Directed Acyclic Graph:

```
        ┌─────┐
        │  A  │
        └──┬──┘
       ┌───┴───┐
       ▼       ▼
    ┌─────┐ ┌─────┐
    │  B  │ │  C  │
    └──┬──┘ └──┬──┘
       └───┬───┘
           ▼
        ┌─────┐
        │  D  │  (runs only after B AND C succeed)
        └─────┘
```

**Implementation:**

```sql
CREATE TABLE task_dependencies (
    task_id          UUID REFERENCES tasks(id),
    depends_on       UUID REFERENCES tasks(id),
    PRIMARY KEY (task_id, depends_on)
);
```

**Execution logic:**

```
When a task completes:
  1. Find all tasks that depend on it
  2. For each dependent task:
     a. Check if ALL its dependencies are now 'succeeded'
     b. If yes → enqueue the dependent task
     c. If any dependency 'failed' → optionally fail the dependent (configurable)
```

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **Scheduler HA** | N scheduler instances, each owns a shard of tasks. If one dies, its shard is redistributed (via consistent hashing or DB-based lease). |
| **Worker scaling** | Stateless workers; scale horizontally based on queue depth. Auto-scale using queue lag metrics. |
| **Clock drift** | Use NTP-synchronized clocks. Store all times in UTC. For cron evaluation, use a well-tested library. |
| **Queue backpressure** | If workers can't keep up, queue grows. Alert on queue depth. Priority lanes ensure critical tasks run first. |
| **Long-running tasks** | Workers hold a lease (via heartbeat). If a task runs for hours, the heartbeat keeps it alive. Set a max execution timeout. |
| **Poison pill tasks** | If a task fails immediately on every attempt (bad config, OOM), mark as permanently failed after max retries. Don't let it clog the queue. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How is this different from Airflow?" | Airflow is primarily a DAG scheduler for batch data pipelines with a Python DSL. This design is a general-purpose task scheduler for any workload. Airflow's scheduler is single-active (HA is limited); this design shards scheduling. |
| "How would you handle tasks across time zones?" | Store all schedules in UTC internally. Convert at the API layer. Handle DST transitions carefully (a cron job at 2:30 AM might not exist during spring-forward). |
| "How would you add rate limiting to task execution?" | Per-task-type rate limiter (token bucket in Redis). Workers check the rate limiter before executing. If rate exceeded, re-enqueue with a short delay. |
| "How would you support task chaining without DAGs?" | Return value of task A is passed as input to task B. Implemented as a lightweight dependency: B depends on A, and A's output is stored and forwarded. Similar to Celery chains. |

---

### Solution 16: Design a Global-Scale Payment System

> **Difficulty:** Staff Level
> **Time allocation:** 45-60 minutes
> **Real-world references:** Stripe, PayPal, Square, Adyen, internal payment platforms at Amazon/Uber

---

#### Opening Statement

> "A payment system is one of the most challenging distributed systems to design because the cost of failure is measured in dollars, trust, and regulatory penalties. Correctness trumps performance. The system must be exactly-once for charges, fully auditable, and compliant with financial regulations. Let me clarify scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Accept payments** | Process credit/debit card payments, bank transfers, and digital wallets |
| FR2 | **Idempotent charges** | Every payment request has an idempotency key — retries must not double-charge |
| FR3 | **Refunds** | Full and partial refunds |
| FR4 | **Multi-currency** | Support payments in 50+ currencies with real-time exchange rates |
| FR5 | **Payment status** | Track payment lifecycle: created → processing → succeeded / failed |
| FR6 | **Webhooks** | Notify merchants of payment events (succeeded, failed, refunded) |
| FR7 | **Ledger** | Double-entry bookkeeping for every money movement |
| FR8 | **Reconciliation** | Daily reconciliation between internal ledger and bank/PSP statements |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Correctness** | Zero double-charges, zero lost payments — this is non-negotiable |
| NFR2 | **Availability** | 99.99% for the payment API |
| NFR3 | **Latency** | < 2 seconds for payment processing (p99) |
| NFR4 | **Throughput** | 10,000 transactions per second (TPS) peak |
| NFR5 | **Auditability** | Every state change is logged immutably |
| NFR6 | **Compliance** | PCI DSS Level 1, SOX, PSD2 (EU), regional regulations |
| NFR7 | **Global** | Operate across multiple regions with data residency requirements |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Peak TPS | 10,000 |
| Average transaction record | ~2 KB |
| Transactions/day | 10K TPS * 86,400 = ~864M/day (peak); realistic avg: ~200M/day |
| Storage/day (transactions) | 200M * 2 KB = ~400 GB |
| Ledger entries/day (2 per txn) | ~400M entries, ~800 GB |
| Storage/year | ~150 TB (transactions + ledger + audit log) |

---

#### Step 3 — High-Level Architecture

```
┌──────────────────┐
│  Merchant / App  │
│  (API Client)    │
└────────┬─────────┘
         │  POST /v1/payments  (idempotency key in header)
         ▼
┌────────────────────────────────────────────────────────────┐
│  API Gateway                                               │
│  - Authentication, rate limiting, TLS termination           │
│  - Route to Payment Service                                │
└────────────────────────┬───────────────────────────────────┘
                         │
                         ▼
┌────────────────────────────────────────────────────────────┐
│  Payment Service (Orchestrator)                            │
│                                                            │
│  1. Idempotency check (has this request been seen before?) │
│  2. Validate payment details                               │
│  3. Risk / Fraud check                                     │
│  4. Route to Payment Service Provider (PSP)                │
│  5. Record result in ledger                                │
│  6. Send webhook to merchant                               │
└────────┬──────────┬──────────┬──────────┬──────────────────┘
         │          │          │          │
         ▼          ▼          ▼          ▼
  ┌───────────┐ ┌────────┐ ┌────────┐ ┌───────────────────┐
  │ Idempotency│ │ Risk   │ │ PSP    │ │ Ledger Service    │
  │ Store     │ │ Engine │ │ Router │ │ (double-entry)    │
  │ (Redis +  │ │        │ │        │ └─────────┬─────────┘
  │  DB)      │ │ ML +   │ │Stripe, │           │
  └───────────┘ │ Rules  │ │Adyen,  │    ┌──────▼──────┐
                └────────┘ │Bank API│    │  Ledger DB  │
                           └───┬────┘    │ (PostgreSQL │
                               │         │  append-only)│
                               ▼         └─────────────┘
                    ┌────────────────┐
                    │  External PSPs │
                    │  / Banks       │
                    └────────────────┘

┌────────────────────────────────────────────────┐
│  Async Workers                                  │
│  - Webhook delivery (with retry)               │
│  - Reconciliation (batch, daily)               │
│  - Settlement processing                       │
│  - Reporting and analytics                     │
└────────────────────────────────────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Idempotency — Preventing Double Charges

This is the single most critical design element. Network failures, client retries, and load balancer retries can all cause a payment request to arrive multiple times.

**Mechanism:**

```
1. Client sends: POST /v1/payments
   Header: Idempotency-Key: "ik_abc123"

2. Payment Service checks idempotency store:
   a. Key exists AND status = 'succeeded' → return cached response (200 OK)
   b. Key exists AND status = 'processing' → return 409 Conflict (in-flight)
   c. Key not found → proceed with payment, store key with status='processing'

3. After PSP response:
   a. Update idempotency record: status='succeeded' or 'failed', store response
   b. Return response to client
```

**Storage:**

```sql
CREATE TABLE idempotency_keys (
    key              VARCHAR(255) PRIMARY KEY,
    merchant_id      UUID NOT NULL,
    request_hash     VARCHAR(64) NOT NULL,
    status           VARCHAR(20) NOT NULL,
    response_code    INTEGER,
    response_body    JSONB,
    created_at       TIMESTAMPTZ NOT NULL,
    expires_at       TIMESTAMPTZ NOT NULL
);
```

- TTL of 24-48 hours (clients should not reuse idempotency keys after this)
- The `request_hash` ensures the same key can't be reused with different payment parameters (prevents misuse)

**Database transaction wrapping:**

```
BEGIN TRANSACTION;
  INSERT INTO idempotency_keys (key, status) VALUES ('ik_abc123', 'processing')
    ON CONFLICT DO NOTHING;
  -- if insert succeeded (rows affected = 1): continue
  -- if conflict (rows affected = 0): another request is handling this; return
  
  INSERT INTO payments (...) VALUES (...);
  INSERT INTO ledger_entries (...) VALUES (...);
COMMIT;
```

The idempotency insert and the payment record creation happen in the same transaction — no window for inconsistency.

##### Deep Dive B: Double-Entry Ledger

Every money movement is recorded as two balanced entries. This is how banks and financial systems have worked for centuries.

```
Payment of $100 from customer to merchant:

┌──────────────┬──────────────┬────────┬────────┐
│ Account      │ Entry Type   │ Amount │ Txn ID │
├──────────────┼──────────────┼────────┼────────┤
│ customer_123 │ DEBIT        │ $100   │ tx_001 │
│ merchant_456 │ CREDIT       │ $100   │ tx_001 │
└──────────────┴──────────────┴────────┴────────┘

Invariant: SUM(debits) = SUM(credits) — ALWAYS.
```

**With fees:**

```
Customer pays $100, platform takes 2.9% + $0.30:

│ customer_123       │ DEBIT  │ $100.00 │ tx_001 │
│ platform_fees      │ CREDIT │ $3.20   │ tx_001 │  (2.9% + $0.30)
│ merchant_456       │ CREDIT │ $96.80  │ tx_001 │
```

**Ledger schema (append-only, immutable):**

```sql
CREATE TABLE ledger_entries (
    id               BIGSERIAL PRIMARY KEY,
    transaction_id   UUID NOT NULL,
    account_id       UUID NOT NULL,
    entry_type       VARCHAR(6) NOT NULL CHECK (entry_type IN ('DEBIT', 'CREDIT')),
    amount           BIGINT NOT NULL,
    currency         CHAR(3) NOT NULL,
    description      TEXT,
    created_at       TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- CRITICAL: amount stored in smallest unit (cents) as BIGINT
-- $100.00 → 10000 (avoids floating point errors)
-- No UPDATE or DELETE allowed on this table (append-only)
```

**Refund as a reversal:**

```
Refund of $100 payment:

│ merchant_456 │ DEBIT  │ $96.80  │ tx_002 │  (reverse the credit)
│ platform_fees│ DEBIT  │ $3.20   │ tx_002 │  (reverse the fee)
│ customer_123 │ CREDIT │ $100.00 │ tx_002 │  (money back to customer)
```

Never modify the original entries — always create new reversal entries. This maintains the full audit trail.

##### Deep Dive C: Payment State Machine

```
                ┌──────────┐
                │ CREATED  │
                └────┬─────┘
                     │  validate, fraud check
                     ▼
                ┌──────────┐
           ┌────│PROCESSING│────┐
           │    └──────────┘    │
           │                    │
    PSP succeeded          PSP declined
           │                    │
           ▼                    ▼
    ┌──────────┐         ┌──────────┐
    │SUCCEEDED │         │ FAILED   │
    └────┬─────┘         └──────────┘
         │
         │  merchant requests refund
         ▼
    ┌──────────┐
    │REFUNDING │
    └────┬─────┘
         │
    ┌────┴─────┐
    ▼          ▼
┌────────┐ ┌────────────┐
│REFUNDED│ │REFUND_FAILED│
└────────┘ └────────────┘
```

Every state transition is:
1. Validated (can only move along defined edges)
2. Persisted atomically with the ledger entry
3. Published as an event (for webhooks, analytics)
4. Audited (who, when, why)

##### Deep Dive D: PSP Routing and Failover

When a payment is processed, it's sent to an external Payment Service Provider (Stripe, Adyen, bank API). The PSP Router makes intelligent decisions:

```
PSP Selection Logic:
  1. Card type + region → eligible PSPs (Visa in EU → Adyen or Stripe)
  2. Cost optimization → cheapest PSP for this transaction type
  3. Success rate → route away from PSPs with degraded success rates
  4. Failover → if primary PSP fails, retry with secondary
  5. Load balancing → spread across PSPs to avoid concentration risk
```

**Handling PSP ambiguity (the hardest failure):**

```
Scenario: We send a charge to Stripe, network times out. Did Stripe charge the card?

Solution:
  1. We assigned our own idempotency key to the Stripe request
  2. We retry the same request to Stripe with the same key
  3. Stripe's idempotency guarantees:
     - If the original succeeded → returns the same success response
     - If the original never arrived → processes it now
  4. We never send to a DIFFERENT PSP after a timeout (would double-charge)
```

This is why idempotency keys flow end-to-end: client → our system → PSP.

##### Deep Dive E: Reconciliation

Daily reconciliation ensures our ledger matches reality.

```
┌─────────────────┐     ┌──────────────────────────┐
│ Internal Ledger │     │ PSP Settlement Report    │
│ (our records)   │     │ (Stripe daily report)    │
└────────┬────────┘     └───────────┬──────────────┘
         │                          │
         └──────────┬───────────────┘
                    │
              ┌─────▼──────┐
              │ Reconciler │
              │ (batch job)│
              └─────┬──────┘
                    │
         ┌──────────┼──────────┐
         │          │          │
    Matched    Unmatched    Unmatched
    (OK)       (ours only)  (PSP only)
                    │          │
              Investigate  Investigate
              (missed      (charge we
               webhook?)    don't know about?)
```

Mismatches are flagged for manual review. Common causes: delayed webhooks, timezone boundary issues, currency conversion rounding.

---

#### Step 5 — Scaling, Reliability, and Compliance

| Concern | Solution |
|---------|----------|
| **Data residency** | EU payments processed and stored in EU region. Route based on card issuer country. Separate database clusters per region. |
| **PCI DSS compliance** | Never store raw card numbers. Use PSP tokenization. The cardholder data environment (CDE) is isolated with strict network controls. |
| **High availability** | Multi-AZ deployment. Active-passive database with synchronous replication (zero data loss). If primary AZ fails, promote standby. |
| **Exactly-once semantics** | Idempotency keys end-to-end. Database transactions for state changes. Ledger append-only (no updates). |
| **Monitoring** | Track: payment success rate (by PSP, by card type, by region), latency p50/p99, failed reconciliation count, PSP error rates. |
| **Disaster recovery** | RPO = 0 (no data loss). RTO < 5 minutes. Tested quarterly with chaos engineering. |
| **Fraud prevention** | Real-time ML scoring on payment attributes (amount, velocity, device fingerprint, geo). Block or 3D-Secure challenge if risk score exceeds threshold. |
| **Cost optimization** | Route to cheapest PSP for each transaction type while maintaining success rate SLO. A/B test PSP routing changes. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How do you handle currency conversion?" | Convert at the PSP (they handle the FX). Record both the original and converted amounts in the ledger. Lock the exchange rate at the time of charge. |
| "How do you handle partial captures (auth then capture)?" | Two-step: authorize (hold funds) → capture (settle). Authorization expires after ~7 days. Supports partial captures for e-commerce (ship partial order). |
| "What happens if reconciliation finds a mismatch?" | Alert the finance ops team. Common resolutions: re-fetch the webhook, adjust ledger with a correction entry (never mutate existing entries), escalate to PSP support. |
| "How would you add subscriptions / recurring billing?" | Separate subscription service with a state machine (active, past_due, cancelled). Scheduler triggers charges on billing dates. Handle card expiry, retry logic for declined charges, dunning emails. |

---

### Solution 17: Design a Multi-Tenant SaaS Platform

> **Difficulty:** Staff Level
> **Time allocation:** 45-60 minutes
> **Real-world references:** Salesforce, Slack, Atlassian, AWS (internal multi-tenancy), Grafana Cloud

---

#### Opening Statement

> "Multi-tenancy is an architectural decision with deep implications for data isolation, performance, cost, and operational complexity. The fundamental question is: how do we serve hundreds or thousands of customers on shared infrastructure while making each one feel like they have their own dedicated system? Let me clarify the context."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Tenant onboarding** | Self-service signup creates an isolated tenant with its own workspace |
| FR2 | **Data isolation** | Tenant A can never see or access Tenant B's data |
| FR3 | **Per-tenant configuration** | Custom branding, feature flags, retention policies per tenant |
| FR4 | **Tiered plans** | Free, Pro, Enterprise with different limits and features |
| FR5 | **Admin panel** | Platform operators can manage tenants, view usage, disable accounts |
| FR6 | **RBAC per tenant** | Each tenant has its own users, roles, and permissions |
| FR7 | **API access** | Each tenant gets API keys scoped to their data |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Noisy neighbor protection** | One tenant's heavy usage must not degrade others |
| NFR2 | **Scale** | Support 10,000+ tenants (ranging from 1 user to 100K users) |
| NFR3 | **Data residency** | Enterprise tenants may require data stored in a specific region |
| NFR4 | **Cost efficiency** | Small tenants must be cheap to serve (can't provision per-tenant infra for everyone) |
| NFR5 | **Availability** | 99.9% for all tenants; 99.99% SLA for Enterprise tier |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Total tenants | 10,000 |
| Tenant size distribution | 90% small (< 100 users), 9% medium (100-10K), 1% large (10K-100K) |
| Total users across all tenants | ~2 million |
| API QPS (total) | 50,000 |
| Top 1% of tenants generate | ~50% of total QPS |
| Storage per small tenant | ~100 MB |
| Storage per large tenant | ~500 GB |

---

#### Step 3 — High-Level Architecture

```
┌───────────────────────────────────────────────────────────────────┐
│  Tenant Users (browsers, API clients)                             │
└────────────────────────────┬──────────────────────────────────────┘
                             │
                             ▼
┌───────────────────────────────────────────────────────────────────┐
│  Edge / API Gateway                                               │
│  - TLS termination                                               │
│  - Tenant identification (from subdomain, API key, or JWT)        │
│  - Per-tenant rate limiting                                      │
│  - Route to correct regional cluster                             │
└────────────────────────────┬──────────────────────────────────────┘
                             │
                             ▼
┌───────────────────────────────────────────────────────────────────┐
│  Application Layer (Stateless)                                    │
│                                                                   │
│  ┌─────────────────┐  ┌──────────────────┐  ┌────────────────┐   │
│  │ Tenant Context  │  │ Feature Flag     │  │ RBAC           │   │
│  │ Middleware      │  │ Service          │  │ Service        │   │
│  │ (injects        │  │ (per-tenant      │  │ (per-tenant    │   │
│  │  tenant_id into │  │  feature gates)  │  │  roles/perms)  │   │
│  │  every request) │  │                  │  │                │   │
│  └─────────────────┘  └──────────────────┘  └────────────────┘   │
└────────────────────────────┬──────────────────────────────────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
    ┌──────────────┐  ┌───────────┐  ┌──────────────┐
    │ Shared Pool  │  │ Dedicated │  │ Shared Queue │
    │ Database     │  │ Database  │  │ (Kafka)      │
    │ (small/med   │  │ (large    │  │              │
    │  tenants)    │  │  tenants) │  │              │
    └──────────────┘  └───────────┘  └──────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Tenant Data Isolation Models

The most critical architectural decision. Three models exist on a spectrum:

**Model 1: Shared Database, Shared Schema (with tenant_id column)**

```sql
CREATE TABLE projects (
    id          UUID PRIMARY KEY,
    tenant_id   UUID NOT NULL,      ← every table has this
    name        VARCHAR(255),
    ...
);

-- Every query MUST include tenant_id:
SELECT * FROM projects WHERE tenant_id = 'tenant_123' AND ...

-- Row-Level Security (PostgreSQL):
CREATE POLICY tenant_isolation ON projects
    USING (tenant_id = current_setting('app.current_tenant')::UUID);
```

| Pros | Cons |
|------|------|
| Most cost-efficient (one DB for all tenants) | One bug in a query can leak data across tenants |
| Simple operations (one DB to backup, monitor) | Noisy neighbor risk (one tenant's query slows all) |
| Easy cross-tenant analytics for the platform | Schema migrations affect all tenants at once |

**Model 2: Shared Database, Separate Schema**

```
Database: saas_platform

Schema: tenant_123
  - projects
  - users
  - settings

Schema: tenant_456
  - projects
  - users
  - settings
```

| Pros | Cons |
|------|------|
| Stronger isolation than shared schema | Thousands of schemas = operational overhead |
| Per-tenant migrations possible | Connection pooling harder (one pool per schema) |
| Easy to export/drop a tenant's data | PostgreSQL may struggle with 10K+ schemas |

**Model 3: Separate Database per Tenant**

```
Database: tenant_123_db → Cluster A
Database: tenant_456_db → Cluster A
Database: tenant_789_db → Cluster B (large tenant, dedicated)
```

| Pros | Cons |
|------|------|
| Strongest isolation | Expensive (small tenants don't justify a whole DB) |
| Per-tenant performance tuning | Operational nightmare at scale (10K databases) |
| Easy data residency compliance | Cross-tenant queries impossible |

**Recommended hybrid approach (staff-level answer):**

```
┌──────────────────────────────────────────────────────────────┐
│  Tier       │  Isolation Model   │  Why                      │
├─────────────┼────────────────────┼───────────────────────────┤
│  Free/Small │  Shared schema     │  Cost efficient; RLS for  │
│  (90%)      │  (tenant_id col)   │  isolation                │
├─────────────┼────────────────────┼───────────────────────────┤
│  Medium     │  Shared DB,        │  Better isolation with    │
│  (9%)       │  separate schema   │  manageable overhead      │
├─────────────┼────────────────────┼───────────────────────────┤
│  Enterprise │  Dedicated DB      │  Maximum isolation; meets │
│  (1%)       │  (own cluster)     │  compliance requirements  │
└─────────────┴────────────────────┴───────────────────────────┘
```

A **tenant routing service** maps each tenant_id to its storage location:

```json
{
  "tenant_123": { "model": "shared", "cluster": "shared-us-east-1", "schema": "public" },
  "tenant_789": { "model": "dedicated", "cluster": "ent-tenant-789-eu-west-1" }
}
```

This mapping is cached in every application instance and refreshed on tenant provisioning events.

##### Deep Dive B: Noisy Neighbor Protection

The biggest operational challenge in multi-tenancy.

**Layer 1: API Gateway rate limiting (per tenant)**

```yaml
rate_limits:
  free:
    requests_per_minute: 60
    burst: 10
  pro:
    requests_per_minute: 600
    burst: 100
  enterprise:
    requests_per_minute: 6000
    burst: 1000
```

**Layer 2: Query-level resource limits**

```sql
-- PostgreSQL per-tenant statement timeout
SET statement_timeout = '5s';  -- free tier
SET statement_timeout = '30s'; -- enterprise

-- Connection pool limits per tenant
-- Free: max 5 connections
-- Pro: max 20 connections
-- Enterprise: dedicated pool
```

**Layer 3: Compute isolation**

| Strategy | Description |
|----------|-------------|
| **Per-tenant thread pools** | Each tenant gets a bounded thread pool. One tenant's slow requests don't starve others. |
| **Fair-share scheduling** | Weighted round-robin across tenants when resources are contested. |
| **Dedicated worker pools** | Enterprise tenants route to dedicated pods/instances. |
| **Circuit breaker per tenant** | If a tenant's requests repeatedly time out, temporarily throttle them. |

**Layer 4: Background job isolation**

Tenant-triggered async jobs (exports, imports, bulk operations) run in isolated queues with per-tenant concurrency limits.

##### Deep Dive C: Tenant-Aware Feature Flags

```json
{
  "feature": "advanced_analytics",
  "rules": [
    { "tenant_tier": "enterprise", "enabled": true },
    { "tenant_tier": "pro", "enabled": true },
    { "tenant_tier": "free", "enabled": false },
    { "tenant_ids": ["tenant_beta_1", "tenant_beta_2"], "enabled": true }
  ]
}
```

Feature flags control:
- Which UI features are visible
- Which API endpoints are accessible
- Which backend capabilities are active (e.g., SSO, audit logs, custom RBAC)
- Gradual rollouts of new features (enable for 10% of tenants, then 50%, then 100%)

##### Deep Dive D: Tenant Onboarding and Offboarding

**Onboarding (< 30 seconds for free tier, minutes for enterprise):**

```
1. Create tenant record in tenant registry
2. Provision resources based on tier:
   - Free: assign to shared pool, create tenant_id row
   - Enterprise: spin up dedicated database, configure networking
3. Create admin user account
4. Apply default configuration (branding, limits, feature flags)
5. Send welcome notification
```

**Offboarding (tenant deletion):**

```
1. Soft-delete: mark tenant as 'disabled' → all API calls return 403
2. Grace period: 30 days (in case of accidental deletion)
3. Data export: provide downloadable archive to tenant admin
4. Hard-delete after grace period:
   - Shared schema: DELETE FROM ... WHERE tenant_id = X (cascade)
   - Separate schema: DROP SCHEMA tenant_X CASCADE
   - Dedicated DB: decommission cluster
5. Purge from all caches, queues, and search indexes
6. Audit log entry for compliance
```

---

#### Step 5 — Scaling and Operations

| Concern | Solution |
|---------|----------|
| **Tenant routing** | Centralized tenant registry (cached locally). On each request, middleware resolves tenant_id → storage location. |
| **Database scaling** | Shared pool: vertical scaling + read replicas. When a shared pool gets too large, split tenants across multiple pools (shard by tenant_id range). |
| **Tenant migration** | Move a growing tenant from shared to dedicated. Use logical replication or dual-write to migrate without downtime. |
| **Monitoring per tenant** | Tag all metrics, logs, and traces with tenant_id. Dashboards for per-tenant health, usage, and cost. |
| **Cost attribution** | Track resource usage per tenant (API calls, storage, compute time). Feed into billing and capacity planning. |
| **Compliance** | Tenant data export (GDPR), data residency routing, audit logs per tenant, encryption at rest with per-tenant keys (enterprise). |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How do you handle tenant-specific customizations?" | Config-driven (not code forks). Per-tenant settings in a JSON config store. For deep customizations (Enterprise), use a plugin/extension system. |
| "How do you handle cross-tenant data sharing?" | Explicit sharing model: Tenant A grants Tenant B read access to specific resources. Implemented via access control policies, not by weakening isolation. |
| "How do you bill tenants?" | Metered billing: track API calls, storage, seats via a usage tracking service. Aggregate daily. Integrate with Stripe Billing for invoicing. |
| "What if a single large tenant outgrows the platform?" | Migrate to a dedicated cluster (single-tenant deployment with the same codebase). Use the same API, different infrastructure. |

---

### Solution 18: Design a Real-Time Collaborative Editor (Google Docs)

> **Difficulty:** Staff Level
> **Time allocation:** 45-60 minutes
> **Real-world references:** Google Docs, Notion, Figma, VS Code Live Share, Etherpad

---

#### Opening Statement

> "A collaborative editor is one of the hardest distributed systems problems because multiple users are simultaneously editing the same document and every user must see a consistent view in real time. The core algorithm challenge — conflict resolution for concurrent edits — is the heart of this design. Let me clarify the scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Real-time co-editing** | Multiple users edit the same document simultaneously with < 200ms visible latency |
| FR2 | **Conflict resolution** | Concurrent edits are merged automatically without data loss |
| FR3 | **Cursor presence** | See other users' cursors and selections in real time |
| FR4 | **Version history** | Full revision history with the ability to view/restore any past version |
| FR5 | **Offline editing** | Users can edit offline; changes sync when reconnected |
| FR6 | **Rich text** | Support formatting (bold, italic, headers, lists, tables, images) |
| FR7 | **Permissions** | Owner, editor, commenter, viewer roles per document |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Latency** | < 100ms for local edit to appear on collaborator's screen (same region) |
| NFR2 | **Consistency** | All users converge to the same document state (eventual consistency with strong convergence) |
| NFR3 | **Scale** | 100M documents; up to 100 concurrent editors per document |
| NFR4 | **Availability** | 99.9%; offline mode for resilience |
| NFR5 | **Durability** | Zero document loss |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Total documents | 100 million |
| Documents with active editors right now | 500,000 |
| Edits per second (global) | ~2 million (avg 4 edits/sec per active doc) |
| Average document size | 50 KB |
| Total storage | 100M * 50 KB = ~5 TB (documents) + version history |
| WebSocket connections (concurrent) | ~2 million users online |

---

#### Step 3 — High-Level Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│  Client (Browser)                                                │
│  ┌─────────────────────────────────────────────────────────┐     │
│  │  Local Document Model (in-memory)                       │     │
│  │  + Local Operation Buffer (unacknowledged ops)          │     │
│  │  + CRDT / OT Engine (merge remote ops)                  │     │
│  └──────────────────────────┬──────────────────────────────┘     │
└─────────────────────────────┼────────────────────────────────────┘
                              │  WebSocket (bidirectional)
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  Collaboration Service (Stateful — one session per document)     │
│                                                                  │
│  ┌──────────────────┐  ┌─────────────────────────────────────┐   │
│  │ WebSocket Server │  │ Document Session Manager            │   │
│  │ (handles          │  │ - Maintains in-memory document state│   │
│  │  connections)    │  │ - Applies + broadcasts operations   │   │
│  └──────────────────┘  │ - Manages cursor presence           │   │
│                        └──────────────────┬──────────────────┘   │
└───────────────────────────────────────────┼──────────────────────┘
                                            │
                         ┌──────────────────┼──────────────────┐
                         │                  │                  │
                         ▼                  ▼                  ▼
              ┌──────────────────┐  ┌──────────────┐  ┌──────────────────┐
              │ Document Store   │  │ Operations   │  │ Presence Service │
              │ (S3 + Postgres)  │  │ Log          │  │ (Redis Pub/Sub)  │
              │                  │  │ (append-only)│  │                  │
              │ - Latest snapshot│  │ - All ops    │  │ - Who is online  │
              │ - Version history│  │   since last │  │ - Cursor positions│
              │                  │  │   snapshot   │  │                  │
              └──────────────────┘  └──────────────┘  └──────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Conflict Resolution — OT vs CRDT

This is **the** core algorithmic challenge.

**The problem:** User A and User B both edit position 5 at the same time. User A inserts "X" and User B deletes the character at position 5. How do we merge these into a consistent document?

**Approach 1: Operational Transformation (OT) — used by Google Docs**

```
Original document: "ABCDE"

User A (at position 2): Insert "X"  → operation: Insert(2, "X")
User B (at position 4): Delete(4)   → operation: Delete(4)

Without transformation:
  A applies own op: "ABXCDE"
  A applies B's op: Delete(4) → "ABXCE"  (deleted 'D')

  B applies own op: "ABCD"
  B applies A's op: Insert(2, "X") → "ABXCD"

  DIVERGED! A has "ABXCE", B has "ABXCD"

With OT — transform B's op against A's op:
  A inserted at position 2 (before B's position 4)
  → B's Delete(4) becomes Delete(5) (shifted by 1)
  
  A: "ABXCDE" → Delete(5) → "ABXCE"  ✓
  B: "ABCD"   → Insert(2,"X") → "ABXCD" → wait...

Actually, both sides must transform each other's ops:
  Server acts as the single source of truth for operation ordering.
```

**OT with a central server:**

```
1. Client generates operation locally (instant feedback)
2. Sends operation to server
3. Server transforms the operation against any concurrent ops it has received
4. Server broadcasts the transformed operation to all other clients
5. Clients transform and apply the incoming operation against their pending local ops
```

| Pros | Cons |
|------|------|
| Well-proven (Google Docs uses this since 2010) | Complex transformation functions (one per operation type pair) |
| Central server simplifies correctness | Server is a single point of ordering — harder to decentralize |
| Works well for text and rich text | Difficult to implement correctly for complex data types |

**Approach 2: CRDTs (Conflict-free Replicated Data Types) — used by Figma, Yjs**

Instead of transforming operations, use a data structure that **merges automatically** without conflicts.

**Text CRDT (e.g., YATA algorithm in Yjs):**

```
Each character has a unique, immutable ID: (clientID, clock)

"HELLO" is stored as:
  (A,0)='H'  (A,1)='E'  (A,2)='L'  (A,3)='L'  (A,4)='O'

User A inserts 'X' between 'E' and 'L':
  New char: (A,5)='X', parent=(A,1), right=(A,2)

User B inserts 'Y' between 'E' and 'L':
  New char: (B,0)='Y', parent=(A,1), right=(A,2)

Both insertions are at the same position. The CRDT uses a deterministic
tie-breaking rule (e.g., higher clientID wins left position):
  Result: "HE" + Y + X + "LLO" = "HEYXLLO" (or "HEXYLLO" depending on rule)
  Both clients arrive at the same result WITHOUT a central server.
```

| Pros | Cons |
|------|------|
| Works peer-to-peer (no central server needed) | Higher memory overhead (metadata per character) |
| Offline-first by design | Tombstone accumulation (deleted chars are marked, not removed) |
| Mathematically guaranteed convergence | Complex to implement from scratch |
| Libraries exist (Yjs, Automerge) | Performance tuning needed for large documents |

**Recommendation:** Use a **CRDT library (Yjs)** with a **central relay server** for simplicity. The server doesn't need to understand the CRDT — it just relays and persists operations. This gets the best of both: decentralized correctness with centralized simplicity.

##### Deep Dive B: Real-Time Communication Layer

```
Client connects to Collaboration Service via WebSocket:

1. Client opens document → WebSocket handshake
2. Server loads document state (from snapshot + pending ops)
3. Server sends current state + version vector to client
4. Client syncs local state

Ongoing editing:
  Client → Server: { type: "op", doc_id: "d_123", ops: [...], version: 42 }
  Server → All other clients: { type: "op", doc_id: "d_123", ops: [...transformed...], version: 43 }

Presence:
  Client → Server: { type: "cursor", doc_id: "d_123", position: 156, user: "Alice" }
  Server → All other clients: (broadcast cursor position)
```

**Scaling WebSocket connections:**

| Challenge | Solution |
|-----------|----------|
| Single server can't hold all connections | Shard by document: all editors of doc X connect to the same server (via consistent hashing or a routing table) |
| Server crash loses document session | WAL of operations. New server replays from last snapshot + ops log. |
| Cross-server communication (rare: admin broadcasts) | Redis Pub/Sub for signaling between servers |
| Sticky sessions | API Gateway routes WebSocket connections by doc_id to the assigned server |

##### Deep Dive C: Persistence and Version History

**Snapshot strategy:**

```
Timeline:
  Op1 → Op2 → Op3 → [Snapshot v1] → Op4 → Op5 → [Snapshot v2] → Op6 → ...

Snapshot every N operations (e.g., every 1000 ops) or every T minutes (e.g., every 5 min).

To load a document:
  1. Load the latest snapshot
  2. Replay all operations since that snapshot
  3. Result: current document state
```

**Storage:**

```
Documents Table (PostgreSQL):
  id, title, owner_id, current_version, created_at, updated_at

Snapshots (S3):
  s3://docs/{doc_id}/snapshots/{version}.json
  (compressed full document state)

Operations Log (PostgreSQL or Cassandra):
  doc_id, version (sequential), operation_data, user_id, timestamp

Version History (for user-facing "see revision history"):
  Periodically (every 30 min or on significant changes) save a named version.
  Users can browse, compare, and restore.
```

##### Deep Dive D: Offline Editing

```
1. Client goes offline
2. User continues editing → operations stored in local buffer (IndexedDB)
3. Client reconnects
4. Client sends buffered operations to server with its last known version
5. Server merges using CRDT (automatic conflict resolution)
6. Server sends any operations the client missed
7. Client is now in sync
```

CRDTs make offline editing natural — they're designed for exactly this scenario. OT-based systems struggle more with offline because the central server must order all operations.

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **Hot documents** (many editors) | A single server handles one document session. For documents with 100+ editors, the server must be beefy. Limit concurrent editors (Google Docs caps at ~100). |
| **Server failure** | Operations are persisted to a durable log. New server replays from last snapshot. In-flight ops may be re-sent by clients (idempotent via version/clock). |
| **Global latency** | Deploy collaboration servers in multiple regions. For same-region editors: < 50ms. Cross-region: relay through a primary server or use CRDT (which tolerates latency natively). |
| **Large documents** | Chunk documents into blocks/pages. Only load and sync the visible portion. Lazy-load the rest. |
| **Storage growth** | Periodically compact operations into snapshots. Archive old versions to cold storage. Tombstone collection for CRDTs. |
| **Permissions** | Enforced at the server: reject operations from users without edit permission. Read-only users receive ops but can't send them. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How would you add commenting?" | Comments are anchored to a text range (identified by CRDT character IDs, not positions). As text is edited around a comment, the anchor moves with it. Comments stored separately, linked by anchor IDs. |
| "How would you add real-time formatting?" | Formatting is represented as operations: `format(range, bold=true)`. The CRDT/OT handles concurrent formatting changes the same way as text changes. |
| "How does Google Docs handle this at scale?" | OT with a central server (Operational Transformation). Each document has one "server" (can be a microservice instance). Jupiter OT algorithm. Server is the single source of truth for operation ordering. |
| "How would you support tables or complex objects?" | Model the document as a tree of blocks (paragraphs, tables, images). Each block is a CRDT. Table cells are nested documents. Operations are scoped to a block. |

---

### Solution 19: Design a Distributed Cache (Redis Cluster)

> **Difficulty:** Staff Level
> **Time allocation:** 45-60 minutes
> **Real-world references:** Redis Cluster, Memcached, Amazon ElastiCache, Hazelcast, Couchbase

---

#### Opening Statement

> "A distributed cache serves as the performance backbone for virtually every high-scale system. The key design challenges are partitioning data across nodes, maintaining consistency during node failures, handling hot keys, and achieving sub-millisecond latency. Let me clarify what we're designing."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **GET / SET / DEL** | Basic key-value operations with sub-millisecond latency |
| FR2 | **TTL** | Keys expire after a configurable time-to-live |
| FR3 | **Rich data structures** | Support strings, hashes, lists, sets, sorted sets |
| FR4 | **Atomic operations** | INCR, CAS (compare-and-swap), Lua scripts for transactional logic |
| FR5 | **Pub/Sub** | Publish/subscribe messaging for real-time events |
| FR6 | **Cluster mode** | Data partitioned across multiple nodes transparently |
| FR7 | **Persistence** | Optional durability (RDB snapshots, AOF append-only file) |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Latency** | < 1ms for GET/SET (p99) within same datacenter |
| NFR2 | **Throughput** | 100K+ operations per second per node |
| NFR3 | **Availability** | Survive single node failure with no data loss and no downtime |
| NFR4 | **Scalability** | Scale to 100+ nodes, petabytes of data |
| NFR5 | **Memory efficiency** | Maximize useful data per GB of RAM |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Total data size | 10 TB (in-memory) |
| Nodes (128 GB usable RAM each, 60% fill) | ~130 data nodes |
| Replication factor | 2 (1 primary + 1 replica) → ~260 nodes total |
| Operations per second (total) | 10 million |
| Average key size | 50 bytes |
| Average value size | 500 bytes |
| Total keys | ~18 billion |

---

#### Step 3 — High-Level Architecture

```
┌──────────────────┐     ┌──────────────────┐
│  Application     │     │  Application     │
│  Server 1        │     │  Server 2        │
│  ┌────────────┐  │     │  ┌────────────┐  │
│  │ Cache Client│  │     │  │ Cache Client│  │
│  │ (smart)    │  │     │  │ (smart)    │  │
│  └─────┬──────┘  │     │  └─────┬──────┘  │
└────────┼─────────┘     └────────┼─────────┘
         │                        │
         │  Client knows the hash slot mapping;
         │  sends request directly to the correct node.
         │                        │
         ▼                        ▼
┌────────────────────────────────────────────────────────────────┐
│                     Cache Cluster                              │
│                                                                │
│  Hash Slots: 0 ─────────────────────────────────── 16383      │
│                                                                │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐ │
│  │  Node A          │  │  Node B          │  │  Node C      │ │
│  │  Slots: 0-5460   │  │  Slots: 5461-10922│ │  Slots:      │ │
│  │  (Primary)       │  │  (Primary)       │  │  10923-16383 │ │
│  │                  │  │                  │  │  (Primary)   │ │
│  │  Replica: Node D │  │  Replica: Node E │  │  Replica: F  │ │
│  └──────────────────┘  └──────────────────┘  └──────────────┘ │
│                                                                │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐ │
│  │  Node D          │  │  Node E          │  │  Node F      │ │
│  │  Replica of A    │  │  Replica of B    │  │  Replica of C│ │
│  └──────────────────┘  └──────────────────┘  └──────────────┘ │
│                                                                │
│  Gossip Protocol: nodes exchange health + slot mapping info    │
└────────────────────────────────────────────────────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Hash Slot Partitioning

Unlike generic consistent hashing, Redis Cluster uses **fixed hash slots** (16,384 slots):

```
slot = CRC16(key) % 16384

Key "user:123"   → CRC16 = 7894  → slot 7894 → Node B
Key "session:abc"→ CRC16 = 2301  → slot 2301 → Node A
```

**Why 16,384 slots instead of consistent hashing?**

| Aspect | Hash Slots | Consistent Hashing |
|--------|------------|-------------------|
| Slot reassignment | Move specific slots between nodes | Virtual nodes make this less predictable |
| Client routing | Client caches slot→node mapping (small table: 16K entries) | Client must know the full hash ring |
| Resharding | Move slots one at a time with live migration | Rebalancing can be complex |
| Simplicity | Easy to reason about and debug | More abstract |

**Hash tags for co-location:**

```
Keys "user:{123}:profile" and "user:{123}:settings"
both hash on "{123}" → same slot → same node

This enables multi-key operations (MGET, Lua scripts, transactions)
on related keys without cross-node coordination.
```

##### Deep Dive B: Replication and Failover

```
Normal operation:
  Client → Node A (primary, slots 0-5460)
  Node A replicates to Node D (asynchronous)

Node A fails:
  1. Node D detects A is unreachable (missed heartbeats for N seconds)
  2. Node D starts an election:
     - Requests votes from other primary nodes
     - Needs majority of primaries to agree
  3. Node D becomes the new primary for slots 0-5460
  4. Clients receive MOVED redirect → update slot mapping
  5. Failover complete in seconds

Node A recovers:
  - Joins as a replica of Node D
  - Syncs missed data
```

**The async replication trade-off:**

```
Client writes to Primary A → A responds "OK" → A replicates to D (async)

If A crashes after responding but BEFORE replicating:
  → That write is LOST.

Mitigation options:
  1. WAIT command: client blocks until N replicas acknowledge (sync replication)
     → Trades latency for durability
  2. Accept the small data loss window (usually < 1 second of data)
     → Appropriate for cache (data can be re-fetched from the source)
  3. For critical data, don't use cache as the source of truth
```

##### Deep Dive C: Hot Key Problem

A single extremely popular key (celebrity tweet, flash sale item) can overwhelm one node.

```
"product:iphone15:stock" → slot 8842 → Node B
Black Friday: 1M reads/sec on this single key → Node B is crushed
```

**Solutions:**

| Strategy | How | Trade-off |
|----------|-----|-----------|
| **Client-side local cache** | Cache hot keys in application memory (LRU, TTL = 1-5 seconds) | Slightly stale data; simple and very effective |
| **Read from replicas** | Route reads to replicas (READONLY mode) | Spreads load; eventually consistent |
| **Key replication / sharding** | Create N copies: `product:iphone15:stock:{0..9}`. Write to all, read from random. | Write amplification; application complexity |
| **Proxy-based caching** | A caching proxy (like Twemproxy or Envoy) in front of the cluster with local caching | Extra hop; good for read-heavy |

**Recommendation:** Client-side local caching with short TTL is the simplest and most effective. For extreme cases, combine with read-from-replica.

##### Deep Dive D: Eviction Policies

When memory is full, the cache must decide what to evict.

| Policy | Algorithm | Best For |
|--------|-----------|----------|
| **noeviction** | Return error on write when full | When every cached item is critical |
| **allkeys-lru** | Evict least recently used key | General purpose (most common) |
| **allkeys-lfu** | Evict least frequently used key | When access frequency is more predictive than recency |
| **volatile-lru** | LRU only among keys with TTL set | Mix of permanent and temporary keys |
| **volatile-ttl** | Evict keys closest to expiration | When short-TTL items are least valuable |
| **allkeys-random** | Random eviction | When all keys are equally valuable |

**LRU approximation (Redis approach):**

True LRU requires tracking access order for every key (expensive). Redis uses **sampled LRU**: pick N random keys (default 5), evict the one with the oldest access time. Surprisingly effective — with N=10, it closely approximates true LRU.

##### Deep Dive E: Persistence Options

| Mode | How | Recovery Time | Data Loss Risk |
|------|-----|---------------|----------------|
| **RDB (snapshot)** | Periodic full dump to disk (fork + serialize) | Fast (load single file) | Up to the snapshot interval (minutes) |
| **AOF (append-only file)** | Log every write command to disk | Slower (replay all commands) | ~1 second (with fsync every second) |
| **RDB + AOF** | Both | Moderate | Minimal — AOF for completeness, RDB for fast restart |
| **None** | Pure in-memory | Instant (but cold cache) | All data lost on restart |

**Fork overhead for RDB snapshots:**

```
Redis forks the process to create a snapshot. The forked child writes the snapshot
while the parent continues serving requests.

Problem: fork() copies page tables → can cause a latency spike (10-100ms)
         Copy-on-write means memory usage can temporarily double

Mitigation: Schedule snapshots during low traffic. Use replicas for persistence
            (replica does the snapshot, primary stays lean).
```

---

#### Step 5 — Scaling and Operations

| Concern | Solution |
|---------|----------|
| **Resharding** | Move hash slots between nodes with live migration. Redis pauses briefly on the last key batch per slot. Plan reshards during low traffic. |
| **Adding nodes** | New node joins empty. Assign slots from existing nodes. Data migrates live. |
| **Memory fragmentation** | Redis uses jemalloc. Monitor `mem_fragmentation_ratio`. If > 1.5, consider restarting the node (replica takes over). |
| **Thundering herd** | When a popular cache key expires, hundreds of requests hit the DB simultaneously. Solution: lock-based refresh (one request rebuilds, others wait) or probabilistic early expiration. |
| **Cache stampede prevention** | Use `SETNX` to acquire a lock. Winner refreshes the cache. Losers wait or serve stale data briefly. |
| **Monitoring** | Track: hit rate, memory usage, eviction rate, connected clients, replication lag, slow log, command latency histograms. |
| **Cache warming** | On deployment or node replacement, pre-populate frequently accessed keys from the database to avoid cold cache performance dip. |

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How do you choose between Redis and Memcached?" | Redis: richer data types, persistence, replication, Lua scripting. Memcached: simpler, multi-threaded (better per-node throughput for simple GET/SET), no persistence. Use Redis for most cases; Memcached for simple, extreme-throughput caching. |
| "How do you handle cache invalidation?" | Three patterns: TTL-based (simple, stale window), event-driven (publish invalidation on write), write-through (update cache on every write). TTL + event-driven is the most common combination. |
| "How would you build a cache for a multi-region setup?" | Option A: cache per region, independent (simplest, highest cache miss rate after failover). Option B: replicate cache across regions (complex, uses cross-region replication). Option C: tiered — local L1 cache + regional L2 cache. |
| "How is this different from the Key-Value Store (Solution 4)?" | A cache is in-memory, optimized for speed, and tolerant of data loss (data can be refetched). A KV store is disk-backed, focused on durability, and is the source of truth. |

---

### Solution 20: Design a Search Engine (Google-scale)

> **Difficulty:** Staff Level
> **Time allocation:** 45-60 minutes
> **Real-world references:** Google Search, Bing, Elasticsearch, Apache Solr, Meilisearch

---

#### Opening Statement

> "Designing a web-scale search engine is arguably the most ambitious system design question. It spans web crawling, indexing petabytes of content, ranking billions of documents in milliseconds, and serving results with extreme availability. I'll focus on the core architecture and go deep on the inverted index and ranking. Let me define the scope."

---

#### Step 1 — Requirements Clarification

##### Functional Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| FR1 | **Web crawling** | Continuously discover and fetch web pages |
| FR2 | **Indexing** | Process crawled pages and build a searchable index |
| FR3 | **Search** | Given a text query, return the top-K most relevant documents ranked by relevance |
| FR4 | **Autocomplete** | Suggest queries as the user types |
| FR5 | **Snippet generation** | Show a relevant text excerpt from each result |
| FR6 | **Freshness** | Recently published or updated content should be findable within minutes to hours |
| FR7 | **Spell correction** | "Did you mean..." suggestions for misspelled queries |

##### Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR1 | **Index size** | 100 billion web pages |
| NFR2 | **Query latency** | < 500ms (p99) for first page of results |
| NFR3 | **QPS** | 100,000 search queries per second |
| NFR4 | **Availability** | 99.99% |
| NFR5 | **Freshness** | Breaking news indexed within minutes |
| NFR6 | **Relevance** | High-quality ranking (precision and recall) |

---

#### Step 2 — Estimation

| Metric | Value |
|--------|-------|
| Web pages indexed | 100 billion |
| Average page size (after cleaning) | 50 KB |
| Raw content | 100B * 50 KB = 5 PB |
| Inverted index size | ~20% of content = ~1 PB |
| Index with replication (3x) | ~3 PB |
| Search QPS | 100,000 |
| Crawl rate | ~10,000 pages/sec (to refresh index weekly) |

---

#### Step 3 — High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          OFFLINE PIPELINE                               │
│                                                                         │
│  ┌──────────┐    ┌──────────────┐    ┌──────────────┐    ┌───────────┐ │
│  │  URL     │───▶│  Web Crawler │───▶│  Content     │───▶│  Indexer  │ │
│  │  Frontier│    │  (distributed│    │  Processor   │    │  (build   │ │
│  │          │    │   fetch)     │    │  (parse,     │    │   inverted│ │
│  │          │◀───│              │    │   clean,     │    │   index)  │ │
│  │  (priority    └──────────────┘    │   extract)   │    └─────┬─────┘ │
│  │   queue)  │                       └──────────────┘          │       │
│  └──────────┘                                                  │       │
│                                                                ▼       │
│                                                    ┌──────────────────┐│
│                                        ┌──────────▶│  Index Segments  ││
│                                        │           │  (distributed    ││
│  ┌──────────────┐    ┌────────────┐    │           │   sharded files) ││
│  │  PageRank /  │───▶│  Link      │────┘           └──────────────────┘│
│  │  Ranking     │    │  Graph DB  │                                    │
│  │  Computation │    │            │                                    │
│  └──────────────┘    └────────────┘                                    │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│                          ONLINE SERVING                                 │
│                                                                         │
│  ┌──────────┐    ┌──────────────┐    ┌──────────────────────────────┐   │
│  │  User    │───▶│  Query       │───▶│  Search Frontend             │   │
│  │  (browser│    │  Service     │    │  - Parse query               │   │
│  │   / app) │◀───│  (API GW)   │◀───│  - Spell correct             │   │
│  └──────────┘    └──────────────┘    │  - Fan out to index shards   │   │
│                                      │  - Merge + rank results      │   │
│                                      │  - Generate snippets         │   │
│                                      └──────────┬───────────────────┘   │
│                                                  │                      │
│                               ┌──────────────────┼────────────────┐     │
│                               │                  │                │     │
│                               ▼                  ▼                ▼     │
│                        ┌────────────┐     ┌────────────┐  ┌──────────┐ │
│                        │Index Shard │     │Index Shard │  │Index     │ │
│                        │    0       │     │    1       │  │Shard N   │ │
│                        │(replica A) │     │(replica A) │  │(repl. A) │ │
│                        │(replica B) │     │(replica B) │  │(repl. B) │ │
│                        └────────────┘     └────────────┘  └──────────┘ │
└─────────────────────────────────────────────────────────────────────────┘
```

---

#### Step 4 — Deep Dives

##### Deep Dive A: Web Crawler

**URL Frontier (priority queue of URLs to crawl):**

```
┌──────────────────────────────────────────────┐
│  URL Frontier                                │
│                                              │
│  Priority Queue:                             │
│  ┌───────────────────────────────────┐       │
│  │ Priority 1: News sites (crawl    │       │
│  │             every 5 min)         │       │
│  ├───────────────────────────────────┤       │
│  │ Priority 2: Popular sites (crawl │       │
│  │             every hour)          │       │
│  ├───────────────────────────────────┤       │
│  │ Priority 3: Long-tail pages      │       │
│  │             (crawl weekly)       │       │
│  └───────────────────────────────────┘       │
│                                              │
│  Politeness Queue (per domain):              │
│  ┌──────────────────────────────────────┐    │
│  │ example.com: min 1s between requests │    │
│  │ news.com:    min 0.5s               │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

**Crawler design:**

```
1. Dequeue URL from frontier
2. Check robots.txt (cached per domain)
3. Fetch page (HTTP GET with timeout)
4. Detect duplicates:
   a. URL dedup (have we crawled this URL recently?)
   b. Content dedup (SimHash / MinHash fingerprint — detect near-duplicate content)
5. Extract links → add new URLs to frontier
6. Send content to Content Processor pipeline
```

**Distributed crawling at scale:**

- Partition URLs by domain (one crawler instance per domain set) to enforce politeness
- Use a distributed URL dedup store (Bloom filter or Redis set)
- Respect `Crawl-delay` in robots.txt
- Handle traps: infinite calendars, session IDs in URLs, spider traps — use max depth and URL pattern detection

##### Deep Dive B: Inverted Index

The inverted index is the core data structure that makes search fast.

**Forward index (what the crawler produces):**

```
doc_1: "the quick brown fox"
doc_2: "the lazy brown dog"
doc_3: "quick fox jumps"
```

**Inverted index (what the search engine queries):**

```
Term          → Posting List (doc_id, term_frequency, positions)
─────────────────────────────────────────────────────────
"brown"       → [(doc_1, tf=1, pos=[2]), (doc_2, tf=1, pos=[2])]
"dog"         → [(doc_2, tf=1, pos=[3])]
"fox"         → [(doc_1, tf=1, pos=[3]), (doc_3, tf=1, pos=[1])]
"jumps"       → [(doc_3, tf=1, pos=[2])]
"lazy"        → [(doc_2, tf=1, pos=[1])]
"quick"       → [(doc_1, tf=1, pos=[1]), (doc_3, tf=1, pos=[0])]
"the"         → [(doc_1, tf=1, pos=[0]), (doc_2, tf=1, pos=[0])]
```

**Query processing:**

```
Query: "quick brown fox"

1. Look up posting lists for each term:
   quick → [doc_1, doc_3]
   brown → [doc_1, doc_2]
   fox   → [doc_1, doc_3]

2. AND query: intersect posting lists
   doc_1 appears in all three → candidate

3. Rank candidates using scoring function (TF-IDF, BM25, etc.)

4. Return top-K results
```

**Posting list compression:**

Posting lists for common terms ("the", "is", "of") contain billions of doc_ids. Compression is essential:

| Technique | How | Compression Ratio |
|-----------|-----|-------------------|
| **Variable-byte encoding (VByte)** | Store delta between consecutive doc_ids using variable-length bytes | 2-4x |
| **PForDelta** | Pack blocks of deltas into fixed-width frames | 4-8x |
| **Roaring Bitmaps** | Hybrid bitmap representation for dense posting lists | 10x+ for dense sets |

**Index sharding:**

```
Strategy 1: Document-based sharding
  Shard 0: documents 0-999,999
  Shard 1: documents 1,000,000-1,999,999
  → Each shard has a complete inverted index for its document subset
  → Query fans out to ALL shards, each returns top-K, then merge

Strategy 2: Term-based sharding
  Shard 0: terms A-F
  Shard 1: terms G-M
  → A single query may only hit a few shards
  → But multi-term queries require cross-shard joins (complex)

Google uses document-based sharding (simpler query path).
```

##### Deep Dive C: Ranking

**Two-phase ranking:**

```
Phase 1: Retrieval + Lightweight Scoring (per shard)
  - BM25 or TF-IDF on the inverted index
  - Return top 1000 candidates per shard
  - Runs in milliseconds using the inverted index

Phase 2: Re-ranking (centralized)
  - ML model (LambdaMART, neural ranker, or transformer-based)
  - Uses 200+ features per document:
    - Text relevance (BM25 score)
    - PageRank (link authority)
    - Freshness (recency of content)
    - User signals (click-through rate, dwell time)
    - Page quality (spam score, readability)
    - Domain authority
  - Re-rank top candidates, return top 10 for page 1
```

**BM25 scoring (the workhorse of text retrieval):**

```
BM25(q, d) = Σ IDF(term) * (tf * (k1 + 1)) / (tf + k1 * (1 - b + b * |d| / avgdl))

Where:
  tf = term frequency in document d
  |d| = document length
  avgdl = average document length in the corpus
  k1 = 1.2 (tuning: term frequency saturation)
  b = 0.75 (tuning: length normalization)
  IDF(term) = log((N - df + 0.5) / (df + 0.5))
  N = total documents, df = documents containing the term
```

**PageRank (link analysis):**

```
Every page has a score based on the link graph:
  PR(A) = (1-d) + d * Σ (PR(T) / L(T))
  
  d = damping factor (0.85)
  T = pages that link to A
  L(T) = number of outbound links from T

Computed offline in a batch MapReduce/Spark job over the link graph.
Pages linked by many high-authority pages get higher PageRank.
```

##### Deep Dive D: Query Processing Pipeline

```
User types: "best coffea shops neer me"
                │
                ▼
┌───────────────────────┐
│ 1. Spell Correction   │  "coffea" → "coffee", "neer" → "near"
│    (Norvig's model /  │
│     neural speller)   │
└───────────┬───────────┘
            │  "best coffee shops near me"
            ▼
┌───────────────────────┐
│ 2. Query Parsing      │  Tokenize, stem, remove stop words
│    + Intent Detection │  Detect: local search intent → use geo ranking
└───────────┬───────────┘
            │  tokens: ["best", "coffee", "shop", "near"]
            ▼
┌───────────────────────┐
│ 3. Query Expansion    │  Add synonyms: "coffee" → "coffee", "cafe"
│                       │  Add personalization: user's location
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│ 4. Fan-out to Index   │  Send query to all shards in parallel
│    Shards             │  Each shard returns top-K (e.g., K=1000)
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│ 5. Merge + Re-rank    │  Merge results from all shards
│                       │  Apply ML ranker on top candidates
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│ 6. Generate Snippets  │  For top 10 results, extract the most relevant
│                       │  text fragment containing query terms
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│ 7. Return Results     │  10 results + snippets + spell suggestion +
│                       │  autocomplete suggestions + ads
└───────────────────────┘
```

---

#### Step 5 — Scaling and Reliability

| Concern | Solution |
|---------|----------|
| **Index serving at scale** | Document-sharded index across thousands of machines. Each shard replicated 3x. Route queries to the least-loaded replica. |
| **Index freshness** | Dual index: a large "base" index (rebuilt weekly) + a small "real-time" index (updated within minutes). Merge results from both. |
| **Query latency** | Cache popular queries (30% of queries are repeats). Terminate early if top results have high-enough scores. Skip low-quality shards. |
| **Crawler politeness** | Per-domain rate limiting. Respect robots.txt. Distributed URL frontier to avoid overloading any single site. |
| **Spam/SEO manipulation** | ML-based spam detection at index time. Penalize link farms, keyword stuffing, cloaking. Manual quality rater feedback loop. |
| **Availability** | Every shard replicated to 3+ nodes in multiple AZs. If a replica fails, traffic shifts instantly. No single point of failure. |
| **Index build** | Offline batch pipeline (MapReduce / Spark). Build new index segments, then swap into serving atomically. |
| **Cost** | Tiered storage: frequently accessed index segments in RAM, less popular segments on SSD, rare segments on HDD. |

---

#### Data Model

```
Document Store (metadata — distributed DB):
  doc_id      | url                          | title              | pagerank | last_crawled
  ------------|------------------------------|--------------------|----------|-------------
  1           | https://example.com/page1    | "Example Page"     | 0.0023   | 2024-01-15
  2           | https://news.com/article     | "Breaking News"    | 0.0156   | 2024-01-15

Inverted Index (per shard, on disk + memory-mapped):
  term → compressed posting list [(doc_id, tf, positions), ...]

Link Graph (for PageRank, stored in graph DB or adjacency list):
  source_doc_id → [target_doc_id_1, target_doc_id_2, ...]

URL Frontier (distributed priority queue — Redis / Kafka):
  (priority, last_crawled, url, domain)
```

---

#### Common Follow-Up Questions

| Question | Key Points |
|----------|------------|
| "How does autocomplete work?" | Trie of popular queries, scored by frequency and recency. As the user types, traverse the trie and return top-K completions. Serve from an in-memory index at the edge for < 50ms latency. |
| "How would you handle personalization?" | Re-rank results based on user history (clicked results, preferred domains, language, location). Add personalization features to the ML ranker. Privacy trade-off. |
| "How do you keep the index fresh for breaking news?" | Dedicated real-time crawl pipeline for high-priority sources (news sites, Twitter). Small "real-time index" updated within minutes. Merged with the base index at query time. |
| "How does snippet generation work?" | For each result, find the text passage with the highest density of query terms. Highlight matching terms. This is done at serving time (not pre-computed, since it depends on the query). |
| "How would you build image/video search?" | Extract visual features (embeddings from a CNN/ViT model). Build a vector index (approximate nearest neighbor — HNSW, IVF). Query: encode the query image → search the vector index. Combine with text metadata for multi-modal search. |

---

---

*Good system design is not about memorizing architectures. It is about developing the judgment to make the right trade-offs for the right context.*
