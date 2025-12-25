# Kafka Docker Compose

A Docker Compose setup for running Apache Kafka locally with Zookeeper and Kafka UI for easy monitoring and management.

## 📋 Overview

This repository provides a ready-to-use Docker Compose configuration for running a local Kafka cluster. It includes:

- **Apache Kafka** - Distributed streaming platform
- **Zookeeper** - Coordination service for Kafka
- **Kafka UI** - Web-based interface for monitoring and managing Kafka

## 🚀 Quick Start

### Prerequisites

- Docker Engine 20.10+
- Docker Compose 2.0+

### Starting the Services

```bash
docker-compose up -d
```

### Stopping the Services

```bash
docker-compose down
```

### Removing All Data

```bash
docker-compose down -v
```

## 🔧 Services

### Zookeeper

- **Image**: `confluentinc/cp-zookeeper:7.5.0`
- **Port**: `2181`
- **Purpose**: Manages Kafka cluster coordination and metadata

### Kafka Broker

- **Image**: `confluentinc/cp-kafka:7.5.0`
- **Ports**: 
  - `9092` - External access (localhost)
  - `29092` - Internal Docker network access
- **Purpose**: Message broker for event streaming

**Connection Strings:**
- From host machine: `localhost:9092`
- From Docker containers: `kafka:29092`

### Kafka UI

- **Image**: `provectuslabs/kafka-ui:latest`
- **Port**: `8080`
- **Access**: http://localhost:8080
- **Purpose**: Web interface for managing topics, messages, consumers, and monitoring

## 📊 Accessing Kafka UI

Once the services are running, open your browser and navigate to:

```
http://localhost:8080
```

From the UI, you can:
- Create and manage topics
- View and send messages
- Monitor consumer groups
- View broker configurations
- Inspect cluster health

## 🔌 Connecting Your Application

### From Host Machine

```properties
bootstrap.servers=localhost:9092
```

### From Docker Container

Add your application to the `kafka-net` network and use:

```properties
bootstrap.servers=kafka:29092
```

**Example docker-compose.yml for your app:**

```yaml
services:
  your-app:
    image: your-app-image
    networks:
      - kafka-net

networks:
  kafka-net:
    external: true
```

## ⚙️ Configuration

### Key Kafka Settings

| Environment Variable | Value | Description |
|---------------------|-------|-------------|
| `KAFKA_BROKER_ID` | `1` | Unique broker identifier |
| `KAFKA_ZOOKEEPER_CONNECT` | `zookeeper:2181` | Zookeeper connection string |
| `KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR` | `1` | Replication factor for offset topic |
| `KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS` | `0` | Consumer group rebalance delay |

### Network Configuration

All services are connected via the `kafka-net` bridge network, enabling seamless communication between containers.

## 🛠️ Troubleshooting

### Kafka Connection Refused

If you get "connection refused" errors:

1. Ensure all containers are running:
   ```bash
   docker-compose ps
   ```

2. Check Kafka logs:
   ```bash
   docker-compose logs kafka
   ```

3. Verify Zookeeper is healthy:
   ```bash
   docker-compose logs zookeeper
   ```

### Port Already in Use

If ports `2181`, `8080`, or `9092` are already in use, modify the port mappings in `docker-compose.yml`:

```yaml
ports:
  - "9093:9092"  # Change host port to 9093
```

### Container Won't Start

Remove existing volumes and restart:

```bash
docker-compose down -v
docker-compose up -d
```

## 📝 Alternative Configurations

This repository includes several docker-compose variants:

- `docker-compose.yml` - Full setup with Kafka UI and explicit networking
- `docker-compose.yml-2` - Minimal setup without Kafka UI
- `docker-compose.yml-3` - Setup with Kafka UI but without explicit network definition

## 🔍 Useful Commands

### View Logs

```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f kafka
```

### Check Service Status

```bash
docker-compose ps
```

### Restart a Service

```bash
docker-compose restart kafka
```

### Execute Commands in Kafka Container

```bash
docker-compose exec kafka bash
```

### Create a Topic Manually

```bash
docker-compose exec kafka kafka-topics --create \
  --bootstrap-server localhost:9092 \
  --topic my-topic \
  --partitions 3 \
  --replication-factor 1
```

### List Topics

```bash
docker-compose exec kafka kafka-topics --list \
  --bootstrap-server localhost:9092
```

### Produce Messages

```bash
docker-compose exec kafka kafka-console-producer \
  --bootstrap-server localhost:9092 \
  --topic my-topic
```

### Consume Messages

```bash
docker-compose exec kafka kafka-console-consumer \
  --bootstrap-server localhost:9092 \
  --topic my-topic \
  --from-beginning
```

## 📚 Additional Resources

- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [Confluent Platform Documentation](https://docs.confluent.io/)
- [Kafka UI Documentation](https://docs.kafka-ui.provectus.io/)

## 📄 License

This configuration is provided as-is for development and testing purposes.

## 🤝 Contributing

Feel free to submit issues or pull requests to improve this setup.
