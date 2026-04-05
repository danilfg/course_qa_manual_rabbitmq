# 🐰 RabbitMQ Cluster with Docker Compose

![RabbitMQ](https://img.shields.io/badge/RabbitMQ-Cluster-orange?style=flat-square\&logo=rabbitmq)
![Docker](https://img.shields.io/badge/Docker-Compose-blue?style=flat-square\&logo=docker)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat-square)

---

# 📜 Description

This repository provides a configuration for deploying a **RabbitMQ cluster using Docker Compose**.

### 🗂 Main Files

* **`docker-compose.yml`**

  * Starts RabbitMQ containers and connects configuration files.

* **`rabbitmq.config`**

  * Configures RabbitMQ settings including:
  * clustering
  * port configuration
  * enabling required plugins

* **`definitions.json`**

  * Creates predefined RabbitMQ structures:
  * queues
  * exchanges
  * users
  * bindings

---

# 💻 Recommended Hardware

Running a RabbitMQ cluster in Docker requires sufficient system resources.

### Minimum requirements

| Resource | Minimum         |
| -------- | --------------- |
| CPU      | 2 cores         |
| RAM      | 2 GB            |
| Disk     | 5 GB free space |

The cluster will run, but performance may be limited under load.

### Recommended configuration

| Resource | Recommended       |
| -------- | ----------------- |
| CPU      | 4 cores           |
| RAM      | 4–8 GB            |
| Disk     | 10+ GB free space |

If you are using **Docker Desktop**, it is recommended to allocate:

```
Memory: 4–6 GB
CPUs: 3–4
```

This ensures that RabbitMQ nodes and the Management UI run smoothly.

---

# 🚀 Quick Start

### 1️⃣ Clone the repository

```bash
git clone https://github.com/danilfg/course_qa_manual_rabbitmq.git
cd course_qa_manual_rabbitmq
```

---

### 2️⃣ Create the Docker network

```
docker network create rabbitmq-cluster
```

---

### 3️⃣ Start the containers

```bash
docker-compose up -d
```

After startup:

* **Port 15672** — RabbitMQ Management UI
* **Port 15671** — AMQP over SSL

---

# 🔧 Working with RabbitMQ

### 1️⃣ Check queue information

To retrieve information about the queue `q.user.created`:

```bash
curl -u guest:guest -X GET \
  http://localhost:15672/api/queues/%2F/q.user.created
```

---

### 2️⃣ Send a message to the queue

To publish a message to `q.user.created`:

```bash
curl -u guest:guest -X POST \
  http://localhost:15672/api/exchanges/%2F/amq.default/publish \
  -H "Content-Type: application/json" \
  -d '{
        "properties": {},
        "routing_key": "q.user.created",
        "payload": "{\"user_id\":123, \"action\":\"created\", \"timestamp\":\"2025-01-05T12:00:00Z\", \"details\":{\"email\":\"user@example.com\", \"name\":\"John Doe\"}}",
        "payload_encoding": "string"
      }'
```

---

### 3️⃣ Check queue again

After sending a message you can check the queue again:

```bash
curl -u guest:guest -X GET \
  http://localhost:15672/api/queues/%2F/q.user.created
```

---

### 4️⃣ Read a message from the queue

To retrieve a message from `q.user.created`:

```bash
curl -u guest:guest -X POST \
  http://localhost:15672/api/queues/%2F/q.user.created/get \
  -H "Content-Type: application/json" \
  -d '{
        "count": 1,
        "ackmode": "ack_requeue_false",
        "encoding": "auto",
        "truncate": 50000
      }'
```

Explanation:

* **count** — number of messages to read
* **ackmode**

  * `ack_requeue_false` → message removed after reading
  * `ack_requeue_true` → message remains in queue
* **truncate** — maximum message size returned

---

# 🖥 RabbitMQ Management UI

After starting the containers, the RabbitMQ web interface is available at:

🌐 [http://localhost:15672](http://localhost:15672)

Credentials:

```
username: guest
password: guest
```

---

# ⚙️ How It Works

### RabbitMQ Initialization

When the container starts, the `definitions.json` file automatically creates:

* queue `q.user.created`
* exchange `amq.default`
* users and permissions

---

### Interacting with RabbitMQ

You can interact with RabbitMQ using:

* the Management UI
* the HTTP API
* automation tools
* testing tools

---

# 💡 Summary

This project provides a ready-to-run RabbitMQ cluster that allows:

* 📦 quick deployment
* 🛠 interaction via API and web interface
* 🌀 easy experimentation with messaging and clustering

It is useful for **learning event-driven architectures and testing message-based systems**.

---

# 👨‍🏫 Author

**Daniil Nikolaev**

QA Automation & DevOps Engineer
Creator of open-source testing platforms
Mentor in software testing and automation
Consulting companies on test automation frameworks and CI/CD infrastructure

Telegram
[https://t.me/danilfg](https://t.me/danilfg)

---

# 🤝 Contributing

If you have suggestions or improvements, feel free to open an **Issue** or submit a **Pull Request**.

![GitHub](https://img.shields.io/badge/GitHub-Contribute-blue?style=flat-square\&logo=github)
