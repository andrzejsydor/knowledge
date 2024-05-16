---
tags:
  - Kafka
---

# #Kafka


[Kafka Visualization](https://softwaremill.com/kafka-visualisation/)

[Kafka Monitoring](https://kafka.apache.org/documentation/#monitoring)

[Kafka Security]([https://kafka.apache.org/documentation/#security](https://kafka.apache.org/documentation/#security))


#### Kafka trust properties
[spring.kafka.consumer.properties.spring.json.trusted.packages](http://spring.kafka.consumer.properties.spring.json.trusted.packages) specifies comma-delimited list of package patterns allowed for deserialization. '*' means deserialize all packages.


## Message delivery

* none: The producer considers the records successfully delivered once it sends the records to the broker. This is basically “fire and forget.”
* one: The producer waits for the lead broker to acknowledge that it has written the record to its log.
* all: The producer waits for an acknowledgment from the lead broker and from the following brokers that they have successfully written the record to their logs.

### **Message Delivery Semantics**

- _At most once_—Messages may be lost but are never redelivered.
    
- _At least once_—Messages are never lost but may be redelivered.
    
- _Exactly once_—this is what people actually want, each message is delivered once and only once.

![Kafka_Message_Delivery](Kafka_Message_Delivery.png)

## Commands

![Kafka_Commands](Kafka_Commands.png)
