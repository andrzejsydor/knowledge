---
tags:
  - Architecture
  - Outbox_Pattern
  - Inbox_Pattern
---
# Architecture

**Outbox Pattern** #Outbox_Pattern  - it ensures that a message was sent (e.g. to a queue) successfully at least once. With this pattern, instead of directly publishing a message to the queue, we store it in temporary storage (e.g. database table).

**Inbox Pattern** #Inbox_Pattern - it is similar to Outbox Pattern. It’s used to handle incoming messages (e.g. from a queue). Accordingly, we have a table in which we’re storing incoming events. Contrary to outbox pattern, we first save the event in the database, then we’re returning ACK to queue


![](https://event-driven.io/static/614379d9263d1b1395bf8ad305047ed3/a331c/2020-12-30-outbox.png)


### Delivery Guarantees

- **At-most once**
- **At-least once**
- **Exactly-once**

# Other

### ShedLock

ShedLock — a Java library that makes sure our scheduled tasks run only once at the same time **and is an alternative to [Quartz](https://www.baeldung.com/quartz)**


# Links

[# Outbox, Inbox patterns and delivery guarantees explained](https://event-driven.io/en/outbox_inbox_patterns_and_delivery_guarantees_explained/) #Outbox_Pattern 

[# ShedLock with Spring](https://www.baeldung.com/shedlock-spring) 
