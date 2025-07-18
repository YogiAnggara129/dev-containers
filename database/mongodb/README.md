# 🍃 MongoDB Dev Container

Reusable development container for MongoDB using `docker run` and a modular `Makefile`.

---

## 🔧 Configuration

| Variable           | Default Value   | Description                             |
|--------------------|------------------|-----------------------------------------|
| `CONTAINER_NAME`   | `mongo`          | Docker container name                   |
| `MONGO_INITDB_ROOT_USERNAME` | `admin`     | MongoDB root username                   |
| `MONGO_INITDB_ROOT_PASSWORD` | `admin`     | MongoDB root password                   |
| `MONGO_VERSION`    | `7`              | MongoDB image version                   |
| `VOLUME_NAME`      | `mongo-data`     | Docker volume for data persistence      |
| `PORT`             | `27018`          | Host port mapped to container (27017)   |

---

## 🗂️ Optional Initialization

Place an `init.js` file in this directory to run custom JavaScript during `make init`.  
This can be used to create databases, collections, or seed initial data.

Example content for `init.js`:
```js
db = db.getSiblingDB("mydb");
db.createCollection("users");
db.users.insert({ name: "admin", role: "root" });
