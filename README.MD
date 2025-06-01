
# 🧩 FastAPI Microservice – User & Widget API ![Python](https://img.shields.io/badge/Python-3.13-blue) ![FastAPI](https://img.shields.io/badge/FastAPI-0.115.12-green) ![MongoDB](https://img.shields.io/badge/MongoDB-%E2%9C%85-success) ![Docker](https://img.shields.io/badge/Docker-4.41-blue)

### 🔧 Project Description

A **modular microservice backend** built with **FastAPI**, providing full REST API support for user and `widget` resources. The system is designed with a strong focus on **security**, **scalability**, and **extensibility**, allowing easy integration of future services and features.

---

### 🧱 Tech Stack

- **Python 3.13+**
- **FastAPI 0.115.12** – modern, high-performance web framework
- **MongoDB** – used via `pymongo` (async logic handled manually)
- **Pydantic Settings 2.9.1** – typed and environment-based config
- **Passlib 1.7.4 [bcrypt]** – secure password hashing
- **python-jose 3.4.0** – JWT-based authentication
- **python-dotenv 1.1.0** – .env configuration loader
- **nh3 0.2.21** – HTML sanitizer for content if needed
- **Docker Desktop 4.41** – containerization (planned)
- **Nginx (planned)** – reverse proxy and static file server

---

### 📁 **Project Structure**

```
.
├── api/                 # FastAPI routers (users, auth, widgets)
├── core/                # Configuration, DB, RBAC, middleware
├── models/              # Async MongoDB models (ORM logic)
├── schemas/             # Pydantic data schemas (User, Permission, Role)
├── main.py              # FastAPI application entry point
├── config.py            # Environment-based config (.env: Mongo, JWT, CORS, etc.)
└── Dockerfile + nginx/  # (planned) containerization and reverse proxy setup
```

---

### 🔐 **Key Features**

* ✅ User registration and login
* ✅ Password hashing
* ✅ Read and update user data (GET, PATCH)
* ✅ Roles and permissions (RBAC)
* ✅ Assign permissions to users
* ✅ JWT-based authentication
* ✅ CORS support (Cross-Origin Resource Sharing)
* ✅ Rate limiting (for both authenticated and anonymous users)
* ✅ Comprehensive error handling and HTTP status codes
* ✅ `/me` endpoint for user self-introspection

---

### 🔜 **Planned Features**

* [ ] Middleware for request logging and traffic analysis
* [ ] File uploads (e.g., user avatars)
* [ ] Widget system with dedicated permission model
* [ ] Unit tests with pytest and coverage
* [ ] Full Docker + Nginx container setup
* [ ] CI/CD pipeline (e.g., GitHub Actions)

## 💬 Feedback

Contributions and suggestions are welcome!

👤 **Author**: Piotr Lipiński
🗓 **Date**: June 2025