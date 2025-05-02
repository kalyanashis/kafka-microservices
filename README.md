# kafka-microservices

#docker-compose file steps explanation

_**1. Zookeeper is required for Kafka to work.**_

zookeeper:
image: confluentinc/cp-zookeeper:7.5.0

_**Uses Confluent's Zookeeper Docker image, version 7.5.0.**_

    container_name: zookeeper

_**Sets a custom container name (easier to reference).**_

    ports:
      - "2181:2181"

_**Maps port 2181 of the host to the same port in the container — this is the default Zookeeper port.**_

    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000

_**Environment variables to configure Zookeeper.

ZOOKEEPER_CLIENT_PORT: port Zookeeper listens on for client connections.

ZOOKEEPER_TICK_TIME: base time unit (in ms) used for Zookeeper timeouts.**_


_**2. kafka: Service**_


kafka:
image: confluentinc/cp-kafka:7.5.0

_**Uses the Kafka Docker image from Confluent (includes Kafka + built-in CLI tools).**_


    container_name: kafka

_**Custom container name.**_


    ports:
      - "9092:9092"

_**Maps Kafka's default broker port (9092) to the host — needed for producer/consumer apps to connect.**_


    depends_on:
      - zookeeper

_**Ensures Zookeeper starts before Kafka. Kafka depends on it.**_


    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1

_**KAFKA_BROKER_ID: Unique ID for this Kafka broker.

KAFKA_ZOOKEEPER_CONNECT: Tells Kafka how to find Zookeeper.

KAFKA_ADVERTISED_LISTENERS: How Kafka advertises itself to external clients (use localhost for local development).

KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: Set to 1 because we’re using a single broker. In production, use at least 3.**_


| Component | Role                       | Port |
| --------- | -------------------------- | ---- |
| Zookeeper | Kafka coordination service | 2181 |
| Kafka     | Message broker             | 9092 |
