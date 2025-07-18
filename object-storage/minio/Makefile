.PHONY: start stop clean init

CONTAINER_NAME=minio
IMAGE_NAME=minio/minio
IMAGE_TAG=latest
VOLUME_NAME=minio-data
PORT_HTTP=9000
PORT_CONSOLE=9001
MINIO_ROOT_USER=minio
MINIO_ROOT_PASSWORD=minio123

start:
	@echo "🚀 Starting MinIO container..."
	docker run -d \
		--name $(CONTAINER_NAME) \
		-p $(PORT_HTTP):9000 \
		-p $(PORT_CONSOLE):9001 \
		-e MINIO_ROOT_USER=$(MINIO_ROOT_USER) \
		-e MINIO_ROOT_PASSWORD=$(MINIO_ROOT_PASSWORD) \
		-v $(VOLUME_NAME):/data \
		--restart unless-stopped \
		$(IMAGE_NAME):$(IMAGE_TAG) server /data --console-address ":9001"

stop:
	@echo "🛑 Stopping and removing MinIO container..."
	docker stop $(CONTAINER_NAME) && docker rm $(CONTAINER_NAME)

clean:
	@echo "🧹 Removing volume..."
	docker volume rm $(VOLUME_NAME)

init:
	@echo "📦 Creating bucket(s)..."
	# 📝 Edit below to manually define your bucket(s)
	docker exec -i $(CONTAINER_NAME) sh -c "\
		mc alias set local http://localhost:9000 $(MINIO_ROOT_USER) $(MINIO_ROOT_PASSWORD) && \
		mc mb -p local/my-bucket"
