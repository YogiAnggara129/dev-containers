.PHONY: start stop clean

CONTAINER_NAME=kafka
IMAGE_NAME=bitnami/kafka
IMAGE_TAG=3.6.0
PORT_BROKER=9092
PORT_CONTROLLER=9093

start:
	@echo "Checking container status..."
	@if [ "$$(docker ps -q -f name=^/$(CONTAINER_NAME)$$)" ]; then \
		echo "Container '$(CONTAINER_NAME)' is already running."; \
	elif [ "$$(docker ps -a -q -f name=^/$(CONTAINER_NAME)$$)" ]; then \
		echo "Container '$(CONTAINER_NAME)' exists but not running. Starting it..."; \
		docker start $(CONTAINER_NAME); \
	else \
		echo "Container '$(CONTAINER_NAME)' does not exist. Creating and starting it..."; \
		docker run -d --name $(CONTAINER_NAME) --hostname $(CONTAINER_NAME) \
			-p $(PORT_BROKER):$(PORT_BROKER) \
			-p $(PORT_CONTROLLER):$(PORT_CONTROLLER) \
			-e KAFKA_CFG_NODE_ID=0 \
			-e KAFKA_CFG_PROCESS_ROLES=controller,broker \
			-e KAFKA_CFG_LISTENERS=PLAINTEXT://0.0.0.0:$(PORT_BROKER),CONTROLLER://0.0.0.0:$(PORT_CONTROLLER) \
			-e KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://$(CONTAINER_NAME):$(PORT_BROKER),CONTROLLER://$(CONTAINER_NAME):$(PORT_CONTROLLER) \
			-e KAFKA_CFG_LISTENER_SECURITY_PROTOCOL_MAP=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT \
			-e KAFKA_CFG_CONTROLLER_QUORUM_VOTERS=0@$(CONTAINER_NAME):$(PORT_CONTROLLER) \
			-e KAFKA_CFG_CONTROLLER_LISTENER_NAMES=CONTROLLER \
			$(IMAGE_NAME):$(IMAGE_TAG); \
	fi

stop:
	@echo "Stopping container if running..."
	@docker stop $(CONTAINER_NAME) 2>/dev/null || echo "Container is not running."

clean:
	@echo "Removing container if exists..."
	@docker rm -f $(CONTAINER_NAME) 2>/dev/null || echo "Container does not exist."

init:
	@echo "There is no need to initialized"
	# @echo "📦 Initializing Kafka topics..."
	# @echo "Creating topic orders with 3 partitions..."
	# @docker exec $(CONTAINER_NAME) /opt/bitnami/kafka/bin/kafka-topics.sh --create \
	# 	--bootstrap-server localhost:$(PORT_BROKER) \
	# 	--topic orders \
	# 	--partitions 3 \
	# 	--replication-factor 1
	# @echo "Creating topic payments with 2 partitions..."
	# @docker exec $(CONTAINER_NAME) /opt/bitnami/kafka/bin/kafka-topics.sh --create \
	# 	--bootstrap-server localhost:$(PORT_BROKER) \
	# 	--topic payments \
	# 	--partitions 2 \
	# 	--replication-factor 1
	# @echo "Creating topic notifications with 1 partition..."
	# @docker exec $(CONTAINER_NAME) /opt/bitnami/kafka/bin/kafka-topics.sh --create \
	# 	--bootstrap-server localhost:$(PORT_BROKER) \
	# 	--topic notifications \
	# 	--partitions 1 \
	# 	--replication-factor 1
	# @echo "✅ Kafka topics created: orders, payments, notifications"