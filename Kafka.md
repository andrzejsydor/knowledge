
# Kafka

- [Tools](https://github.com/andrzejsydor/knowledge/blob/main/Kafka.md#tools)

- [Problems](https://github.com/andrzejsydor/knowledge/blob/main/Kafka.md#problems)

- [Links](https://github.com/andrzejsydor/knowledge/blob/main/Kafka.md#links)

## [Kafka Visualisation](https://softwaremill.com/kafka-visualisation/)

# Tools

## [https://akhq.io/docs/#installation](akhq.io)

Kafka GUI for Apache Kafka ® to manage topics, topics data, consumers group, schema registry, connect and more...

# Problems

## Poison Pill

[What is a poison pill?](https://www.confluent.io/blog/spring-kafka-can-your-kafka-consumers-handle-a-poison-pill/#:~:text=A%20poison%20pill%20(in%20the,A%20deserialization%20failure)


[https://stephantromer.dev/blog/how-to-handle-poison-pills-in-kafka](https://stephantromer.dev/blog/how-to-handle-poison-pills-in-kafka)



# Links

[https://kafka.apache.org/quickstart](quickstart)

[Kafka Download](https://kafka.apache.org/downloads)

[Kafka for M1(kymeric/cp-kafka)](https://hub.docker.com/r/kymeric/cp-kafka)



### [kafkacat](https://github.com/edenhill/kcat)

[https://docs.confluent.io/platform/current/app-development/kafkacat-usage.html](https://docs.confluent.io/platform/current/app-development/kafkacat-usage.html)

### [Conductor](https://www.conduktor.io/)

## Scripts

### Topics:


Topic Options


Create a topic only if a topic with the same name doesn't exist: 

	bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --create --topic test_topic --replication-factor 2 --partitions 2 --if-not-exists

List the topics: 

	bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --list

Describe the topic: 

	bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --topic test_topic --describe

Alter the number of partitions: 

	bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --alter --topic test_topic --partitions 3

Delete a topic: 

	bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --delete --topic test_topic

Create a topic with more replications than available brokers: 

	bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --create --topic test_topic --replication-factor 4 --partitions 2

Dump the log segments: 

	bin/kafka-run-class.sh kafka.tools.DumpLogSegments --print-data-log --files /data/kafka/topic-1-0/00000000000000000001.log

Rotate the logs: 

	bin/kafka-configs.sh --zookeeper zookeeper1:2181/kafka --alter --entity-type topics --entity-name topic-1 --add-config segment.ms=60000

