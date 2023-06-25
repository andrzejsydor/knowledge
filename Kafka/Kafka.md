# Kafka

[Kafka Visualization](https://softwaremill.com/kafka-visualisation/)



## Message delivery

* none: The producer considers the records successfully delivered once it sends the records to the broker. This is basically “fire and forget.”
* one: The producer waits for the lead broker to acknowledge that it has written the record to its log.
* all: The producer waits for an acknowledgment from the lead broker and from the following brokers that they have successfully written the record to their logs.

![Kafka_Message_Delivery](Kafka_Message_Delivery.png)

## Commands

![Kafka_Commands](Kafka_Commands.png)
