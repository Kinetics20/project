
# 🧩 FastAPI Microservice – User & Widget API ![Python](https://img.shields.io/badge/Python-3.13-blue) ![FastAPI](https://img.shields.io/badge/FastAPI-0.115.12-green) ![MongoDB](https://img.shields.io/badge/MongoDB-8.0.9-success) ![Docker](https://img.shields.io/badge/Docker-4.41-blue)

### 🔧 Project Overview

A **complete microservice API** built with **FastAPI**, providing full REST support for **user** and **widget** resources. It features **JWT authentication**, **role-based access control**, **Redis caching**, and **MongoDB** as the data store. Monitoring is handled via **Prometheus** and **Grafana**. The system is containerized with **Docker** and deployed using **Kubernetes**.

![Grafana_dashboard](https://github.com/Kinetics20/project/blob/main/microservice/docs/grafana_dashboard.jpg)

![Prometheus_target_health](https://github.com/Kinetics20/project/blob/main/microservice/docs/prometheus.jpg)

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

Once the application is running, you can access the interactive documentation at:

```
http://localhost/docs
```


![Swagger UI](https://github.com/Kinetics20/project/blob/main/microservice/docs/widget_Api.jpg)

---

### 🔐 Authentication Workflow (Required to Use the API)

To successfully use all CRUD operations on users and widgets, follow these steps:

1. **Create an admin user**  
   - Go to the `users` section → `POST /users/`
   - Provide user details and set the `status` field to `admin`
   - Example payload:
     ```json
     {
       "username": "adminuser",
       "password": "yourpassword",
       "status": "admin"
     }
     ```

2. **Authorize the session**  
   - Click the **Authorize** button in the top right of the Swagger UI
   - Enter the JWT token you received after logging in as the admin user

3. **Start making authenticated requests**  
   - After successful authorization, you can now:
     - Create, read, update, and delete users
     - Perform all widget operations (CRUD)
     - Access endpoints protected by authentication

> ⚠️ **Note**: If the user is not created with `"status": "admin"`, access to certain endpoints will be restricted.

---

### 📊 Monitoring with Prometheus and Grafana

This project includes a full monitoring setup using **Prometheus** and **Grafana**, deployed via **Helm**.

---

### 🔧 1. Install Helm

First, install Helm on your system:

```bash
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
````

---

### 📦 2. Add Helm Repositories

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

---

### 🧠 3. Create Monitoring Namespace

```bash
kubectl create namespace monitoring
```

---

### 📈 4. Install Prometheus

```bash
helm install prometheus prometheus-community/prometheus \
  --namespace monitoring \
  --set alertmanager.persistentVolume.storageClass=hostpath \
  --set server.persistentVolume.storageClass=hostpath \
  --values - <<EOF
server:
  additionalScrapeConfigs:
    - job_name: 'widget-api'
      static_configs:
        - targets: ['widget-api:80']
EOF
```

This configures Prometheus to scrape metrics from the `widget-api` container.

---

### 📉 5. Install Grafana

```bash
helm install grafana grafana/grafana \
  --namespace monitoring \
  --set persistence.storageClassName=hostpath \
  --set persistence.enabled=true \
  --set adminPassword='admin' \
  --values - <<EOF
datasources:
  datasources.yaml:
    apiVersion: 1
    datasources:
    - name: Prometheus
      type: prometheus
      url: http://prometheus-server.monitoring.svc.cluster.local
      access: proxy
      isDefault: true
EOF
```

Login credentials:

* **Username**: `admin`
* **Password**: `admin`

---

### 🔌 6. Port Forwarding

Forward ports locally to access both dashboards in your browser:

```bash
kubectl port-forward -n monitoring svc/grafana 3000:80
```

Grafana will be available at:

```
http://localhost:3000
```

```bash
kubectl port-forward -n monitoring svc/prometheus-server 3001:80
```

Prometheus will be available at:

```
http://localhost:3001
```

---

### 🧾 7. Import Dashboard in Grafana

To visualize metrics from the `widget-api`, import the preconfigured dashboard:

1. Open Grafana → **Dashboards** → **Create** → **Import**
2. Use the file located at:

   ```
   microservice/api/scripts/grafana-dashboard.json
   ```
3. You can upload the file or paste the raw JSON into the text editor.

> ✅ The dashboard will show live metrics from Prometheus such as pod health, API performance, and request frequency.


---
### 📦 Author

👤 **Piotr Lipiński**
🗓 **Finished**: July 2025
📫 Contributions welcome!
