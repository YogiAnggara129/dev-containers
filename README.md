# 🐳 dev-containers

**Reusable development containers using `docker run` and minimal Makefile interface**

Each folder provides a self-contained environment for local development, independently runnable and easy to copy across projects.

---

## 🧱 Key Concepts

- 📦 **Modular** – One folder = one service (e.g., PostgreSQL, Redis, Kafka)
- 🔁 **Self-contained** – No global Makefile; everything runs independently
- ⚙️ **Minimal & Flexible** – Uses `docker run` and local volumes only
- 🧪 **Copy-paste Ready** – Just copy the folder into any project and use

---

## 📂 Basic Usage

Navigate into the desired service folder (e.g., `postgres/`) and run:

```bash
bash
SalinEdit
make start     # Start the container if not running
make stop      # Stop the container (keeps volumes)
make clean     # Stop and remove container and volumes
make init      # Run optional initialization steps (e.g., create DB or buckets)

```

---

## ⚡ Custom Variables (Optional)

## 🧰 Available Containers

- `postgres/` → PostgreSQL
- `redis/` → Redis
- `minio/` → MinIO
- `kafka/` → Kafka (Redpanda)
- `mongo/` → MongoDB
    
    *More coming soon.*
    

---

Let me know if you want to add badges, usage examples, or a `CONTRIBUTING.md` later!