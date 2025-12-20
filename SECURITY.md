---
tags:
  - Security
  - Authentication
  - Authorization
  - oauth
  - Session
  - Spring Session
---

## Table of Contents
1. [Authentication vs Authorization](#authentication-vs-authorization)
2. [Zero Trust Architecture](#zero-trust-architecture)
3. [OAuth 2.0](#oauth-20)
4. [Session-Based Authentication](#session-based-authentication)
5. [Tips & Problems](#tips--problems)

# Authentication vs Authorization

| Authentication | Authorization |
| ------- | --- |
| "who are you" | "are you allowed to do that?" |
| Typically established during login| Users authentication |
| Used to determine authorization| set using "allow" rules' there are no "deny" rules |
| Can be external (LDAP) | attached to a user or group |

# Zero Trust Architecture

## Three mutually reinforcing principles:

### Never trust, always verify
• Every single request—by a user, a device, or a workload—must prove its identity and legitimacy before it touches a resource.
• Verification is continuous and context-aware: time of day, geolocation, device health, behavioral patterns, etc.
• Multi-factor authentication (MFA) and mutual TLS (mTLS) for service-to-service calls are typical enablers.

### Least-privilege access
• Grant only the minimum rights needed, for the shortest time needed.
• Prefer role-based or attribute-based access control over direct user permissions, and issue short-lived, scoped credentials.
• Regularly review and remove privileges that are no longer justified.

### Assume breach
• Design as if an attacker is already inside.
• Segment networks and workloads, encrypt data in transit and at rest, and collect detailed audit logs.
• Monitor continuously for abnormal behaviour and be ready to contain or revoke access quickly.


# OAuth 2.0

- [OAuth 2.0 Simplified](https://www.oauth.com/)
- [OAuth 2.0](https://oauth.net/2/) 

## Roles
- **Resource Owner**: User who authorizes an application to access their account.
- **Client**: Application that wants to access the user's account.
- **Authorization Server**: Validates the user's identity and issues access tokens.
- **Resource Server**: The API or service accepting access tokens.

## Common Grant Types
- **Authorization Code**: For server-side apps; most secure.
- **PKCE**: Extension for mobile/SPA apps to prevent code interception.
- **Client Credentials**: For machine-to-machine communication.

## Tokens and Headers
- **Access Token**: Short-lived credential used to call the Resource Server (RS).
- **Refresh Token**: Longer-lived credential used by the client to obtain new access tokens.
- **Authorization Header**:
  - Format: `Authorization: Bearer <access_token>`
- **Token Formats**:
  - **JWT**: Self-contained, signed token with claims; verifiable by RS without a network call.
  - **Opaque**: Non-self-describing; RS must introspect/validate with the Authorization Server (AS).

# Session-Based Authentication

Session-based authentication is a stateful authentication mechanism where the server maintains session state and identifies users through session identifiers (typically stored in cookies).

## How Session Authentication Works

1. **Login**: User provides credentials → Server validates → Creates session → Stores session data → Returns session ID (cookie)
2. **Subsequent Requests**: Client sends session ID (cookie) → Server validates session → Grants access
3. **Logout**: Server invalidates session → Client removes cookie

## Spring Session Framework

Spring Session provides an API and implementations for managing user session information, abstracting away from the servlet container's session management.

### Key Benefits

- **Container Independence**: Works across different servlet containers (Tomcat, Jetty, etc.)
- **Distributed Sessions**: Store sessions in Redis, JDBC, MongoDB, or Hazelcast for clustered deployments
- **RESTful APIs**: Support for session management in REST APIs
- **WebSocket Support**: Maintain session state in WebSocket connections

### Session Storage Options

- **Redis**: Fast, in-memory storage ideal for distributed systems
- **JDBC**: Database-backed storage for persistence and auditability
- **MongoDB**: Document-based storage for flexible session data
- **Hazelcast**: In-memory data grid for high-performance clustering

### Session Lifecycle

1. **Creation**: Session created upon successful authentication
2. **Access**: Session validated on each request
3. **Refresh**: Last accessed time updated
4. **Expiration**: Session invalidated after timeout period
5. **Destruction**: Session removed from storage

### Best Practices

#### Session Timeout

- Set appropriate timeout based on application security requirements
- Typical values: 15-30 minutes for sensitive applications, 1-2 hours for general use
- Consider user activity patterns when setting timeout

#### Secure Cookie Configuration

- Use `HttpOnly` flag to prevent XSS attacks
- Use `Secure` flag for HTTPS-only transmission
- Set `SameSite` attribute to prevent CSRF attacks
- Use custom cookie names to avoid fingerprinting

#### Concurrent Session Control

- Limit number of concurrent sessions per user
- Choose between blocking new logins or invalidating oldest session
- Track active sessions for security monitoring

#### Session Fixation Protection

- Always change session ID after authentication
- Use `changeSessionId()` strategy (default in Spring Security)
- Prevents attackers from hijacking pre-authentication sessions

#### Session Storage Considerations

- **In-Memory**: Fast but lost on restart, not suitable for clustering
- **Redis**: Fast, distributed, requires Redis infrastructure
- **JDBC**: Persistent, auditable, but slower than in-memory
- **MongoDB**: Flexible schema, good for complex session data

### Common Patterns

#### Pattern 1: Distributed Session Clustering

Use external session store (Redis/JDBC) to share sessions across multiple application instances. Essential for horizontal scaling and high availability.

#### Pattern 2: Remember Me Functionality

Extend session lifetime for trusted devices using persistent tokens. Separate from main session timeout for better security.

#### Pattern 3: Session Management API

Provide REST endpoints for session introspection and management. Useful for admin interfaces and security monitoring.

### Security Considerations

- **Session Hijacking**: Mitigate with secure cookies, HTTPS, and session fixation protection
- **Session Fixation**: Always regenerate session ID after authentication
- **Session Timeout**: Balance security (shorter) with user experience (longer)
- **Concurrent Sessions**: Limit to prevent account sharing and detect suspicious activity
- **Session Storage**: Encrypt sensitive session data, especially in distributed stores

For implementation details and code examples, see [Spring Session Implementation](./dev/Spring_Session.md).


## Tips & Problems

### log4j & gradle find dependency

[Log4Shell Remediation CheatSheet - Snyk](https://snyk.io/wp-content/uploads/cheat-sheet-log4shell-remediation-v6.pdf)



# #Links

[SecureFlag Knowledge Flag](https://knowledge-base.secureflag.com/)

[OWASP Top 10](https://owasp.org/Top10/)

[OWASP API Security Project](https://owasp.org/www-project-api-security/)

[OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)

[PortSwigger](https://portswigger.net/web-security/all-topics)

