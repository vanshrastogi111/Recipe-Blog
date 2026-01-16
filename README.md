
---

# Recipe Blog

A secure, containerized Node.js application orchestrated with Kubernetes, automated with GitHub Actions, and monitored with Prometheus & Grafana.

## 🚀 Project Overview

This project demonstrates a complete DevSecOps lifecycle:

* **App:** Node.js (Express.js) with MongoDB.
* **Containerization:** Docker & Docker Compose.
* **Orchestration:** Kubernetes (Self-healing, Scaling).
* **Security:** Trivy Image Scanning (CI phase).
* **Automation:** GitHub Actions for Build & Push.
* **Observability:** Prometheus (Metrics) & Grafana (Visualization).

---

## 🛠️ Prerequisites

Ensure you have the following installed:

1. **Docker Desktop** (With Kubernetes enabled).
2. **Git** (For version control).
3. **Helm** (Package manager for Kubernetes).
* *Windows install:* `winget install Helm.Helm`



---

## 🏗️ Quick Start: Kubernetes Deployment

Follow these steps to run the entire stack locally on Kubernetes.

### 1. Start the Database

The application requires a MongoDB instance running inside the cluster.

```powershell
kubectl apply -f k8s/mongo.yaml

```

### 2. Deploy the Application

Deploy the web server and the service to expose it.

```powershell
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

```

### 3. Access the Application

Since we are using Docker Desktop, the LoadBalancer might take time. Force a direct connection:

```powershell
kubectl port-forward svc/recipe-blog-service 3000:80

```

* **Open in Browser:** [http://localhost:3000](https://www.google.com/search?q=http://localhost:3000)

---

## 🐳 Alternative: Run with Docker Compose

If you want to run the app without Kubernetes (for quick testing), you can use the included Compose file.

```powershell
docker-compose up --build

```

---

## 📊 Monitoring Setup (Prometheus & Grafana)

We use the **Kube-Prometheus-Stack** to gather metrics.

### 1. Install the Stack

```powershell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack

```

### 2. Get Grafana Credentials

Run this command to retrieve the admin password:

```powershell
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(kubectl get secret --namespace default monitoring-grafana -o jsonpath="{.data.admin-password}")))

```

### 3. Access the Dashboard

Open a tunnel to the Grafana dashboard:

```powershell
kubectl port-forward svc/monitoring-grafana 8080:80

```

* **Link:** [http://localhost:8080](https://www.google.com/search?q=http://localhost:8080)
* **User:** `admin`
* **Password:** *(The output from step 2)*

### 4. View App Metrics

1. Go to **Dashboards** (Four squares icon) -> **Dashboards**.
2. Open **Kubernetes / Compute Resources / Namespace (Pods)**.
3. Set **Namespace** to `default`.
4. Set **Pod** to `recipe-blog-deployment-xxxx`.

---

## 🤖 CI/CD Pipeline (GitHub Actions)

This project uses a fully automated pipeline defined in `.github/workflows/devsecops.yml`.

### Workflow Steps:

1. **Checkout Code:** Pulls the latest code from GitHub.
2. **Build:** Creates a Docker image with the Commit SHA tag.
3. **Scan:** Uses **Trivy** to scan for HIGH/CRITICAL vulnerabilities.
4. **Login:** Authenticates with Docker Hub.
5. **Push:** Uploads the image to Docker Hub.

### Secrets Configuration:

To make the pipeline work, set these in **GitHub Repo Settings > Secrets > Actions**:

* `DOCKER_USERNAME`: Your Docker Hub ID.
* `DOCKER_PASSWORD`: Your Docker Hub Access Token.

---

## 📂 Project Structure

```text
.
├── .github/workflows/     # CI/CD Pipeline definitions
│   └── devsecops.yml
├── k8s/                   # Kubernetes Manifests
│   ├── deployment.yaml    # App Deployment (Pods)
│   ├── service.yaml       # Network Access (LoadBalancer)
│   └── mongo.yaml         # Database Deployment & Service
├── MongoDB Data/          # Database seed data (categories.json)
├── public/                # Static assets (CSS, Images, JS)
├── server/                # Database connection & models
├── views/                 # EJS Templates (Frontend)
├── app.js                 # Main Application Entry Point
├── Dockerfile             # Container instructions
├── docker-compose.yml     # Local dev environment
└── README.md              # Project Documentation

```

---

## 🔧 Troubleshooting

* **App Status `ImagePullBackOff`:**
* Ensure your `k8s/deployment.yaml` has `imagePullPolicy: IfNotPresent` if running locally without pushing to Hub.


* **Grafana "No Data":**
* Ensure you selected the **Pods** dashboard, not the Cluster/Node dashboard.


* **Service Pending:**
* Use the `kubectl port-forward` command listed in Step 3 to bypass the LoadBalancer wait time.
