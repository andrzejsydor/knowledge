---
tags:
  - API
  - Versioning
  - REST
  - Architecture
---

# API Versioning Strategies

This document outlines common strategies for versioning APIs, discussing their pros and cons.

- [URI Path Versioning](#uri-path-versioning)
- [Query Parameter Versioning](#query-parameter-versioning)
- [Header Versioning](#header-versioning-custom-header)
- [Media Type Versioning](#media-type-versioning-content-negotiation)

---

## URI Path Versioning

- **Description**: The version number is included in the URL path (e.g., `/api/v1/resource`).
- **Pros**: Easy to implement, explicit, cache-friendly.
- **Cons**: URI changes, potentially breaking bookmarks/links if not managed.

## Query Parameter Versioning

- **Description**: The version is passed as a query parameter (e.g., `/api/resource?version=1`).
- **Pros**: Easy to implement, explicit.
- **Cons**: Can be harder to cache (some caches ignore query params), less readable.

## Header Versioning (Custom Header)

- **Description**: A custom header is used to specify the version (e.g., `X-API-Version: 1`).
- **Pros**: URLs stay clean.
- **Cons**: Requires client to send headers, harder to test directly in browser address bar.

## Media Type Versioning (Content Negotiation)

- **Description**: The version is specified in the `Accept` header (e.g., `Accept: application/vnd.myapi.v1+json`).
- **Pros**: Semantically correct (managing representations), clean URLs.
- **Cons**: Most complex to implement and test, checking headers is required.
