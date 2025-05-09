---
tags:
  - Java
  - Spring
---

# #Java

---
- [Constraing Composition](#Constraining-Composition)
- [Spring](#Spring)
- [Other](#Other)
	- [Failsafe](##Failsafe)
 	- [immutables](https://github.com/immutables/immutables)	
- [Links](#Link)
---


## [Constraing Composition](https://www.baeldung.com/java-bean-validation-constraint-composition)

JSR 380: Bean Validation 2.0

Javax Validation - https://www.baeldung.com/javax-validation


```
List<@NotBlankString> preferences;
```

The specification supports the new Optional type:
```
private LocalDate dateOfBirth;
public Optional<@PastLocalDate> getDateOfBirth() {
    returnOptional.of(dateOfBirth);
}
```

###########################

# #Spring
## Spring MVC test - page response

```java
package com.example.demo;

import lombok.Data;
import lombok.RequiredArgsConstructor;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@RequiredArgsConstructor
@RestController
class UserController {
    private final UserRepository repository;

    @GetMapping("/list")
    Page<Product> all(Pageable pageable) {
        return repository.findAll(pageable);
    }
}

interface UserRepository extends JpaRepository<Product, Long> {
}

@Data
@Entity
class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String name;
}
```


```java
package com.example.demo;

import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.domain.Page;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;

import static org.junit.jupiter.api.Assertions.assertEquals;

@SpringBootTest
@AutoConfigureMockMvc
class ProductControllerTest {

    @Autowired
    UserRepository repository;
    @Autowired
    MockMvc mvc;

    @Test
    void filterTest() throws Exception {
        repository.save(getEntity());

        String content = mvc.perform(MockMvcRequestBuilders.get("/list")
                ).andReturn()
                .getResponse().getContentAsString();

        Page<Product> page = new ObjectMapper()
                .readValue(content, new TypeReference<RestPageImpl<Product>>() {
                });
        assertEquals(page.getContent().get(0).getName(), "product1");
    }

    private Product getEntity() {
        Product product = new Product();
        product.setName("product1");
        return product;
    }
}
```

```java
package com.example.demo;

import com.fasterxml.jackson.annotation.JsonCreator;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.databind.JsonNode;
import org.springframework.data.domain.PageImpl;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;

import java.util.ArrayList;
import java.util.List;

public class RestPageImpl<T> extends PageImpl<T> {
 
    @JsonCreator(mode = JsonCreator.Mode.PROPERTIES)
    public RestPageImpl(@JsonProperty("content") List<T> content,
                        @JsonProperty("number") int number,
                        @JsonProperty("size") int size,
                        @JsonProperty("totalElements") Long totalElements,
                        @JsonProperty("pageable") JsonNode pageable,
                        @JsonProperty("last") boolean last,
                        @JsonProperty("totalPages") int totalPages,
                        @JsonProperty("sort") JsonNode sort,
                        @JsonProperty("first") boolean first,
                        @JsonProperty("numberOfElements") int numberOfElements,
                        @JsonProperty("empty") boolean empty) {

        super(content, PageRequest.of(number, size), totalElements);
    }
 
    public RestPageImpl(List<T> content, Pageable pageable, long total) {
        super(content, pageable, total);
    }
 
    public RestPageImpl(List<T> content) {
        super(content);
    }
 
    public RestPageImpl() {
        super(new ArrayList<>());
    }
}
```



# Other

## Failsafe
[Failsafe](https://failsafe.dev/) is a lightweight, zero-dependency library for handling failures in Java 8+. It has a concise API for handling everyday use cases and the flexibility to handle everything else.

```Java
RetryPolicy<Object> retryPolicy = RetryPolicy.builder()
  .handle(ConnectException.class)
  .withDelay(Duration.ofSeconds(1))
  .withMaxRetries(3)
  .build();

// Run with retries 
Failsafe.with(retryPolicy).run(() -> connect());  
// Get with retries 
Connection connection = Failsafe.with(retryPolicy).get(() -> connect());`
```



# Link

[Spring Cloud Config Server](https://github.com/hyness/spring-cloud-config-server)
