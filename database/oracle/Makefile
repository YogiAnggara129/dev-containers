.PHONY: start stop clean init

CONTAINER_NAME = oracle-db
IMAGE_NAME = container-registry.oracle.com/database/free
IMAGE_TAG = latest
PORT = 1521
WEB_PORT = 5500
VOLUME_PATH = /oracle-data
ORACLE_PASSWORD = password
MEMORY_LIMIT = 2g
CPU_LIMIT = 0.5
INIT_SQL = ./init.sql

start:
	@echo "🔍 Checking container status..."
	@if [ "$$(docker ps -q -f name=^/$(CONTAINER_NAME)$$)" ]; then \
		echo "✅ Container '$(CONTAINER_NAME)' is already running"; \
	elif [ "$$(docker ps -aq -f name=^/$(CONTAINER_NAME)$$)" ]; then \
		echo "🔄 Container exists but stopped - Starting..."; \
		docker start $(CONTAINER_NAME); \
		echo "⏳ Oracle takes 3-5 minutes to become available"; \
	else \
		echo "🚀 Creating new Oracle DB container..."; \
		docker run -d \
			--name $(CONTAINER_NAME) \
			-p $(PORT):1521 \
			-p $(WEB_PORT):5500 \
			-e ORACLE_PWD=$(ORACLE_PASSWORD) \
			-v $(VOLUME_PATH):/opt/oracle/oradata \
			-v $$(pwd)/init.sql:/docker-entrypoint-initdb.d/init.sql \
			--memory=$(MEMORY_LIMIT) \
			--cpus=$(CPU_LIMIT) \
			$(IMAGE_NAME):$(IMAGE_TAG); \
		echo "⏳ Initial startup takes 5-10 minutes (including init.sql execution)"; \
		echo "🔍 Monitor logs with: docker logs -f $(CONTAINER_NAME)"; \
	fi

stop:
	@echo "🛑 Stopping container..."
	@docker stop $(CONTAINER_NAME) 2>/dev/null || echo "Container not running"

clean:
	@echo "🧹 Removing container..."
	@docker rm -f $(CONTAINER_NAME) 2>/dev/null || echo "Container does not exist"
	@echo "💾 Note: Volume remains at: $(VOLUME_PATH)"

init:
	@echo "🔄 Checking if init.sql exists..."
	@if [ ! -f "$(INIT_SQL)" ]; then \
		echo "❌ Error: $(INIT_SQL) not found"; \
		echo "   Create a file named init.sql in the current directory"; \
		exit 1; \
	fi
	@echo "🔍 Checking container status..."
	@if [ -z "$$(docker ps -aq -f name=^/$(CONTAINER_NAME)$$)" ]; then \
		echo "❌ Container does not exist - Run 'make start' first"; \
		exit 1; \
	fi
	@if [ -z "$$(docker ps -q -f name=^/$(CONTAINER_NAME)$$)" ]; then \
		echo "🔄 Starting stopped container..."; \
		docker start $(CONTAINER_NAME); \
		sleep 5; \
	fi
	@echo "⏳ Executing init.sql (may take several minutes)..."
	@docker cp $(INIT_SQL) $(CONTAINER_NAME):/tmp/init.sql
	@docker exec $(CONTAINER_NAME) sqlplus -s sys/$(ORACLE_PASSWORD)@//localhost:1521/FREEPDB1 as sysdba @/tmp/init.sql
	@echo "✅ init.sql executed successfully"