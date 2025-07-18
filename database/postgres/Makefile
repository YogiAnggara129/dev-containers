CONTAINER_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_VERSION=17-alpine
VOLUME_NAME=postgres-data
PORT=5432

start:
	@if [ "$$(docker ps -aq -f name=$(CONTAINER_NAME))" ]; then \
		echo "Container '$(CONTAINER_NAME)' already exists. Starting it..."; \
		docker start $(CONTAINER_NAME); \
	else \
		echo "Creating and starting new container '$(CONTAINER_NAME)'..."; \
		docker run -d --name $(CONTAINER_NAME) \
			-e POSTGRES_PASSWORD=$(POSTGRES_PASSWORD) \
			-v $(VOLUME_NAME):/var/lib/postgresql/data \
			-p ${PORT}:5432 postgres:$(POSTGRES_VERSION); \
	fi

stop:
	@if [ "$$(docker ps -q -f name=$(CONTAINER_NAME))" ]; then \
		echo "Stopping container '$(CONTAINER_NAME)'..."; \
		docker stop $(CONTAINER_NAME); \
	else \
		echo "Container '$(CONTAINER_NAME)' is not running."; \
	fi

init:
	docker exec -i $(CONTAINER_NAME) psql -U $(POSTGRES_USER) < init.sql

clean:
	@$(MAKE) stop
	@if [ "$$(docker ps -aq -f name=$(CONTAINER_NAME))" ]; then \
		echo "Removing container '$(CONTAINER_NAME)'..."; \
		docker rm $(CONTAINER_NAME); \
	fi
	@if [ "$$(docker volume ls -q -f name=$(VOLUME_NAME))" ]; then \
		echo "Removing volume '$(VOLUME_NAME)'..."; \
		docker volume rm $(VOLUME_NAME); \
	fi
