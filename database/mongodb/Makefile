.PHONY: start stop clean init

CONTAINER_NAME=mongo
PORT=27018
VOLUME_NAME=mongo-data
INIT_FILE=init.js

start:
	@echo "🚀 Starting MongoDB container..."
	@if [ "$$(docker ps -q -f name=^/$(CONTAINER_NAME)$$)" ]; then \
		echo "🔁 Container '$(CONTAINER_NAME)' is already running."; \
	elif [ "$$(docker ps -a -q -f name=^/$(CONTAINER_NAME)$$)" ]; then \
		echo "▶️ Starting existing container..."; \
		docker start $(CONTAINER_NAME); \
	else \
		echo "🆕 Creating and running container..."; \
		docker run -d \
			--name $(CONTAINER_NAME) \
			-p $(PORT):27017 \
			-v $(VOLUME_NAME):/data/db \
			mongo:7; \
	fi

stop:
	@echo "🛑 Stopping MongoDB container..."
	-docker stop $(CONTAINER_NAME)

clean:
	@echo "🧹 Removing MongoDB container and volume..."
	-docker stop $(CONTAINER_NAME)
	-docker rm $(CONTAINER_NAME)
	-docker volume rm $(VOLUME_NAME)

init:
	@echo "📦 Running initialization script..."
	@docker cp $(INIT_FILE) $(CONTAINER_NAME):/$(INIT_FILE)
	@docker exec -i $(CONTAINER_NAME) mongosh /$(INIT_FILE)
