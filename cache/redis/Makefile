.PHONY: start stop clean init

CONTAINER_NAME = redis
IMAGE_NAME = redis
IMAGE_TAG = 7.2-alpine
PORT = 6379
VOLUME_NAME = redis-data
PASSWORD = redispass

start:
	@if [ "$$(docker ps -aq -f name=$(CONTAINER_NAME))" ]; then \
		echo "Container '$(CONTAINER_NAME)' already exists. Starting it..."; \
		docker start $(CONTAINER_NAME); \
	else \
		echo "Creating and starting new container '$(CONTAINER_NAME)'..."; \
		docker run -d \
			--name $(CONTAINER_NAME) \
			-p $(PORT):6379 \
			-v $(VOLUME_NAME):/data \
			-e REDIS_PASSWORD=$(PASSWORD) \
			--restart unless-stopped \
			$(IMAGE_NAME):$(IMAGE_TAG)
	fi

stop:
	@echo "🛑 Stopping Redis container..."
	@docker stop $(CONTAINER_NAME) 2>/dev/null || echo "Container already stopped"

clean:
	@echo "🧹 Removing Redis container and volume..."
	@docker rm -f $(CONTAINER_NAME) 2>/dev/null || echo "Container does not exist"
	@docker volume rm $(VOLUME_NAME) 2>/dev/null || echo "Volume does not exist"

init:
	@echo "🔑 Testing Redis connection..."
	@docker exec $(CONTAINER_NAME) redis-cli -a $(PASSWORD) PING | grep -q "PONG" && \
		echo "✅ Redis is ready" || \
		(echo "❌ Redis connection failed"; exit 1)