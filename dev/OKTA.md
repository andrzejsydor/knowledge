---
tags:
  - OKTA
  - Spring Security
  - OAuth2
  - OIDC
  - SSO
  - Identity Provider
---

# OKTA Implementation

This document contains code examples and implementation details for integrating OKTA with applications. For theory, patterns, and best practices, see [Security - OKTA](../SECURITY.md#okta).

## Dependencies

### Maven

```xml
<!-- Spring Security OAuth2 Resource Server (for OKTA as IdP) -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
</dependency>

<!-- Spring Security OAuth2 Client (for OAuth2/OIDC login) -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>

<!-- Spring Security (if not already included) -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<!-- Spring Web -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

### Gradle

```gradle
// Spring Security OAuth2 Resource Server
implementation 'org.springframework.boot:spring-boot-starter-oauth2-resource-server'

// Spring Security OAuth2 Client
implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'

// Spring Security
implementation 'org.springframework.boot:spring-boot-starter-security'

// Spring Web
implementation 'org.springframework.boot:spring-boot-starter-web'
```

## OKTA Configuration

### application.properties

```properties
# OKTA Configuration
okta.oauth2.issuer=https://dev-123456.okta.com/oauth2/default
okta.oauth2.client-id=your-client-id
okta.oauth2.client-secret=your-client-secret
okta.oauth2.audience=api://default

# Spring Security OAuth2 Resource Server
spring.security.oauth2.resourceserver.jwt.issuer-uri=${okta.oauth2.issuer}
spring.security.oauth2.resourceserver.jwt.audience=${okta.oauth2.audience}

# Spring Security OAuth2 Client (for login)
spring.security.oauth2.client.registration.okta.client-id=${okta.oauth2.client-id}
spring.security.oauth2.client.registration.okta.client-secret=${okta.oauth2.client-secret}
spring.security.oauth2.client.registration.okta.scope=openid,profile,email
spring.security.oauth2.client.registration.okta.redirect-uri={baseUrl}/login/oauth2/code/okta
spring.security.oauth2.client.provider.okta.issuer-uri=${okta.oauth2.issuer}
```

### application.yml

```yaml
okta:
  oauth2:
    issuer: https://dev-123456.okta.com/oauth2/default
    client-id: your-client-id
    client-secret: your-client-secret
    audience: api://default

spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: ${okta.oauth2.issuer}
          audience: ${okta.oauth2.audience}
      client:
        registration:
          okta:
            client-id: ${okta.oauth2.client-id}
            client-secret: ${okta.oauth2.client-secret}
            scope: openid,profile,email
            redirect-uri: "{baseUrl}/login/oauth2/code/okta"
        provider:
          okta:
            issuer-uri: ${okta.oauth2.issuer}
```

## Resource Server Configuration (API Protection)

### Basic Resource Server Setup

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/api/**").authenticated()
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
        return NimbusJwtDecoder.withIssuerLocation("https://dev-123456.okta.com/oauth2/default")
            .build();
    }
}
```

### JWT Decoder with Custom Validation

```java
@Configuration
public class OktaJwtConfig {
    
    @Value("${okta.oauth2.issuer}")
    private String issuerUri;
    
    @Value("${okta.oauth2.audience}")
    private String audience;
    
    @Bean
    public JwtDecoder jwtDecoder() {
        NimbusJwtDecoder decoder = NimbusJwtDecoder.withIssuerLocation(issuerUri)
            .build();
        
        // Custom validators
        decoder.setJwtValidator(jwtValidator());
        
        return decoder;
    }
    
    @Bean
    public JwtValidator jwtValidator() {
        return new DelegatingOAuth2TokenValidator<>(
            new JwtTimestampValidator(),
            new JwtIssuerValidator(issuerUri),
            new JwtClaimValidator<String>("aud", aud -> aud.contains(audience))
        );
    }
}
```

## OAuth2 Client Configuration (Login Flow)

### Basic OAuth2 Client Setup

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/login", "/error").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2Login(oauth2 -> oauth2
                .defaultSuccessUrl("/dashboard", true)
                .failureUrl("/login?error")
            )
            .logout(logout -> logout
                .logoutSuccessUrl("/")
                .invalidateHttpSession(true)
                .deleteCookies("JSESSIONID")
            );
        return http.build();
    }
}
```

### Custom OAuth2 Login Configuration

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/login", "/error").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2Login(oauth2 -> oauth2
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard", true)
                .failureUrl("/login?error")
                .userInfoEndpoint(userInfo -> userInfo
                    .userAuthoritiesMapper(userAuthoritiesMapper())
                )
            );
        return http.build();
    }
    
    @Bean
    public GrantedAuthoritiesMapper userAuthoritiesMapper() {
        return (authorities) -> {
            Set<GrantedAuthority> mappedAuthorities = new HashSet<>();
            
            authorities.forEach(authority -> {
                if (authority instanceof OidcUserAuthority) {
                    OidcUserAuthority oidcUserAuthority = (OidcUserAuthority) authority;
                    OidcIdToken idToken = oidcUserAuthority.getIdToken();
                    OidcUserInfo userInfo = oidcUserAuthority.getUserInfo();
                    
                    // Extract groups/roles from OKTA claims
                    List<String> groups = idToken.getClaimAsStringList("groups");
                    if (groups != null) {
                        groups.forEach(group -> 
                            mappedAuthorities.add(new SimpleGrantedAuthority("ROLE_" + group))
                        );
                    }
                }
            });
            
            return mappedAuthorities;
        };
    }
}
```

