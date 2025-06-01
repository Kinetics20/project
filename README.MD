
# 🧩 FastAPI Microservice – User & Widget API ![Python](https://img.shields.io/badge/Python-3.13-blue) ![FastAPI](https://img.shields.io/badge/FastAPI-0.115.12-green) ![MongoDB](https://img.shields.io/badge/MongoDB-8.0.9-success) ![Docker](https://img.shields.io/badge/Docker-4.41-blue)

### 🔧 Project Description

A **modular microservice backend** built with **FastAPI**, providing full REST API support for user and `widget` resources. The system is designed with a strong focus on **security**, **scalability**, and **extensibility**, allowing easy integration of future services and features.

---

### 🧱 Tech Stack

- **Python 3.13+**
- **FastAPI 0.115.12** – modern, high-performance web framework
- **MongoDB 8.0.9** – used via `pymongo` (async logic handled manually)
- **Pydantic Settings 2.9.1** – typed and environment-based config
- **Passlib 1.7.4 [bcrypt]** – secure password hashing
- **python-jose 3.4.0** – JWT-based authentication
- **python-dotenv 1.1.0** – .env configuration loader
- **nh3 0.2.21** – HTML sanitizer for content if needed
- **Docker Desktop 4.41** – image-based containerization
- **Kubernetes** – database pod deployment via `mongodb.yml` manifest

---

### 📁 Project Structure

```bash
.
├── .k8s/
│   └── mongodb.yml         # K8s manifest to deploy MongoDB as a pod
├── api/
│   ├── Dockerfile          # Dockerfile for building the API image
│   └── ...                 # FastAPI routers and logic
├── core/                   # Configuration, DB, RBAC, middleware
├── models/                 # Async MongoDB models (ORM logic)
├── schemas/                # Pydantic data schemas
├── main.py                 # FastAPI application entry point
├── config.py               # Environment-based config
└── .env                    # Environment variables
````

---

### 🔐 Key Features

* ✅ User registration and login with hashed passwords
* ✅ JWT-based authentication and role/permission system (RBAC)
* ✅ RESTful endpoints for user and widget resources
* ✅ Role and permission assignment logic
* ✅ `.env`-driven config (MongoDB, JWT, CORS)
* ✅ CORS and rate limiting for secure API access
* ✅ MongoDB deployment via Kubernetes manifest (`.k8s/mongodb.yml`)
* ✅ Docker image creation and deployment using:

```bash
docker buildx build -t fastapi:latest .
docker run -d -p 8001:80 --env-file .env fastapi:latest
```

---

### 🔜 Planned Features

* [ ] Middleware for request logging and traffic analysis
* [ ] Widget system with dedicated permission model
* [ ] File uploads (e.g., user avatars)
* [ ] Unit tests with `pytest` and coverage
* [ ] Full containerization with Docker + Nginx reverse proxy
* [ ] CI/CD pipeline integration (e.g., GitHub Actions)

---

## 💬 Feedback

Contributions and suggestions are welcome!

👤 **Author**: Piotr Lipiński
🗓 **Date**: June 2025
