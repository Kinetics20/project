
# 🧩 FastAPI Microservice – User & Widget API ![Python](https://img.shields.io/badge/Python-3.13-blue) ![FastAPI](https://img.shields.io/badge/FastAPI-0.115.12-green) ![MongoDB](https://img.shields.io/badge/MongoDB-8.0.9-success) ![Docker](https://img.shields.io/badge/Docker-4.41-blue)

### 🔧 Project Overview

A **complete microservice API** built with **FastAPI**, providing full REST support for **user** and **widget** resources. It features **JWT authentication**, **role-based access control**, **Redis caching**, and **MongoDB** as the data store. Monitoring is handled via **Prometheus** and **Grafana**. The system is containerized with **Docker** and deployed using **Kubernetes**.

---

### 🧱 Tech Stack

- **Python 3.13+**
- **FastAPI** – high-performance async web framework
- **MongoDB** – data persistence
- **Redis** – in-memory cache store
- **Prometheus + Grafana** – monitoring and visualization
- **Docker Desktop** – image containerization
- **Kubernetes** – container orchestration
- **uv** – Python dependency manager

---

### 🗂 Project Structure

```bash
project/
├── microservice/
│   ├── api/                    # FastAPI logic
│   │   ├── .cert/              # Certificate files (SSL)
│   │   ├── core/               # Configuration, DB, RBAC
│   │   ├── models/             # MongoDB models
│   │   ├── schemas/            # Pydantic schemas
│   │   ├── scripts/            # Grafana dashboard
│   │   ├── utils/              # Utilities (e.g., sanitizer)
│   │   ├── Dockerfile          # API image definition
│   │   └── main.py             # App entry point
│   ├── .k8s/                   # Kubernetes manifests
│   │   ├── mongodb.yml
│   │   ├── redis.yml
│   │   ├── secret.yml
│   │   ├── service.yml
│   │   ├── deployment.yml
│   │   └── prometheus/
└── README.md
````

---

### ✅ Features

* User registration and login (with password hashing)
* JWT-based authentication
* Role-Based Access Control (RBAC)
* Widget CRUD functionality
* Redis-based caching
* MongoDB database backend
* Prometheus + Grafana monitoring
* Kubernetes-based deployment

---

### 🚀 Getting Started

#### 1. Clone the repository

```bash
git clone git@github.com:Kinetics20/project.git
cd project/microservice/api
```

#### 2. Install dependencies with `uv`

```bash
uv sync
```

#### 3. Generate SSL certificates

Navigate to the `.cert` directory and run:

```bash
cd microservice/api/.cert
openssl req -x509 --nodes -days 365 -newkey rsa:2048 -keyout key.pem -out cert.pem
```

These are used for secure authorization.

#### 4. Build Docker image

Go back to the `api` directory and build the Docker image:

```bash
cd ..
docker build -t fast:9 .
```

> If you change the image name (`fast:9`), update it in `.k8s/deployment.yml` under `containers.image`.

---

### ☸️ Kubernetes Deployment

Ensure you have `kubectl` installed:

```bash
sudo snap install kubectl --classic
```

Apply manifests in the `.k8s` directory:

```bash
cd ../.k8s
kubectl apply -f mongodb.yml
kubectl apply -f redis.yml
kubectl apply -f secret.yml
kubectl apply -f service.yml
kubectl apply -f deployment.yml
```

---

### 🔍 Check Deployment Status

```bash
kubectl get pods
```

Example output:

```
NAME                          READY   STATUS    RESTARTS   AGE
mongodb-0                     1/1     Running   ...         ...
redis-xxxxxxx                 1/1     Running   ...         ...
widget-api-xxxxxxx            1/1     Running   ...         ...
```

For monitoring (namespace `monitoring`):

```bash
kubectl get pods -n monitoring
```

---

### 📑 API Documentation

Once running locally, open:

```
http://localhost/docs
```

![Swagger UI](https://github.com/Kinetics20/project/blob/main/microservice/docs/widget_Api.jpg)

---

### 📦 Author

👤 **Piotr Lipiński**
🗓 **Finished**: July 2025
📫 Contributions welcome!
