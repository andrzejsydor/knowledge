---
tags:
  - security
  - Authentication
  - Authorization
  - oauth
---

## Table of Contents
1. [Authentication vs Authorization](#authentication-vs-authorization)
2. [Zero Trust Architecture](#zero-trust-architecture)
3. [OAuth 2.0](#oauth-20)
4. [Tips & Problems](#tips--problems)

# Authentication vs Authorization

## 

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


# #OAuth 2.0

## Roles
- **Resource Owner**: User who authorizes an application to access their account.
- **Client**: Application that wants to access the user's account.
- **Authorization Server**: Validates the user's identity and issues access tokens.
- **Resource Server**: The API or service accepting access tokens.

## Common Grant Types
- **Authorization Code**: For server-side apps; most secure.
- **PKCE**: Extension for mobile/SPA apps to prevent code interception.
- **Client Credentials**: For machine-to-machine communication.

##  Tokens and Headers
- Access Token: Short-lived credential used to call the RS.
- Refresh Token: Longer-lived credential used by the client to obtain new access tokens.
  - Authorization header (Bearer):
    - Authorization: Bearer <access_token>
- Token formats:
  - JWT: Self-contained, signed token with claims; verifiable by RS without a network call.
  - Opaque: Non-self-describing; RS must introspect/validate with the AS.


## Tips & Problems

### log4j & gradle find dependency

[Log4Shell Remediation CheatSheet - Snyk](https://snyk.io/wp-content/uploads/cheat-sheet-log4shell-remediation-v6.pdf)



# Links #Link

[SecureFlag Knowledge Flag](https://knowledge-base.secureflag.com/)

[OWASP Top 10](https://owasp.org/Top10/)

[OWASP API Security Project](https://owasp.org/www-project-api-security/)

[OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)

[PortSwigger](https://portswigger.net/web-security/all-topics)

