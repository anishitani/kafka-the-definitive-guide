# Study Group - Kafka: The Definitive Guide

## Requirements

You'll need Docker Compose installed in order to run the cluster in this project.

## Running Cluster

Run command

```shell
docker-compose up -d --scale kafka=$NUMBER_OF_BROKERS
docker-compose ps
```

## Inspecting cluster with Kafkacat

Using one of the brokers listed using `docker-compose ps`, one can get the list of listerners with Kafkacat.
The following command gets the list of of brokers listening in the cluster.

```shell
docker run --rm --network host confluentinc/cp-kafkacat kafkacat -b localhost:$PORT_FROM_DOCKER_COMPOSE_PS -L
```

The following is a sample result

```raw
Metadata for all topics (from broker -1: localhost:9099/bootstrap):
 5 brokers:
  broker 1001 at 172.27.0.5:9094 (controller)
  broker 1005 at 172.27.0.9:9094
  broker 1003 at 172.27.0.8:9094
  broker 1004 at 172.27.0.7:9094
  broker 1002 at 172.27.0.6:9094
 1 topics:
  topic "test" with 5 partitions:
    partition 0, leader 1003, replicas: 1003,1005,1001,1002,1004, isrs: 1003,1005,1001,1002,1004
    partition 1, leader 1004, replicas: 1004,1001,1002,1003,1005, isrs: 1004,1001,1002,1003,1005
    partition 2, leader 1005, replicas: 1005,1002,1003,1004,1001, isrs: 1005,1002,1003,1004,1001
    partition 3, leader 1001, replicas: 1001,1003,1004,1005,1002, isrs: 1001,1003,1004,1005,1002
    partition 4, leader 1002, replicas: 1002,1004,1005,1001,1003, isrs: 1002,1004,1005,1001,1003

```

## Trying it out

Let's first create a topic using console scripts from Kafka.

We'll assume a cluster with 5 Kafka brokers and we'll create a topic with the same number of partitions as brokers.
Replication we'll be set to the same value.

```shell
docker run --rm --network host wurstmeister/kafka \
    kafka-topics.sh --create --bootstrap-server localhost:$PORT_FROM_DOCKER_COMPOSE_PS --replication-factor 5 --partitions 5 --topic test
```

Okay, we can now start a console producer and consumer applications, also from Kafka installation.
Start the `producer` in a new console with

```shell
docker run -it --rm --network host wurstmeister/kafka \
    kafka-console-producer.sh --bootstrap-server localhost:$PORT_FROM_DOCKER_COMPOSE_PS --topic test
```

and the `consumer` with

```shell
docker run -it --rm --network host wurstmeister/kafka \
    kafka-console-consumer.sh --bootstrap-server localhost:$PORT_FROM_DOCKER_COMPOSE_PS --topic test --from-beginning
```

and that's it!

Start typing in the `producer` console and the messages we'll be displayed at the `consumer` console.
