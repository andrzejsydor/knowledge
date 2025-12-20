---
tags:
  - Security
  - Authentication
  - Authorization
  - OAuth
  - OAuth2
  - JWT
  - Session
  - Spring Session
  - Spring Security
---

## Table of Contents
1. [Authentication vs Authorization](#authentication-vs-authorization)
2. [Zero Trust Architecture](#zero-trust-architecture)
3. [OAuth 2.0](#oauth-20)
4. [JWT (JSON Web Tokens)](#jwt-json-web-tokens)
5. [OAuth 2.0 with JWT](#oauth-20-with-jwt)
6. [Spring Security OAuth2](#spring-security-oauth2)
7. [Session-Based Authentication](#session-based-authentication)
8. [Tips & Problems](#tips--problems)

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

OAuth 2.0 is an authorization framework that enables applications to obtain limited access to user accounts on an HTTP service. It delegates user authentication to the service that hosts the user account and authorizes third-party applications to access the user account.

## Core Concepts

### Roles

- **Resource Owner**: The user who authorizes an application to access their account. The application's access to the user's account is limited to the "scope" of the authorization granted.
- **Client**: The application requesting access to a protected resource. Can be a web application, mobile app, or any application that needs to access user resources.
- **Authorization Server**: The server that issues access tokens to the client after successfully authenticating the resource owner and obtaining authorization. Validates the user's identity and issues access tokens.
- **Resource Server**: The server hosting the protected resources, capable of accepting and responding to protected resource requests using access tokens. The API or service accepting access tokens.

### Authorization Flow

1. **Client Request**: Client requests authorization from the resource owner
2. **User Authorization**: Resource owner grants authorization
3. **Authorization Grant**: Client receives authorization grant (code, token, etc.)
4. **Token Request**: Client requests access token from authorization server
5. **Token Issuance**: Authorization server issues access token
6. **Resource Access**: Client uses access token to access protected resources

## Grant Types

### Authorization Code

- **Use Case**: Server-side web applications
- **Security**: Most secure grant type
- **Flow**: Client redirects user to authorization server → User authorizes → Server returns authorization code → Client exchanges code for access token
- **Benefits**: Code is short-lived, exchanged server-to-server, not exposed to user agent

### Authorization Code with PKCE (Proof Key for Code Exchange)

- **Use Case**: Mobile apps and Single Page Applications (SPAs)
- **Security**: Prevents authorization code interception attacks
- **Flow**: Similar to Authorization Code, but adds code verifier and challenge
- **Benefits**: Protects against code interception in public clients

### Client Credentials

- **Use Case**: Machine-to-machine communication, service accounts
- **Security**: No user interaction required
- **Flow**: Client authenticates with credentials → Receives access token directly
- **Benefits**: Simple for backend services, no user context needed

### Resource Owner Password Credentials

- **Use Case**: Legacy applications, trusted first-party clients
- **Security**: Less secure, credentials passed directly
- **Flow**: Client sends username/password → Receives access token
- **Note**: Not recommended for third-party applications

### Implicit Grant

- **Use Case**: Legacy SPAs (deprecated)
- **Security**: Access token returned directly in redirect (less secure)
- **Note**: Deprecated in favor of Authorization Code with PKCE

## Tokens

### Access Token

- **Purpose**: Credential used to access protected resources
- **Lifetime**: Short-lived (typically 15 minutes to 1 hour)
- **Scope**: Limited to permissions granted during authorization
- **Storage**: Should be stored securely on client side
- **Transmission**: Sent in `Authorization: Bearer <token>` header

### Refresh Token

- **Purpose**: Used to obtain new access tokens without re-authentication
- **Lifetime**: Longer-lived (days, weeks, or months)
- **Security**: More sensitive, must be stored securely
- **Revocation**: Can be revoked by authorization server
- **Rotation**: Best practice to rotate refresh tokens on each use

### Token Formats

- **JWT (JSON Web Token)**: Self-contained, signed token with claims; verifiable by Resource Server without network call to Authorization Server
- **Opaque Token**: Non-self-describing string; Resource Server must introspect/validate with Authorization Server

## Scopes

Scopes define the specific permissions the client is requesting. Common scopes include:
- `read`: Read-only access
- `write`: Write access
- `admin`: Administrative access
- Custom scopes specific to the application

# JWT (JSON Web Tokens)

JWT is a compact, URL-safe token format used to securely transmit information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

## JWT Structure

A JWT consists of three parts separated by dots (`.`):

1. **Header**: Contains token type and signing algorithm
2. **Payload**: Contains claims (statements about an entity and additional data)
3. **Signature**: Used to verify the token hasn't been altered

## JWT Claims

### Registered Claims

- `iss` (Issuer): Who issued the token
- `sub` (Subject): Who the token refers to (usually user ID)
- `aud` (Audience): Intended recipient of the token
- `exp` (Expiration Time): When the token expires
- `iat` (Issued At): When the token was issued
- `nbf` (Not Before): Token not valid before this time
- `jti` (JWT ID): Unique identifier for the token

### Public Claims

- Defined in IANA JWT Registry or as URI
- Should be collision-resistant

### Private Claims

- Custom claims agreed upon between parties
- Should avoid collisions with registered/public claims

## JWT Signing Algorithms

### Symmetric (HMAC)

- **Algorithm**: HS256, HS384, HS512
- **Key**: Shared secret between issuer and verifier
- **Use Case**: Single-party systems, internal services
- **Security**: Key must be kept secret by all parties

### Asymmetric (RSA/ECDSA)

- **Algorithm**: RS256, RS384, RS512, ES256, ES384, ES512
- **Key**: Public/private key pair
- **Use Case**: Multi-party systems, distributed services
- **Security**: Private key kept secret, public key shared

## JWT Advantages

- **Stateless**: No server-side session storage required
- **Self-contained**: All necessary information in the token
- **Scalable**: Works well in distributed systems
- **Portable**: Can be used across different domains
- **Verifiable**: Signature ensures integrity

## JWT Disadvantages

- **Size**: Larger than session IDs (can impact performance)
- **Revocation**: Difficult to revoke before expiration
- **Security**: If compromised, valid until expiration
- **Storage**: Sensitive data exposure if not encrypted

## JWT Best Practices

### Token Lifetime

- Keep access tokens short-lived (15-60 minutes)
- Use refresh tokens for longer sessions
- Implement token rotation for enhanced security

### Token Storage

- **Web Applications**: Use httpOnly cookies (most secure) or memory storage
- **Mobile Apps**: Use secure storage (Keychain/Keystore)
- **Never**: Store in localStorage (vulnerable to XSS)

### Token Validation

- Always verify signature
- Check expiration time
- Validate issuer and audience
- Verify token hasn't been tampered with

### Security Considerations

- Use HTTPS for token transmission
- Implement token rotation
- Monitor for token theft/compromise
- Use appropriate signing algorithms (RS256 recommended)
- Consider encryption for sensitive claims (JWE)

# OAuth 2.0 with JWT

Combining OAuth 2.0 with JWT provides a powerful, scalable authentication and authorization solution.

## OAuth 2.0 + JWT Flow

1. **Authorization Request**: Client redirects user to authorization server
2. **User Authentication**: User authenticates with authorization server
3. **Authorization Grant**: Server returns authorization code
4. **Token Exchange**: Client exchanges code for JWT access token
5. **Token Validation**: Resource server validates JWT signature and claims
6. **Resource Access**: Client accesses protected resources with JWT

## Benefits of OAuth 2.0 + JWT

- **Stateless Authentication**: No server-side session storage
- **Distributed Systems**: Works across microservices
- **Self-Contained**: Token contains all necessary information
- **Scalability**: No shared session store required
- **Performance**: No database lookup for token validation

## Token Types in OAuth 2.0 + JWT

### Access Token (JWT)

- Contains user identity and permissions
- Short-lived (15-60 minutes)
- Signed by authorization server
- Validated by resource server

### Refresh Token

- Typically opaque (not JWT) for security
- Used to obtain new access tokens
- Longer-lived
- Stored securely on client

### ID Token (OpenID Connect)

- JWT containing user identity information
- Used for authentication (not authorization)
- Part of OpenID Connect specification
- Contains user profile claims

## Common Patterns

### Pattern 1: Resource Server Validates JWT

Resource server validates JWT signature using public key from authorization server. No network call needed for validation.

### Pattern 2: Token Introspection

For opaque tokens or additional validation, resource server calls authorization server's introspection endpoint.

### Pattern 3: Token Refresh

Client uses refresh token to obtain new access token when current token expires, without re-authentication.

# Spring Security OAuth2

Spring Security provides comprehensive support for OAuth 2.0 and JWT through Spring Security OAuth2 Resource Server and Authorization Server.

## Spring Security OAuth2 Components

### Resource Server

- Validates OAuth 2.0 access tokens
- Protects REST APIs
- Supports JWT and opaque tokens
- Integrates with Spring Security filter chain

### Authorization Server

- Issues OAuth 2.0 tokens
- Manages client registrations
- Handles user authentication
- Supports multiple grant types
- Can issue JWT or opaque tokens

## Key Features

### JWT Support

- Automatic JWT validation
- Public key resolution (JWK Set endpoint)
- Claim extraction and mapping
- Custom claim validation

### Token Validation

- Signature verification
- Expiration checking
- Issuer and audience validation
- Custom validators

### Security Integration

- Seamless integration with Spring Security
- Method-level security
- Role-based access control
- Security context population

## Common Use Cases

### Use Case 1: Resource Server Only

Application acts as resource server, validating tokens issued by external authorization server (e.g., Keycloak, Auth0).

### Use Case 2: Authorization Server

Application acts as authorization server, issuing tokens for other applications to use.

### Use Case 3: Combined Server

Application acts as both authorization server and resource server, issuing and validating its own tokens.

## Best Practices

### Resource Server Configuration

- Configure JWT decoder with issuer URI
- Set up proper security filter chain
- Map JWT claims to Spring Security authorities
- Implement proper error handling

### Token Validation

- Always validate token signature
- Check token expiration
- Verify issuer and audience
- Validate custom claims

### Security Configuration

- Use method security for fine-grained control
- Implement proper CORS configuration
- Configure token extraction from headers
- Set up proper exception handling

### Performance Considerations

- Cache public keys (JWK Set)
- Use appropriate token expiration times
- Consider token caching strategies
- Monitor token validation performance

For implementation details and code examples, see [Spring Security OAuth2 & JWT Implementation](./dev/Spring_Security_OAuth2_JWT.md).

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

## OAuth 2.0 & JWT

[OAuth 2.0 Simplified](https://www.oauth.com/)

[OAuth 2.0 Specification](https://oauth.net/2/)

[JWT.io](https://jwt.io/) - JWT debugger and information

[Spring Security OAuth2 Resource Server](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/index.html)

[Spring Security OAuth2 Authorization Server](https://docs.spring.io/spring-security/reference/servlet/oauth2/authorization-server/index.html)

[Spring Security OAuth2 & JWT Implementation](./dev/Spring_Security_OAuth2_JWT.md) - Code examples and implementation details

