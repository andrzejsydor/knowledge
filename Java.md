# Java

[Constraing Composition](https://www.baeldung.com/java-bean-validation-constraint-composition)

JSR 380: Bean Validation 2.0

Javax Validation - https://www.baeldung.com/javax-validation


```
List<@NotBlankString> preferences;
```

The specification supports the new Optional type:
```
privateLocalDate dateOfBirth;
publicOptional<@PastLocalDate> getDateOfBirth() {
    returnOptional.of(dateOfBirth);
}
```
