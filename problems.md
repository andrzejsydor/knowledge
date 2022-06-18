# Problems

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
