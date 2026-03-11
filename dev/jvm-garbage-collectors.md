# JVM Garbage Collectors

## Overview

The JVM provides several garbage collector implementations, each with different trade-offs between throughput, latency, and footprint. The choice of GC depends on the application's requirements.

---

## Garbage Collectors by JVM Version

| Garbage Collector | Introduced | Default Since | Status |
|---|---|---|---|
| Serial GC | JDK 1.3 | — | Active (default on single-core / small heaps) |
| Parallel GC (Throughput) | JDK 1.4.2 | JDK 5 – JDK 8 | Active |
| CMS (Concurrent Mark-Sweep) | JDK 1.4.1 | — | Deprecated in JDK 9, removed in JDK 14 |
| G1 (Garbage-First) | JDK 7u4 | JDK 9+ | Active (current default) |
| ZGC | JDK 11 (experimental) | — | Production-ready since JDK 15 |
| Shenandoah | JDK 12 (experimental) | — | Production-ready since JDK 15 |
| Epsilon (No-Op) | JDK 11 | — | Active (special-purpose) |

---

## Detailed Descriptions

### Serial GC (`-XX:+UseSerialGC`)

- **Versions:** JDK 1.3+
- **Algorithm:** Single-threaded, stop-the-world mark-copy (young gen) + mark-sweep-compact (old gen)
- **Best for:** Small applications, single-core machines, containers with limited CPU
- **Pause times:** Unpredictable, proportional to heap size
- **Throughput:** Low

### Parallel GC (`-XX:+UseParallelGC`)

- **Versions:** JDK 1.4.2+
- **Default in:** JDK 5 through JDK 8
- **Algorithm:** Multi-threaded stop-the-world mark-copy (young gen) + mark-sweep-compact (old gen)
- **Best for:** Batch processing, throughput-oriented workloads where pause times are acceptable
- **Pause times:** Still stop-the-world, but faster than Serial due to parallelism
- **Throughput:** High

### CMS — Concurrent Mark-Sweep (`-XX:+UseConcMarkSweepGC`)

- **Versions:** JDK 1.4.1 – JDK 13 (deprecated in JDK 9, removed in JDK 14)
- **Algorithm:** Concurrent mark-sweep for old gen, parallel copy for young gen
- **Best for:** Applications requiring low-latency with moderate heap sizes
- **Pause times:** Short (most work done concurrently)
- **Downsides:** No compaction → heap fragmentation over time, CPU-intensive concurrent phases
- **Note:** Replaced by G1 as the recommended low-latency collector

### G1 — Garbage-First (`-XX:+UseG1GC`)

- **Versions:** JDK 7u4+ (experimental in JDK 6)
- **Default in:** JDK 9+
- **Algorithm:** Region-based, concurrent marking with incremental compaction
- **Best for:** General-purpose workloads, large heaps (4 GB+), applications needing balanced throughput and latency
- **Pause times:** Predictable, configurable via `-XX:MaxGCPauseMillis` (default 200ms)
- **Key improvements over CMS:** Compaction (no fragmentation), predictable pause times, adaptive region sizing

### ZGC (`-XX:+UseZGC`)

- **Versions:** JDK 11 (experimental), production-ready since JDK 15
- **Generational mode:** JDK 21+ (Generational ZGC via `-XX:+UseZGC -XX:+ZGenerational`)
- **JDK 23:** Generational mode became the default (JEP 474); non-generational mode deprecated
- **JDK 24:** Non-generational mode removed; ZGC is now exclusively generational (`-XX:+ZGenerational` flag removed)
- **Algorithm:** Concurrent, region-based, uses colored pointers and load barriers
- **Best for:** Ultra-low-latency applications, very large heaps (multi-terabyte capable, up to 16 TB)
- **Pause times:** Sub-millisecond (typically < 1ms), independent of heap size
- **Throughput:** Slightly lower than G1/Parallel due to barrier overhead

### Shenandoah (`-XX:+UseShenandoahGC`)

- **Versions:** JDK 12+ (experimental), production-ready since JDK 15; backported to JDK 8u and JDK 11u by Red Hat
- **JDK 25:** Generational Shenandoah graduated from experimental to production (JEP 521) — no longer requires `-XX:+UnlockExperimentalVMOptions`; single-generation mode remains the default
- **Algorithm:** Concurrent compacting, uses Brooks forwarding pointers
- **Best for:** Low-latency applications, medium heaps (8–64 GB), similar goals to ZGC
- **Pause times:** Sub-millisecond to low milliseconds (1–10ms range)
- **Note:** Not available in Oracle JDK — available in OpenJDK, Red Hat, Amazon Corretto, Adoptium

