# kafkadbsql
Kafka db sql
https://stackoverflow.com/questions/53247553/kafka-access-inside-and-outside-docker


Ini untuk yang benarnya
======================================================================
version: '3'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"

  kafka:
   image: wurstmeister/kafka
   ports:
     - "9092:9092"
   environment:
     KAFKA_ADVERTISED_HOST_NAME: 128.21.33.43
     KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
     KAFKA_AUTO_CREATE_TOPICS_ENABLE: "true"
   volumes:
     - /var/run/docker.sock:/var/run/docker.sock
=============================================================================
---
version: '2'

services:

  ksqldb-server:
    image: confluentinc/ksqldb-server:0.14.0
    hostname: ksqldb-server
    container_name: ksqldb-server
    ports:
      - "8090:8088"
    environment:
     KSQL_LISTENERS: http://0.0.0.0:8090
     KSQL_BOOTSTRAP_SERVERS: 128.21.33.43:9092
     KSQL_KSQL_LOGGING_PROCESSING_STREAM_AUTO_CREATE: "true"
     KSQL_KSQL_LOGGING_PROCESSING_TOPIC_AUTO_CREATE: "true"

  ksqldb-cli:
    image: confluentinc/ksqldb-cli:0.14.0
    container_name: ksqldb-cli
    depends_on:
     - ksqldb-server
    entrypoint: /bin/sh
    tty: true





