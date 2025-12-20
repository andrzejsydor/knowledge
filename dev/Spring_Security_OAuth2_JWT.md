---
tags:
  - Spring
  - Spring Security
  - OAuth2
  - JWT
  - Security
---

# Spring Security OAuth2 & JWT Implementation

This document contains code examples and implementation details for Spring Security OAuth2 and JWT. For theory, patterns, and best practices, see [Security - OAuth 2.0 and JWT](../SECURITY.md#oauth-20).

## Dependencies

### Maven

```xml
<!-- Spring Security OAuth2 Resource Server -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
</dependency>

<!-- Spring Security OAuth2 Authorization Server -->
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-oauth2-authorization-server</artifactId>
</dependency>

<!-- JWT Support (included in resource-server) -->
<!-- Additional JWT libraries if needed -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

### Gradle

```gradle
// Spring Security OAuth2 Resource Server
implementation 'org.springframework.boot:spring-boot-starter-oauth2-resource-server'

// Spring Security OAuth2 Authorization Server
implementation 'org.springframework.security:spring-security-oauth2-authorization-server'

// JWT Support
implementation 'org.springframework.boot:spring-boot-starter-web'
```

## Resource Server Configuration

### Basic JWT Resource Server

```java
@Configuration
@EnableWebSecurity
public class ResourceServerConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt
                    .decoder(jwtDecoder())
                )
            );
        return http.build();
    }
    
    @Bean
    public JwtDecoder jwtDecoder() {
        return NimbusJwtDecoder.withJwkSetUri("https://your-auth-server/.well-known/jwks.json")
            .build();
    }
}
```

### JWT Decoder with Issuer URI

```java
@Configuration
public class JwtConfig {
    
    @Value("${spring.security.oauth2.resourceserver.jwt.issuer-uri}")
    private String issuerUri;
    
    @Bean
    public JwtDecoder jwtDecoder() {
        return NimbusJwtDecoder.withIssuerLocation(issuerUri).build();
    }
}
```

### application.properties (Resource Server)

```properties
# OAuth2 Resource Server Configuration
spring.security.oauth2.resourceserver.jwt.issuer-uri=https://your-auth-server
spring.security.oauth2.resourceserver.jwt.jwk-set-uri=https://your-auth-server/.well-known/jwks.json

# Or use public key directly
spring.security.oauth2.resourceserver.jwt.public-key-location=classpath:public-key.pem
```

### application.yml (Resource Server)

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://your-auth-server
          # Alternative: jwk-set-uri
          # Alternative: public-key-location
```

## Authorization Server Configuration

### Basic Authorization Server Setup

```java
@Configuration
@EnableWebSecurity
public class AuthorizationServerConfig {
    
    @Bean
    @Order(1)
    public SecurityFilterChain authorizationServerSecurityFilterChain(HttpSecurity http) throws Exception {
        OAuth2AuthorizationServerConfiguration.applyDefaultSecurity(http);
        return http.build();
    }
    
    @Bean
    public RegisteredClientRepository registeredClientRepository() {
        RegisteredClient registeredClient = RegisteredClient.withId(UUID.randomUUID().toString())
            .clientId("client-id")
            .clientSecret("{noop}client-secret")
            .clientAuthenticationMethod(ClientAuthenticationMethod.CLIENT_SECRET_BASIC)
            .authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
            .authorizationGrantType(AuthorizationGrantType.REFRESH_TOKEN)
            .redirectUri("http://localhost:8080/login/oauth2/code/client-id")
            .scope("read")
            .scope("write")
            .build();
        
        return new InMemoryRegisteredClientRepository(registeredClient);
    }
    
    @Bean
    public JWKSource<SecurityContext> jwkSource() {
        RSAKey rsaKey = generateRsa();
        JWKSet jwkSet = new JWKSet(rsaKey);
        return (jwkSelector, securityContext) -> jwkSelector.select(jwkSet);
    }
    
    private RSAKey generateRsa() {
        KeyPair keyPair;
        try {
            KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
            keyPairGenerator.initialize(2048);
            keyPair = keyPairGenerator.generateKeyPair();
        } catch (Exception ex) {
            throw new IllegalStateException(ex);
        }
        RSAPublicKey publicKey = (RSAPublicKey) keyPair.getPublic();
        RSAPrivateKey privateKey = (RSAPrivateKey) keyPair.getPrivate();
        return new RSAKey.Builder(publicKey)
            .privateKey(privateKey)
            .keyID(UUID.randomUUID().toString())
            .build();
    }
    
    @Bean
    public ProviderSettings providerSettings() {
        return ProviderSettings.builder()
            .issuer("http://localhost:9000")
            .build();
    }
}
```

## JWT Customization

