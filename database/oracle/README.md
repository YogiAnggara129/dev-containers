# **🏦 Oracle Database Free Container**

Production-ready Oracle 21c Express Edition container with customizable initialization.

---

## **🔧 Configuration**

| **Variable** | **Default** | **Description** |
| --- | --- | --- |
| **`CONTAINER_NAME`** | **`oracle-db`** | Container name |
| **`IMAGE_NAME`** | **`container-registry.oracle.com/database/free`** | Official Oracle Free image |
| **`PORT`** | **`1521`** | Oracle listener port |
| **`WEB_PORT`** | **`5500`** | Oracle EM Express port |
| **`VOLUME_PATH`** | **`/oracle-data`** | Persistent data location |
| **`ORACLE_PASSWORD`** | **`password`** | SYS/SYSTEM password |
| **`MEMORY_LIMIT`** | **`2g`** | Container memory allocation |
| **`CPU_LIMIT`** | **`0.5`** | CPU cores allocation |

> 💡 Edit these values directly in the Makefile before first run.
>

---

## **💾 Data Persistence**

- **Volume location**:
    
    **`$(VOLUME_PATH)/oradata/`**
    
- **Backup recommendation**:
    
    ```bash
    docker exec oracle-db expdp system/password schemas=APP_ADMIN directory=DATA_PUMP_DIR dumpfile=backup.dmp
    ```

---

## **⚠️ Troubleshooting**

**Issue**: Database not starting

**Solution**:

```bash
# Check memory allocation
docker stats oracle-db

# Increase memory (edit Makefile)
MEMORY_LIMIT = 4g
make clean && make start
```
