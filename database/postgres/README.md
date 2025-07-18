# 🐘 PostgreSQL Dev Container

Reusable development container for PostgreSQL using `docker run` and a modular `Makefile`.

---

## 🔧 Configuration

| Variable | Default Value | Description |
| --- | --- | --- |
| `CONTAINER_NAME` | `postgres` | Docker container name |
| `POSTGRES_USER` | `postgres` | Default user |
| `POSTGRES_PASSWORD` | `postgres` | Default password |
| `POSTGRES_VERSION` | `17-alpine` | PostgreSQL image version |
| `VOLUME_NAME` | `postgres-data` | Docker volume for data persistence |
| `PORT` | `5432` | Host port mapped to container |

---

## 🗂️ Optional Initialization

Place an `init.sql` file in this directory to run custom SQL during `make init`.

---

## 📌 Notes

- Data is persisted in Docker volume: `${VOLUME_NAME}`
- Port and other settings can be overridden via environment variables
- This module is designed to work standalone or as part of a larger `dev-containers` setup

---

## 📜 License

MIT