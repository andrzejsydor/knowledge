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

*Good system design is not about memorizing architectures. It is about developing the judgment to make the right trade-offs for the right context.*