## Working with OKTA Tokens

### Access JWT Claims in Controller

```java
@RestController
@RequestMapping("/api")
public class ApiController {
    
    @GetMapping("/profile")
    public Map<String, Object> getProfile(@AuthenticationPrincipal Jwt jwt) {
        Map<String, Object> profile = new HashMap<>();
        profile.put("subject", jwt.getSubject());
        profile.put("email", jwt.getClaimAsString("email"));
        profile.put("name", jwt.getClaimAsString("name"));
        profile.put("groups", jwt.getClaimAsStringList("groups"));
        return profile;
    }
    
    @GetMapping("/user-info")
    public OidcUser getUserInfo(@AuthenticationPrincipal OidcUser oidcUser) {
        return oidcUser;
    }
}
```

### Extract OKTA Groups/Roles

```java
@Service
public class OktaUserService {
    
    public List<String> extractGroups(Jwt jwt) {
        return jwt.getClaimAsStringList("groups");
    }
    
    public boolean hasGroup(Jwt jwt, String groupName) {
        List<String> groups = extractGroups(jwt);
        return groups != null && groups.contains(groupName);
    }
    
    public boolean hasRole(Jwt jwt, String role) {
        List<String> authorities = jwt.getClaimAsStringList("scp");
        return authorities != null && authorities.contains(role);
    }
}
```

## Method Security with OKTA

### Enable Method Security

```java
@Configuration
@EnableMethodSecurity
public class MethodSecurityConfig {
    // Method security enabled
}
```

### Use Method Security with OKTA Groups

```java
@RestController
@RequestMapping("/api/admin")
public class AdminController {
    
    @GetMapping("/users")
    @PreAuthorize("hasAuthority('ROLE_ADMIN')")
    public List<User> getUsers() {
        return userService.getAllUsers();
    }
    
    @GetMapping("/reports")
    @PreAuthorize("hasAnyAuthority('ROLE_ADMIN', 'ROLE_REPORTER')")
    public List<Report> getReports() {
        return reportService.getAllReports();
    }
}
```

## Custom User Details Service

### Custom OidcUserService

```java
@Service
public class CustomOidcUserService extends OidcUserService {
    
    @Override
    public OidcUser loadUser(OidcUserRequest userRequest) throws OAuth2AuthenticationException {
        OidcUser oidcUser = super.loadUser(userRequest);
        
        // Extract additional information from OKTA
        Map<String, Object> attributes = new HashMap<>(oidcUser.getAttributes());
        
        // Add custom attributes
        attributes.put("customAttribute", "value");
        
        // Build custom user
        return new DefaultOidcUser(
            oidcUser.getAuthorities(),
            userRequest.getIdToken(),
            userRequest.getAccessToken(),
            attributes
        );
    }
}
```

