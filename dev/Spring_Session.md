---
tags:
  - Spring
  - Spring Session
  - Security
  - Session
---

# Spring Session Implementation

This document contains code examples and implementation details for Spring Session. For theory, patterns, and best practices, see [Security - Session-Based Authentication](../SECURITY.md#session-based-authentication).

## Dependencies

### Maven

```xml
<!-- Spring Session with Redis -->
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>io.lettuce</groupId>
    <artifactId>lettuce-core</artifactId>
</dependency>

<!-- Or with JDBC -->
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-jdbc</artifactId>
</dependency>
```

### Gradle

```gradle
// Spring Session with Redis
implementation 'org.springframework.session:spring-session-data-redis'
implementation 'io.lettuce:lettuce-core'

// Or with JDBC
implementation 'org.springframework.session:spring-session-jdbc'
```

## Configuration

### Redis Configuration

```java
@Configuration
@EnableRedisHttpSession(maxInactiveIntervalInSeconds = 1800) // 30 minutes
public class SessionConfig {
    
    @Bean
    public LettuceConnectionFactory connectionFactory() {
        return new LettuceConnectionFactory();
    }
}
```

### application.properties (Redis)

```properties
# Redis Session Store
spring.session.store-type=redis
spring.session.redis.flush-mode=on_save
spring.session.redis.namespace=spring:session
spring.session.timeout=1800s

# Redis Connection
spring.redis.host=localhost
spring.redis.port=6379
spring.redis.password=
```

### application.yml (Redis)

```yaml
spring:
  session:
    store-type: redis
    timeout: 1800s
    redis:
      namespace: spring:session
      flush-mode: on_save
  redis:
    host: ${REDIS_HOST:localhost}
    port: ${REDIS_PORT:6379}
    password: ${REDIS_PASSWORD:}
```

### JDBC Configuration

```java
@Configuration
@EnableJdbcHttpSession
public class SessionConfig {
    // Spring Session will auto-configure JDBC session store
}
```

### application.properties (JDBC)

```properties
# JDBC Session Store
spring.session.store-type=jdbc
spring.session.jdbc.initialize-schema=always
spring.session.timeout=1800s

# Database
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=user
spring.datasource.password=pass
```

## Security Configuration

### Basic Security Configuration

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)
                .maximumSessions(1)
                .maxSessionsPreventsLogin(false)
                .sessionRegistry(sessionRegistry())
            )
            .formLogin(form -> form
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard", true)
                .permitAll()
            )
            .logout(logout -> logout
                .logoutUrl("/logout")
                .logoutSuccessUrl("/login?logout")
                .invalidateHttpSession(true)
                .deleteCookies("JSESSIONID")
            );
        return http.build();
    }
    
    @Bean
    public SessionRegistry sessionRegistry() {
        return new SessionRegistryImpl();
    }
}
```

### Concurrent Session Control

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .sessionManagement(session -> session
                .maximumSessions(1) // Only one session per user
                .maxSessionsPreventsLogin(true) // Block new login if max reached
                .expiredUrl("/login?expired")
            );
        return http.build();
    }
}
```

### Session Fixation Protection

```java
http.sessionManagement(session -> session
    .sessionFixation()
    .changeSessionId() // Default: changes session ID after login
    // .migrateSession() // Alternative: creates new session
    // .newSession() // Alternative: creates new session and migrates data
);
```

## Working with Sessions

### Accessing Session in Controller

```java
@RestController
public class UserController {
    
    @GetMapping("/profile")
    public UserProfile getProfile(HttpSession session) {
        String username = (String) session.getAttribute("username");
        // Access session attributes
        return userService.getProfile(username);
    }
    
    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody LoginRequest request, 
                                   HttpSession session) {
        User user = userService.authenticate(request);
        session.setAttribute("username", user.getUsername());
        session.setAttribute("userId", user.getId());
        return ResponseEntity.ok().build();
    }
}
```

### Using Spring Session's SessionRepository

