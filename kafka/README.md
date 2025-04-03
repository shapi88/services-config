
1. Start the containers using Docker Compose:
```bash
docker-compose up -d
```

Key configuration explanations:
- `zookeeper`: Runs ZooKeeper, which Kafka needs for coordination
- `kafka`: Runs the Kafka broker
- `KAFKA_BROKER_ID`: Unique identifier for the broker
- `KAFKA_ZOOKEEPER_CONNECT`: Tells Kafka where to find ZooKeeper
- `KAFKA_ADVERTISED_LISTENERS`: How clients should connect to Kafka
- Port 2181: ZooKeeper client port
- Port 9092: Kafka client port

2. To verify it's running:
```bash
docker ps
```

3. To test creating a topic (from your host machine):
```bash
docker exec -it <kafka-container-name> kafka-topics --create --topic test-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
docker exec -it <kafka-container-name> kafka-console-consumer --topic test-topic --bootstrap-server localhost:9092 --group my-consumer-group

docker exec -it kafka-kafka-1 kafka-console-consumer --topic smd-consumer-topic --bootstrap-server localhost:9092 --group psc-consumer-group
docker exec -it kafka-kafka-1 kafka-topics --create --topic smd-producer-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1 --group psc-consumer-group
```

4. To stop the services:
```bash
docker-compose down
```

Additional tips:
- Add `volumes` to persist data:
```yaml
  kafka:
    volumes:
      - kafka-data:/var/lib/kafka/data
volumes:
  kafka-data:
```

- For production, modify environment variables like:
  - `KAFKA_NUM_PARTITIONS`
  - `KAFKA_MESSAGE_MAX_BYTES`
  - Increase replication factor (>1) with multiple brokers

This sets up a basic single-broker Kafka instance. Let me know if you need help with multi-broker setup, security configuration, or specific use cases!