### Register Custom User Service

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Autowired
    private CustomOidcUserService customOidcUserService;
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .oauth2Login(oauth2 -> oauth2
                .userInfoEndpoint(userInfo -> userInfo
                    .oidcUserService(customOidcUserService)
                )
            );
        return http.build();
    }
}
```

## OKTA API Integration

### OKTA Management API Client

```java
@Configuration
public class OktaApiConfig {
    
    @Value("${okta.api.token}")
    private String apiToken;
    
    @Value("${okta.org.url}")
    private String orgUrl;
    
    @Bean
    public Client oktaClient() {
        return Clients.builder()
            .setOrgUrl(orgUrl)
            .setClientCredentials(new TokenClientCredentials(apiToken))
            .build();
    }
}
```

### User Management via OKTA API

```java
@Service
public class OktaUserManagementService {
    
    @Autowired
    private Client oktaClient;
    
    public User createUser(String email, String firstName, String lastName) {
        User user = UserBuilder.instance()
            .setEmail(email)
            .setFirstName(firstName)
            .setLastName(lastName)
            .setPassword("TempPassword123!")
            .setActive(true)
            .buildAndCreate(oktaClient);
        
        return user;
    }
    
    public void assignUserToGroup(String userId, String groupId) {
        Group group = oktaClient.getGroup(groupId);
        User user = oktaClient.getUser(userId);
        group.addUser(user);
    }
    
    public void deactivateUser(String userId) {
        User user = oktaClient.getUser(userId);
        user.deactivate();
    }
}
```

## Testing

### Test with Mock OKTA JWT

```java
@SpringBootTest
@AutoConfigureMockMvc
class ApiControllerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void testGetProfileWithValidJwt() throws Exception {
        String jwt = createMockOktaJwt();
        
        mockMvc.perform(get("/api/profile")
                .header("Authorization", "Bearer " + jwt))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.email").exists());
    }
    
    private String createMockOktaJwt() {
        // Create mock JWT with OKTA claims
        // This is a simplified example - use a JWT library in practice
        return "mock.jwt.token";
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
        return new MockJwtDecoder();
    }
}
```

## Common Patterns

### Pattern 1: Resource Server with OKTA

Protect REST APIs using OKTA-issued JWT tokens.

```java
@Configuration
@EnableWebSecurity
public class ResourceServerConfig {
    
    @Bean
    public SecurityFilterChain apiSecurityFilterChain(HttpSecurity http) throws Exception {
        http
            .securityMatcher("/api/**")
            .authorizeHttpRequests(auth -> auth
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2.jwt());
        return http.build();
    }
}
```

### Pattern 2: OAuth2 Login with OKTA

Implement OAuth2 login flow using OKTA as the identity provider.

```java
@Configuration
@EnableWebSecurity
public class OAuth2LoginConfig {
    
    @Bean
    public SecurityFilterChain webSecurityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/login").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2Login(oauth2 -> oauth2
                .defaultSuccessUrl("/dashboard", true)
            );
        return http.build();
    }
}
```

### Pattern 3: Hybrid Approach

Combine OAuth2 login for web interface and JWT validation for API.

```java
@Configuration
@EnableWebSecurity
public class HybridSecurityConfig {
    
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
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/login").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2Login();
        return http.build();
    }
}
```

## Troubleshooting

### Issue: JWT validation fails

**Solution**: 
- Verify issuer URI matches OKTA configuration
- Check audience claim matches API audience
- Ensure OKTA application is properly configured
- Verify token hasn't expired

### Issue: Groups not mapped to authorities

**Solution**: 
- Configure `userAuthoritiesMapper` in OAuth2 login
- Check OKTA group claims are included in token
- Verify group names match expected format

### Issue: Redirect URI mismatch

**Solution**: 
- Ensure redirect URI in application matches OKTA app configuration
- Use exact match (no trailing slashes)
- Verify HTTPS is used in production

## Links

[OKTA Developer Documentation](https://developer.okta.com/docs/)

[OKTA Spring Boot Integration](https://developer.okta.com/docs/guides/implement-oauth-for-okta/springboot/main/)

[OKTA API Reference](https://developer.okta.com/docs/reference/api/)

[OKTA Java SDK](https://github.com/okta/okta-sdk-java)

[Spring Security OAuth2 Resource Server](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/index.html)

[Spring Security OAuth2 Client](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/index.html)

