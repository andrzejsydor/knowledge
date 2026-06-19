---
tags:
  - Architecture
  - System-Design
  - IDs
  - Sqids
---

# Human-Friendly IDs

---
- [Summary](#summary)
- [What Problem This Solves](#what-problem-this-solves)
- [What Is Sqids](#what-is-sqids)
- [When to Use Sqids](#when-to-use-sqids)
- [When Not to Use Sqids](#when-not-to-use-sqids)
- [Sqids vs Other ID Strategies](#sqids-vs-other-id-strategies)
- [Java Example](#java-example)
- [Links](#links)
---

## Summary

**Sqids** is a library for generating short, URL-safe, human-friendly IDs from numbers. It is useful when you want public-facing identifiers that are cleaner than raw incremental database IDs.

## What Problem This Solves

Exposing plain numeric IDs in URLs can be undesirable because they:

- look less user-friendly
- reveal ordering and approximate scale
- make enumeration easier
- are less suitable for copy/paste in public links

Human-friendly IDs help make public URLs cleaner while keeping internal numeric IDs in the database.

## What Is Sqids

**Sqids** converts one or more numbers into short strings using a deterministic, URL-safe alphabet.

Example use cases:

- public URLs like `/orders/86Rf07`
- invitation codes
- coupon-style identifiers
- obfuscated references to internal numeric IDs

Key characteristics:

- short and readable output
- URL-safe
- deterministic encoding/decoding
- available in multiple languages

## When to Use Sqids

Use **Sqids** when:

- your database uses numeric primary keys
- you want nicer public identifiers
- you want lightweight obfuscation of sequential IDs
- you need stable IDs that can be decoded back to numbers

Typical examples:

| Use case | Why Sqids fits |
|---------|----------------|
| Public resource URLs | Cleaner than raw integers |
| Referral/invite codes | Short and readable |
| Admin/customer facing links | Better UX |
| Internal tools with sharable links | Easier copy/paste |

## When Not to Use Sqids

Do **not** use **Sqids** as:

- a security boundary
- encryption
- a replacement for authorization checks
- a globally unique distributed ID generator

If you need:

| Need | Better option |
|------|---------------|
| Cryptographic secrecy | Real encryption / signed tokens |
| Global uniqueness across systems | UUID / ULID / KSUID |
| Time-sortable distributed IDs | ULID / Snowflake-style IDs |
| Strong anti-enumeration guarantees | Authorization + non-guessable identifiers |

## Sqids vs Other ID Strategies

| Strategy | Pros | Cons | Best for |
|---------|------|------|----------|
| Auto-increment integer | Simple, fast, compact | Exposes sequence, predictable | Internal DB keys |
| Sqids | Short, readable, URL-safe, reversible | Not secure, depends on numeric source IDs | Public-friendly IDs over integers |
| UUID | Globally unique, common standard | Long, not human-friendly | Distributed systems |
| ULID | Unique and sortable | Longer than Sqids | Event/resource identifiers |
| Snowflake | Compact and sortable | More infra/design complexity | High-scale distributed systems |

## Java Example

Example idea in a Java service:

```java
long orderId = 12345L;
// encode orderId using a Sqids Java library
String publicId = "...";
```

Typical pattern:

1. store the numeric ID internally
2. expose the Sqids value publicly
3. decode it at the API boundary
4. still enforce authorization normally

## Links

- [Sqids](https://sqids.org/)