### Custom JWT Claims Mapping

```java
@Configuration
public class JwtConfig {
    
    @Bean
    public JwtDecoder jwtDecoder() {
        NimbusJwtDecoder decoder = NimbusJwtDecoder.withJwkSetUri("https://your-auth-server/.well-known/jwks.json")
            .build();
        
        // Custom claim validation
        decoder.setJwtValidator(jwtValidator());
        
        return decoder;
    }
    
    @Bean
    public JwtValidator jwtValidator() {
        return new DelegatingOAuth2TokenValidator<>(
            new JwtTimestampValidator(),
            new JwtIssuerValidator("https://your-auth-server"),
            new JwtClaimValidator<String>("aud", audience -> audience.contains("your-resource-server"))
        );
    }
}
```

### Extract Claims to Authorities

```java
@Configuration
public class JwtConfig {
    
    @Bean
    public JwtAuthenticationConverter jwtAuthenticationConverter() {
        JwtGrantedAuthoritiesConverter authoritiesConverter = new JwtGrantedAuthoritiesConverter();
        authoritiesConverter.setAuthorityPrefix("ROLE_");
        authoritiesConverter.setAuthoritiesClaimName("authorities");
        
        JwtAuthenticationConverter authenticationConverter = new JwtAuthenticationConverter();
        authenticationConverter.setJwtGrantedAuthoritiesConverter(authoritiesConverter);
        
        return authenticationConverter;
    }
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt
                    .jwtAuthenticationConverter(jwtAuthenticationConverter())
                )
            );
        return http.build();
    }
}
```

### Custom Principal Extraction

```java
@Component
public class CustomJwtAuthenticationConverter implements Converter<Jwt, AbstractAuthenticationToken> {
    
    @Override
    public AbstractAuthenticationToken convert(Jwt jwt) {
        Collection<GrantedAuthority> authorities = extractAuthorities(jwt);
        String username = jwt.getClaimAsString("preferred_username");
        return new JwtAuthenticationToken(jwt, authorities, username);
    }
    
    private Collection<GrantedAuthority> extractAuthorities(Jwt jwt) {
        List<String> roles = jwt.getClaimAsStringList("roles");
        return roles.stream()
            .map(role -> new SimpleGrantedAuthority("ROLE_" + role))
            .collect(Collectors.toList());
    }
}
```

## Working with JWT in Controllers

### Access JWT Claims

```java
@RestController
public class UserController {
    
    @GetMapping("/profile")
    public UserProfile getProfile(@AuthenticationPrincipal Jwt jwt) {
        String username = jwt.getClaimAsString("preferred_username");
        String email = jwt.getClaimAsString("email");
        return userService.getProfile(username);
    }
    
    @GetMapping("/user-info")
    public Map<String, Object> getUserInfo(@AuthenticationPrincipal Jwt jwt) {
        Map<String, Object> userInfo = new HashMap<>();
        userInfo.put("subject", jwt.getSubject());
        userInfo.put("username", jwt.getClaimAsString("preferred_username"));
        userInfo.put("email", jwt.getClaimAsString("email"));
        userInfo.put("roles", jwt.getClaimAsStringList("roles"));
        return userInfo;
    }
}
```

### Access Principal Information

```java
@RestController
public class ResourceController {
    
    @GetMapping("/api/resource")
    public ResponseEntity<?> getResource(@AuthenticationPrincipal Jwt jwt) {
        String username = jwt.getSubject();
        List<String> scopes = jwt.getClaimAsStringList("scope");
        
        // Check if user has required scope
        if (!scopes.contains("read")) {
            return ResponseEntity.status(HttpStatus.FORBIDDEN).build();
        }
        
        return ResponseEntity.ok(resourceService.getResource(username));
    }
}
```

## Method Security

### Enable Method Security

```java
@Configuration
@EnableMethodSecurity
public class MethodSecurityConfig {
    // Method security is now enabled
}
```

### Use Method Security Annotations

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping("/{id}")
    @PreAuthorize("hasAuthority('SCOPE_read')")
    public User getUser(@PathVariable Long id) {
        return userService.getUser(id);
    }
    
    @PostMapping
    @PreAuthorize("hasAuthority('SCOPE_write')")
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }
    
    @DeleteMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
    
    @GetMapping("/me")
    @PreAuthorize("authentication.name == #username")
    public User getMyProfile(@RequestParam String username) {
        return userService.getUserByUsername(username);
    }
}
```

## Token Validation and Error Handling

### Custom Token Validation

```java
@Component
public class CustomJwtValidator implements OAuth2TokenValidator<Jwt> {
    
