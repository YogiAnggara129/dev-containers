# **🚀 Kafka Dev Container**

This service provides a standalone Apache Kafka container for development and testing without Zookeeper.

---

## **🔧 Configuration**

| **Variable** | **Default** | **Description** |
| --- | --- | --- |
| **`CONTAINER_NAME`** | **`kafka`** | Container name |
| **`IMAGE_NAME`** | **`bitnami/kafka`** | Docker image |
| **`IMAGE_TAG`** | **`3.6.0`** | Kafka version |
| **`PORT_BROKER`** | **`9092`** | Kafka broker port |
| **`PORT_CONTROLLER`** | **`9093`** | Controller port |

> ⚠️ This setup is for local development only and does not persist data between container restarts.
> 

---

## **📦 Topic Initialization**

To create your desired topics, **edit the `make init` task directly in the `Makefile`**.

Example (already included):

```makefile
init:
	@echo "📦 Initializing Kafka topics..."
	@echo "Creating topic orders with 3 partitions..."
	@docker exec $(CONTAINER_NAME) /opt/bitnami/kafka/bin/kafka-topics.sh --create \
		--bootstrap-server localhost:$(PORT_BROKER) \
		--topic orders \
		--partitions 3 \
		--replication-factor 1
	# Add more topics as needed
```

---

## **🖥️ Windows-Specific Setup**

On Windows, you need to add the container name to your hosts file:

1. Open **`C:\Windows\System32\drivers\etc\hosts`** as Administrator
2. Add this line:
    
    ```bash
    127.0.0.1 kafka
    ```
    
3. Save the file

This ensures the container can resolve its own hostname properly.