```java
@Service
public class SessionService {
    
    @Autowired
    private SessionRepository sessionRepository;
    
    public void addAttributeToSession(String sessionId, String key, Object value) {
        Session session = sessionRepository.findById(sessionId);
        if (session != null) {
            session.setAttribute(key, value);
            sessionRepository.save(session);
        }
    }
    
    public void invalidateSession(String sessionId) {
        sessionRepository.deleteById(sessionId);
    }
    
    public List<Session> getAllActiveSessions() {
        return sessionRepository.findAll().stream()
            .filter(s -> s.getLastAccessedTime()
                .isAfter(Instant.now().minus(Duration.ofMinutes(30))))
            .collect(Collectors.toList());
    }
}
```

## Advanced Configuration

### Session Timeout Configuration

```java
@Configuration
@EnableRedisHttpSession(
    maxInactiveIntervalInSeconds = 1800, // 30 minutes
    redisNamespace = "spring:session"
)
public class SessionConfig {
    // Configuration
}
```

### Secure Cookie Configuration

```java
@Configuration
public class CookieConfig {
    
    @Bean
    public CookieSerializer cookieSerializer() {
        DefaultCookieSerializer serializer = new DefaultCookieSerializer();
        serializer.setCookieName("SESSIONID");
        serializer.setCookiePath("/");
        serializer.setDomainNamePattern("^.+?\\.(\\w+\\.[a-z]+)$");
        serializer.setUseHttpOnlyCookie(true);
        serializer.setUseSecureCookie(true); // HTTPS only
        serializer.setSameSite("Strict"); // CSRF protection
        return serializer;
    }
}
```

### Custom Session Events

```java
@Component
public class SessionEventListener {
    
    @EventListener
    public void onSessionCreated(SessionCreatedEvent event) {
        Session session = event.getSession();
        log.info("Session created: {}", session.getId());
    }
    
    @EventListener
    public void onSessionDestroyed(SessionDestroyedEvent event) {
        log.info("Session destroyed: {}", event.getSessionId());
    }
    
    @EventListener
    public void onSessionExpired(SessionExpiredEvent event) {
        log.info("Session expired: {}", event.getSessionId());
    }
}
```

## Common Patterns

### Pattern 1: Remember Me with Sessions

```java
http.rememberMe(remember -> remember
    .key("uniqueAndSecret")
    .tokenValiditySeconds(86400) // 24 hours
    .userDetailsService(userDetailsService)
);
```

### Pattern 2: Session Management API

```java
@RestController
@RequestMapping("/api/session")
public class SessionController {
    
    @Autowired
    private SessionRepository sessionRepository;
    
    @GetMapping("/current")
    public Map<String, Object> getCurrentSession(HttpSession session) {
        Map<String, Object> sessionData = new HashMap<>();
        sessionData.put("id", session.getId());
        sessionData.put("creationTime", session.getCreationTime());
        sessionData.put("lastAccessedTime", session.getLastAccessedTime());
        sessionData.put("maxInactiveInterval", session.getMaxInactiveInterval());
        return sessionData;
    }
    
    @PostMapping("/invalidate")
    public ResponseEntity<?> invalidateSession(HttpSession session) {
        session.invalidate();
        return ResponseEntity.ok().build();
    }
}
```

## Troubleshooting

### Issue: Sessions not persisting across restarts

**Solution**: Ensure you're using an external session store (Redis, JDBC) instead of in-memory storage.

### Issue: Session timeout not working

**Solution**: Check `maxInactiveIntervalInSeconds` configuration and ensure session repository is properly configured.

### Issue: Concurrent session control not working

**Solution**: Ensure `SessionRegistry` bean is properly configured and session events are being published.

## Links

[Spring Session Documentation](https://docs.spring.io/spring-session/reference/)

[Spring Session with Redis](https://docs.spring.io/spring-session/reference/guides/redis.html)

[Spring Session with JDBC](https://docs.spring.io/spring-session/reference/guides/jdbc.html)

[Spring Security Session Management](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html)

[Baeldung - Spring Session Guide](https://www.baeldung.com/spring-session)

