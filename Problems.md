---
tags:
  - Problem
---

# Problems #Problem

## Table of Contents  

1. [Convert Camel Case to Snake Case in Java](#convert-camel-case-to-snake-case-in-java)  
   1. [Plain Java](#plain-java)  
   2. [Regular Expression](#regular-expression)  
2. [PathVariable as a DTO](#pathvariable-as-a-dto)  
3. [Lombok extend Value](#lombok-extend-value)

## Convert Camel Case to Snake Case in Java

### Plain Java

```
public static String convertCamelCaseToSnake(String input) {
    StringBuilder result = new StringBuilder();
    for (char c: input.toCharArray()) {
        if (Character.isUpperCase(c)) {
            result.append("_").append(Character.toLowerCase(c));
        } else {
            result.append(c);
        }
    }
    return result.toString();
}
```

### Regular Expression

```
public static String convertCamelCaseToSnakeRegex(String input) {
    return input
      .replaceAll("([A-Z])(?=[A-Z])", "$1_")
      .replaceAll("([a-z])([A-Z])", "$1_$2")
      .toLowerCase();
}
```

## PathVariable as a DTO

```
import lombok.Value;

import java.util.UUID;

@Value
public class MyId {
    UUID id;
}
```

```
  @GetMapping("/{myId}")
  String exampleGet(@PathVariable MyId myId) {
      System.out.println(myId);
      return myId.toString();
  }
```

```
import org.springframework.core.convert.converter.Converter;
import org.springframework.stereotype.Component;

import java.util.UUID;

@Component
public class StringToMyIdTimeConverter
  implements Converter<String, MyId> {

    @Override
    public MyId convert(String source) {
        UUID uuid = UUID.fromString(source);
        MyId myId = new MyId(uuid);
        return myId;
    }
}
```

## Lombok extend Value

```
import lombok.AllArgsConstructor;
import lombok.EqualsAndHashCode;
import lombok.Getter;

import java.util.UUID;

@Getter
@AllArgsConstructor
@EqualsAndHashCode
public class MyId {
    private final UUID id;
}
```

```
import lombok.EqualsAndHashCode;
import lombok.Value;

import java.util.UUID;

@Value
@EqualsAndHashCode(callSuper = true)
public class ConcreteId extends MyId {
    public ConcreteId(UUID id) {
        super(id);
    }
}
```
