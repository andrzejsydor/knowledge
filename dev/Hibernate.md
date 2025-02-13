# Hibernate

## Spring JPA - generate statistics

```
spring.jpa.properties.hibernate.generate_statistics=true
```

### Don’t use show_sql to log SQL queries
Better way
```
org.hibernate.SQL=DEBUG
org.hibernate.type.descriptor.sql:DEBUG

# SQL statements and parameters
log4j.logger.org.hibernate.SQL=debug
log4j.logger.org.hibernate.orm.jdbc.bind=trace
# Statistics and slow queries
log4j.logger.org.hibernate.stat=debug
log4j.logger.org.hibernate.SQL_SLOW=info
```

## get vs load

| **Feature**                        | **get()**                                          | **load()**                                                    |
| ---------------------------------- | -------------------------------------------------- | -------------------------------------------------------------- |
| **Loading Strategy**               | Eager loading (immediate database query)           | Lazy loading (proxy object, data fetched on access)            |
| **Return Value (if exists)**       | Actual object                                      | Proxy object                                                   |
| **Database Query**                 | Executes immediately                               | Executes when a property is accessed                           |
| **Return Value (if not exists)**   | null                                              | Results in an **ObjectNotFoundException** when accessing properties |
| **Hibernate Cache**                | Retrieves from the cache if present               | Still returns proxy even if cached                             |

## HikariCP

[https://github.com/openbouquet/HikariCP](https://github.com/openbouquet/HikariCP)

[https://www.baeldung.com/hikaricp](https://www.baeldung.com/hikaricp)

[https://www.baeldung.com/spring-boot-hikari](https://www.baeldung.com/spring-boot-hikari)

[https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing](https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing)

### HikariCP stackoverflow

[How do I configure HikariCP in my Spring Boot app in my application.properties files?](https://stackoverflow.com/questions/26490967/how-do-i-configure-hikaricp-in-my-spring-boot-app-in-my-application-properties-f)

## JPlusOne

[JPlusOne Hibernate n+1](https://github.com/adgadev/jplusone)

## Hypersistence Optimizer

[Hypersistence Optimizer](https://vladmihalcea.com/hypersistence-optimizer/)

[hypersistence-optimizer](https://github.com/vladmihalcea/hypersistence-optimizer)


## Hibernate Types

[Hibernate Types](https://github.com/vladmihalcea/hibernate-types)

# #Hibernate #Link 

[Thorben Janssen YT](https://www.youtube.com/channel/UCYeDPubBiFCZXIOgGYoyADw)

[How does orphanRemoval work with JPA and Hibernate](https://vladmihalcea.com/orphanremoval-jpa-hibernate/)

[Implementing equals() and hashCode()](https://thorben-janssen.com/ultimate-guide-to-implementing-equals-and-hashcode-with-hibernate/)

[The best way to implement equals, hashCode, and toString with JPA and Hibernate](https://vladmihalcea.com/the-best-way-to-implement-equals-hashcode-and-tostring-with-jpa-and-hibernate/)

[How to generate UUIDs as primary keys with Hibernate](https://thorben-janssen.com/generate-uuids-primary-keys-hibernate/)

[Modeling self-referencing associations with Hibernate](https://thorben-janssen.com/self-referencing-associations/?ck_subscriber_id=198329661)

[Spring Data JPA – How to Return DTOs from Native Queries](https://thorben-janssen.com/spring-data-jpa-dto-native-queries/)

[The JPA and Hibernate second-level cache](https://vladmihalcea.com/jpa-hibernate-second-level-cache/)

[https://thorben-janssen.com/hibernate-logging-guide/](https://thorben-janssen.com/hibernate-logging-guide/)

[6 Performance Pitfalls when using Spring Data JPA](https://thorben-janssen.com/6-performance-pitfalls-when-using-spring-data-jpa/)

[Native Queries – How to call native SQL queries with JPA & Hibernate](https://thorben-janssen.com/jpa-native-queries/)

[A beginner’s guide to database locking and the lost update phenomena](https://vladmihalcea.com/a-beginners-guide-to-database-locking-and-the-lost-update-phenomena/)

[Spring Transaction and Connection Management](https://vladmihalcea.com/spring-transaction-connection-management/)

# Tips 

[The Spring Data findAll Anti-Pattern](https://vladmihalcea.com/spring-data-findall-anti-pattern/)

[Spring Data JPA MultipleBagFetchException](https://vladmihalcea.com/spring-data-jpa-multiplebagfetchexception/)

[Spring MDC Transaction Logging](https://vladmihalcea.com/spring-mdc-transaction-logging/)

[The best way to validate the DDL schema with Spring and Hibernate](https://vladmihalcea.com/validate-ddl-schema-spring-hibernate/)