### Epsilon — No-Op GC (`-XX:+UnlockExperimentalVMOptions -XX:+UseEpsilonGC`)

- **Versions:** JDK 11+
- **Algorithm:** Allocates memory but never reclaims it
- **Best for:** Performance testing, extremely short-lived applications, GC-free benchmarking
- **Note:** JVM will exit with `OutOfMemoryError` when heap is exhausted

---

## Default GC by JDK Version

| JDK Version | Default Garbage Collector |
|---|---|
| JDK 5 – 8 | Parallel GC |
| JDK 9 – 26+ | G1 GC |

> **Note:** G1 remains the overall JVM default. When explicitly choosing ZGC, generational mode
> became the default in JDK 23 and the only mode in JDK 24+.

---

## Quick Selection Guide

| Priority | Recommended GC |
|---|---|
| Maximum throughput (batch jobs) | Parallel GC |
| Balanced throughput + latency (general purpose) | G1 GC |
| Ultra-low latency (< 1ms pauses) | ZGC or Shenandoah |
| Minimal memory footprint / single-core | Serial GC |
| GC-free benchmarking | Epsilon GC |

---

## JVM Flags Summary

```bash
# Serial
java -XX:+UseSerialGC -jar app.jar

# Parallel (throughput)
java -XX:+UseParallelGC -jar app.jar

# G1 (default since JDK 9)
java -XX:+UseG1GC -jar app.jar

# ZGC (generational by default since JDK 23, exclusively generational since JDK 24)
java -XX:+UseZGC -jar app.jar

# Shenandoah (generational mode production-ready since JDK 25)
java -XX:+UseShenandoahGC -jar app.jar

# Shenandoah generational mode (JDK 25+)
java -XX:+UseShenandoahGC -XX:+ShenandoahGenerational -jar app.jar

# Epsilon (no-op)
java -XX:+UnlockExperimentalVMOptions -XX:+UseEpsilonGC -jar app.jar
```

---

## Recent JDK Release Highlights (GC-related)

### JDK 21 (LTS, September 2023)

- **Generational ZGC** introduced as experimental (`-XX:+UseZGC -XX:+ZGenerational`)
- G1 remains default

### JDK 23 (September 2024)

- **JEP 474:** Generational ZGC became the default ZGC mode; non-generational ZGC deprecated
- Parallel GC Full GC algorithm replaced with improved Mark-Sweep-Compact (fixes quadratic performance issue with many live objects)
- ~230 GC-related changes

### JDK 24 (March 2025)

- Non-generational ZGC removed — ZGC is now **exclusively generational**
- `-XX:+ZGenerational` flag removed (no longer needed)

### JDK 25 (LTS, September 2025)

- **JEP 521:** Generational Shenandoah graduated to production (no longer experimental)
- G1 remembered set merging improved — significant memory reduction (e.g., 2 GB → 0.75 GB peak in benchmarks)
- Better G1 pause time spike avoidance during Space-Reclamation
- GCLocker fix for Parallel/Serial GC preventing premature VM shutdowns with heavy JNI usage
- ~200 GC-related changes

### JDK 26 (March 2026 — current)

- **JEP 516:** Ahead-of-Time Object Caching with any GC — GC-independent AOT cache format supporting all HotSpot collectors (Project Leyden)
- **JEP 522:** G1 throughput improvements via reduced synchronization between application and GC threads
- Improved GC CPU usage accounting — tracks time in pauses and concurrent phases, new logging and programmatic access
- ~380 GC-related changes (nearly double JDK 25)

---

## Further Reading

- [HotSpot Virtual Machine Garbage Collection Tuning Guide (JDK 24)](https://docs.oracle.com/en/java/javase/24/gctuning/)
- [ZGC — Oracle Documentation (JDK 24)](https://docs.oracle.com/en/java/javase/24/gctuning/z-garbage-collector.html)
- [JEP 474: ZGC Generational Mode by Default](https://openjdk.org/jeps/474)
- [JEP 521: Generational Shenandoah](https://openjdk.org/jeps/521)
- [JDK 26 G1/Parallel/Serial GC changes](https://tschatzl.github.io/2026/02/26/jdk26-g1-serial-parallel-gc-changes.html)
- [Shenandoah GC — OpenJDK Wiki](https://wiki.openjdk.org/display/shenandoah)