    @Override
    public OAuth2TokenValidatorResult validate(Jwt jwt) {
        List<String> errors = new ArrayList<>();
        
        // Custom validation logic
        String customClaim = jwt.getClaimAsString("custom_claim");
        if (customClaim == null || customClaim.isEmpty()) {
            errors.add("Missing required custom_claim");
        }
        
        // Check token expiration buffer
        Instant expiration = jwt.getExpiresAt();
        if (expiration != null && expiration.isBefore(Instant.now().plusSeconds(60))) {
            errors.add("Token expires too soon");
        }
        
        if (errors.isEmpty()) {
            return OAuth2TokenValidatorResult.success();
        }
        return OAuth2TokenValidatorResult.failure(errors);
    }
}
```

### Custom Error Handling

```java
@Component
public class JwtAuthenticationEntryPoint implements AuthenticationEntryPoint {
    
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response,
                        AuthenticationException authException) throws IOException {
        response.setStatus(HttpStatus.UNAUTHORIZED.value());
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        
        Map<String, Object> body = new HashMap<>();
        body.put("status", HttpStatus.UNAUTHORIZED.value());
        body.put("error", "Unauthorized");
        body.put("message", authException.getMessage());
        body.put("path", request.getServletPath());
        
        ObjectMapper mapper = new ObjectMapper();
        mapper.writeValue(response.getOutputStream(), body);
    }
}
```

## Testing

### Test with Mock JWT

```java
@SpringBootTest
@AutoConfigureMockMvc
class ResourceControllerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void testGetResourceWithValidJwt() throws Exception {
        String jwt = createTestJwt();
        
        mockMvc.perform(get("/api/resource")
                .header("Authorization", "Bearer " + jwt))
            .andExpect(status().isOk());
    }
    
    private String createTestJwt() {
        // Create test JWT with required claims
        // This is a simplified example - use a JWT library in practice
        return "test.jwt.token";
    }
}
```

### Test Configuration

```java
@TestConfiguration
public class TestSecurityConfig {
    
    @Bean
    @Primary
    public JwtDecoder jwtDecoder() {
        // Return a mock JWT decoder for testing
        return new NimbusJwtDecoder(new MockJwtDecoder());
    }
}
```

## Common Patterns

### Pattern 1: Multiple Resource Servers

```java
@Configuration
public class MultiResourceServerConfig {
    
    @Bean
    @Order(1)
    public SecurityFilterChain apiSecurityFilterChain(HttpSecurity http) throws Exception {
        http
            .securityMatcher("/api/**")
            .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
            .oauth2ResourceServer(oauth2 -> oauth2.jwt());
        return http.build();
    }
    
    @Bean
    @Order(2)
    public SecurityFilterChain webSecurityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth.anyRequest().permitAll());
        return http.build();
    }
}
```

### Pattern 2: Token Introspection (Opaque Tokens)

```java
@Configuration
public class OpaqueTokenConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .oauth2ResourceServer(oauth2 -> oauth2
                .opaqueToken(opaqueToken -> opaqueToken
                    .introspectionUri("https://your-auth-server/oauth2/introspect")
                    .introspectionClientCredentials("client-id", "client-secret")
                )
            );
        return http.build();
    }
}
```

### Pattern 3: Custom Token Extraction

```java
@Component
public class CustomBearerTokenResolver implements BearerTokenResolver {
    
    @Override
    public String resolve(HttpServletRequest request) {
        // Check custom header first
        String token = request.getHeader("X-Access-Token");
        if (token != null) {
            return token;
        }
        
        // Fall back to standard Authorization header
        String authHeader = request.getHeader("Authorization");
        if (authHeader != null && authHeader.startsWith("Bearer ")) {
            return authHeader.substring(7);
        }
        
        return null;
    }
}
```

## Troubleshooting

### Issue: JWT validation fails

**Solution**: 
- Verify issuer URI is correct
- Check JWK Set endpoint is accessible
- Ensure token hasn't expired
- Verify signature algorithm matches

### Issue: Claims not mapped to authorities

**Solution**: 
- Configure `JwtGrantedAuthoritiesConverter`
- Check claim name matches JWT structure
- Verify authority prefix configuration

### Issue: CORS issues with token requests

**Solution**: 
- Configure CORS properly in Spring Security
- Allow credentials in CORS configuration
- Check preflight request handling

## Links

[Spring Security OAuth2 Resource Server](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/index.html)

[Spring Security OAuth2 Authorization Server](https://docs.spring.io/spring-security/reference/servlet/oauth2/authorization-server/index.html)

[JWT with Spring Security](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/jwt.html)

[OAuth 2.0 Specification](https://oauth.net/2/)

[JWT.io](https://jwt.io/) - JWT debugger and information

[Baeldung - Spring Security OAuth2](https://www.baeldung.com/spring-security-oauth2)

