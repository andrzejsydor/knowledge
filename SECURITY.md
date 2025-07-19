---
tags:
  - security
---

## Table of Contents
1. Authentication vs Authorization
2. Zero Trust Architecture
3. Tips & Problems

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


## Tips & Problems

### log4j & gradle find dependency

[Log4Shell Remediation CheatSheet - Snyk](https://snyk.io/wp-content/uploads/cheat-sheet-log4shell-remediation-v6.pdf)



# Links #Link

[SecureFlag Knowledge Flag](https://knowledge-base.secureflag.com/)

[OWASP Top 10](https://owasp.org/Top10/)

[OWASP API Security Project](https://owasp.org/www-project-api-security/)

[OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)

[PortSwigger](https://portswigger.net/web-security/all-topics)

