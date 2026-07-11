---
tags:
  - Kafka
---

# #Kafka

- [Tools](#tools)

- [Problems](#problem)
  - Poison Pill
  - Consumer Group offset change

- [Links](#links)
  - [Kafka Visualisation](https://softwaremill.com/kafka-visualisation/)
  - [Kafka Deep Dive](https://lucid.app/lucidchart/5580e561-c75f-495d-b480-e303a74d84e1/view)
  - [Zero Copy Basics](https://blog.2minutestreaming.com/p/apache-kafka-zero-copy-operating-system-optimization)

- [Certification](#certification)


# Tools

## [akhq.io](https://akhq.io/docs/#installation)

Kafka GUI for Apache Kafka ® to manage topics, topics data, consumers group, schema registry, connect and more...

# #Problem

## Poison Pill

[What is a poison pill?](https://www.confluent.io/blog/spring-kafka-can-your-kafka-consumers-handle-a-poison-pill/#poison-pill)


[https://stephantromer.dev/blog/how-to-handle-poison-pills-in-kafka](https://stephantromer.dev/blog/how-to-handle-poison-pills-in-kafka)


## How to Reset Kafka Consumer Group Offset

[How to Reset Kafka Consumer Group Offset - GIST](https://gist.github.com/marwei/cd40657c481f94ebe273ecc16601674b)

```
kafka-consumer-groups --bootstrap-server <kafkahost:port> --group <group_id> --topic <topic_name> --reset-offsets --to-earliest --execute
```

Resetting options:
```
--shift-by <positive_or_negative_integer>
--to-current
--to-latest
--to-offset <offset_integer>
--to-datetime <datetime_string>
--by-duration <duration_string>
```

# #Links

[Kafka vs RabbitMQ — comparison & cheat sheet](Kafka_vs_RabbitMQ.md)

[Kafka vs RabbitMQ — System Design Interview Guide](Kafka_vs_RabbitMQ_Interview_Guide.md)

[Kafka Quickstart](https://kafka.apache.org/quickstart)

[Kafka Documentation](https://kafka.apache.org/documentation/)

[Kafka Download](https://kafka.apache.org/downloads)

[Kafka for M1(kymeric/cp-kafka)](https://hub.docker.com/r/kymeric/cp-kafka)


### [kafkacat](https://github.com/edenhill/kcat)

[https://docs.confluent.io/platform/current/app-development/kafkacat-usage.html](https://docs.confluent.io/platform/current/app-development/kafkacat-usage.html)

### [Conductor](https://www.conduktor.io/)

# Certification

[Certification](https://www.confluent.io/certification/)

[https://codingnconcepts.com/post/apache-kafka-ccdak-exam-notes/](https://codingnconcepts.com/post/apache-kafka-ccdak-exam-notes/)


# Scripts

Note: since Kafka 3.x the CLI tools use `--bootstrap-server` (ZooKeeper is deprecated and removed in Kafka 4.0 in favour of KRaft).

### Topics:


Topic Options


Create a topic only if a topic with the same name doesn't exist: 

	bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test_topic --replication-factor 2 --partitions 2 --if-not-exists

List the topics: 

	bin/kafka-topics.sh --bootstrap-server localhost:9092 --list

Describe the topic: 

	bin/kafka-topics.sh --bootstrap-server localhost:9092 --topic test_topic --describe

Alter the number of partitions: 

	bin/kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic test_topic --partitions 3

Delete a topic: 

	bin/kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic test_topic

Create a topic with more replications than available brokers: 

	bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test_topic --replication-factor 4 --partitions 2

Dump the log segments: 

	bin/kafka-run-class.sh kafka.tools.DumpLogSegments --print-data-log --files /data/kafka/topic-1-0/00000000000000000001.log

Rotate the logs: 

	bin/kafka-configs.sh --bootstrap-server localhost:9092 --alter --entity-type topics --entity-name topic-1 --add-config segment.ms=60000

