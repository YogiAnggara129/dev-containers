# 🪣 MinIO Dev Container

Reusable MinIO container setup with a `Makefile` to simplify common tasks.

---

## 🔧 Configuration

| Variable              | Default         | Description                        |
|-----------------------|------------------|------------------------------------|
| `CONTAINER_NAME`      | `minio`          | Container name                     |
| `IMAGE_NAME`          | `minio/minio`    | Docker image                       |
| `IMAGE_TAG`           | `latest`         | Image tag                          |
| `VOLUME_NAME`         | `minio-data`     | Docker volume name                 |
| `PORT_HTTP`           | `9000`           | MinIO HTTP port                    |
| `PORT_CONSOLE`        | `9001`           | MinIO web console port             |
| `MINIO_ROOT_USER`     | `minio`          | Root username                      |
| `MINIO_ROOT_PASSWORD` | `minio123`       | Root password                      |

---

## 📦 Init Bucket

To create your desired bucket(s), **edit the `make init` task directly in the `Makefile`**.

Example (already included):

```make
init:
	@echo "📦 Creating bucket(s)..."
	# 📝 Edit below to manually define your bucket(s)
	docker exec -i $(CONTAINER_NAME) sh -c "\
		mc alias set local http://localhost:9000 $(MINIO_ROOT_USER) $(MINIO_ROOT_PASSWORD) && \
		mc mb -p local/my-bucket"

---

## 📁 Volume

All MinIO data is stored inside the Docker volume `${VOLUME_NAME}`.