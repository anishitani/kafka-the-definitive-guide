# Study Group - Kafka: The Definitive Guide

## Requirements

You'll need Docker Compose installed in order to run the cluster in this project.

## Running Cluster

Run command

```shell
docker-compose up -d --scale kafka=$NUMBER_OF_BROKERS
docker-compose ps
```
