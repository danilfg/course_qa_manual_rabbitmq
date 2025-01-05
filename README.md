# 🐰 RabbitMQ Cluster с Docker Compose

![RabbitMQ](https://img.shields.io/badge/RabbitMQ-Cluster-orange?style=flat-square&logo=rabbitmq)
![Docker](https://img.shields.io/badge/Docker-Compose-blue?style=flat-square&logo=docker)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat-square)

## 📜 Описание

Этот репозиторий предоставляет конфигурацию для развёртывания RabbitMQ-кластера с использованием **Docker Compose**. 

### 🗂 Основные файлы:
- **`docker-compose.yml`**:
  - Запускает контейнеры RabbitMQ и подключает к ним файлы конфигурации.
- **`rabbitmq.config`**:
  - Устанавливает основные настройки RabbitMQ:
    - Кластеризация.
    - Настройка портов.
    - Включение необходимых плагинов.
- **`definitions.json`**:
  - Создаёт предварительно настроенные структуры RabbitMQ:
    - Очереди.
    - Exchange.
    - Пользователи.
    - Связи (bindings).

---

## 🚀 Быстрый старт

### 1️⃣ Склонируйте репозиторий
```bash
git clone https://github.com/danilfg/course_qa_manual_rabbitmq.git
cd course_qa_manual_rabbitmq
```

### 2️⃣ Запустите контейнеры
```bash
docker-compose up -d
```

- После запуска:
  - **Порт 15672**: Веб-интерфейс Management UI.
  - **Порт 15671**: AMQP over SSL (по умолчанию).

---

## 🔧 Взаимодействие с RabbitMQ

### **1. Проверка информации об очереди**

Чтобы получить данные об очереди `q.user.created`:

```bash
curl -u guest:guest -X GET \
  http://localhost:15672/api/queues/%2F/q.user.created
```

---

### **2. Отправка сообщения в очередь**

Для отправки сообщения в очередь `q.user.created` используйте следующий `curl` запрос:

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

### **3. Повторная проверка информации об очереди**

После отправки сообщения снова проверьте состояние очереди:

```bash
curl -u guest:guest -X GET \
  http://localhost:15672/api/queues/%2F/q.user.created
```

---

### **4. Чтение сообщения из очереди**

Чтобы получить сообщение из очереди `q.user.created`, используйте команду:

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

- **`count`**: Количество сообщений для чтения (в данном случае `1`).
- **`ackmode`**:
  - `ack_requeue_false`: Сообщение будет удалено после чтения.
  - `ack_requeue_true`: Сообщение останется в очереди.
- **`truncate`**: Максимальный размер сообщения в байтах.

---

## 🖥 Management UI

После запуска контейнеров веб-интерфейс RabbitMQ доступен по адресу:
- 🌐 [http://localhost:15672](http://localhost:15672)
- 🔑 Логин: `guest`
- 🔑 Пароль: `guest`

---

## ⚙️ Логика работы

1. **Настройка RabbitMQ**:
   - При запуске контейнера используется файл `definitions.json`, который автоматически создаёт структуру:
     - Очередь: `q.user.created`.
     - Exchange: `amq.default`.
     - Пользователи и права доступа.

2. **Взаимодействие с RabbitMQ**:
   - С помощью Management UI или HTTP API вы можете отправлять, получать и управлять сообщениями.

---

## 💡 Заключение

Этот проект предоставляет готовую настройку RabbitMQ-кластера с возможностью:
- 📦 Быстрого развёртывания.
- 🛠 Управления через API и веб-интерфейс.
- 🌀 Лёгкой настройки кластеризации и автоматизации.

---

## 🤝 Контакты
Если у вас есть вопросы или предложения, создайте **Issue** в репозитории! 😊

![GitHub](https://img.shields.io/badge/GitHub-Contribute-blue?style=flat-square&logo=github)
