# **🧠 Redis Dev Container**

Single-node Redis container setup with **`Makefile`** for simplified development workflows.

---

## **🔧 Configuration**

| **Variable** | **Default** | **Description** |
| --- | --- | --- |
| **`CONTAINER_NAME`** | **`redis`** | Container name |
| **`IMAGE_NAME`** | **`redis`** | Docker image |
| **`IMAGE_TAG`** | **`7.2-alpine`** | Redis version (alpine for smaller size) |
| **`PORT`** | **`6379`** | Redis server port |
| **`VOLUME_NAME`** | **`redis-data`** | Persistent data volume |
| **`PASSWORD`** | **`redispass`** | Redis auth password |

> ℹ️ Alpine-based image keeps the container lightweight (~40MB)
> 

---

## **📦 Initialization**

The **`init`** target performs basic health check. To add custom initialization:

```makefile
init:
	@echo "📦 Initializing Redis..."
	# Create sample keys
	@docker exec $(CONTAINER_NAME) redis-cli -a $(PASSWORD) SET welcome "Hello from Redis"
	# Create sample list
	@docker exec $(CONTAINER_NAME) redis-cli -a $(PASSWORD) RPUSH colors red green blue
	@echo "✅ Redis initialized with sample data"
```

---

## **🔍 Verification Test**

1. Start the service:
    
    ```bash
    make start
    make init
    ```
    
2. Test basic operations:
    
    ```bash
    # Set value
    docker exec redis redis-cli -a $(PASSWORD) SET test:foo bar
    
    # Get value
    docker exec redis redis-cli -a $(PASSWORD) GET test:foo
    
    # Check persistence
    make stop && make start
    docker exec redis redis-cli -a $(PASSWORD) GET test:foo
    ```
    

---

## **💾 Data Persistence**

All Redis data persists in the Docker volume:

- Volume location: **`/var/lib/docker/volumes/redis-data`**
- Data files: **`dump.rdb`** (RDB snapshot) and **`appendonly.aof`** (if enabled)

To enable AOF persistence (append-only file), add this to **`start`** command:

```
-e REDIS_APPENDONLY=yes \
```