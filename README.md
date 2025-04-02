# KampusReport Deployment Guide

KampusReport is a cloud-native academic research management platform designed to streamline research submission, assessment, and collaboration for students and supervisors. This guide details the steps required to deploy KampusReport using Kubernetes.

## 🚀 Features
- **Plagiarism detection & versioning**
- **Supervisor feedback workflows**
- **SIWES tracking & analytics**
- **Real-time notifications**
- **Institution-wide reporting dashboards**

---

## 🏗 Architecture Overview

KampusReport is deployed in a **Kubernetes** cluster using multiple components:
- **Laravel Application** (PHP backend)
- **Nginx Proxy** (Web server & load balancing)
- **Mysql Database** (Persistent storage)
- **Redis** (Queue & caching system)
- **MinIO/S3** (File storage for research documents)

---

## 🔧 Prerequisites
Ensure you have the following installed:
- [Docker](https://docs.docker.com/get-docker/)
- [Kubernetes](https://kubernetes.io/docs/setup/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Helm](https://helm.sh/docs/intro/install/)

---

## 📦 Deployment Steps

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/wabsence/kampusreport-k8s.git
cd kampusreport-k8s
```

### 2️⃣ Configure Environment Variables
Create a `.env` file from the example template:
```bash
cp .env.example .env
```
Edit the `.env` file with appropriate values, including database credentials, Redis, and S3 configurations.

### 3️⃣ Deploy Mysql & Redis
```bash
kubectl apply -f kampusreport-k8s/db-kampusreport.yaml
kubectl apply -f k8s/redis.yaml
```

### 4️⃣ Deploy Laravel Application
```bash
kubectl apply -f kampusreport-k8s/app-kampusreport.yaml
```
This deploys the Laravel backend with **Init Containers** that handle:
- Code bootstrapping
- Directory structuring
- Application environment setup

### 5️⃣ Deploy Nginx
```bash
kubectl apply -f kampusreport-k8s/nginx.yaml
```
Nginx is configured for **handling large research document uploads** and optimized **buffer settings**.

### 6️⃣ Run Database Migrations
```bash
kubectl apply -f kampusreport-k8s/app-kampusreport-migrationsjob.yaml
```
This job automatically applies schema updates and seeds necessary data.

### 7️⃣ Expose Services
```bash
kubectl apply -f kampusreport-k8s/app-kampusreport-ingress.yaml
```
This enables access to the application via an **Ingress Controller**.

### 8️⃣ Verify Deployment
```bash
kubectl get pods
kubectl get services
```
Ensure all services are running correctly.

---

## 🌍 Accessing the Application
Once deployed, visit the assigned domain or IP:
```bash
http://kampusreport.com
```
Login with provided credentials or create an account.

---

## 🔄 Scaling & Maintenance
To **scale the Laravel application**, run:
```bash
kubectl scale deployment laravel-app --replicas=3
```
To **restart a service**:
```bash
kubectl rollout restart deployment/laravel-app
```

---

## 📌 Next Steps
- **Migrate to AWS EKS** for improved scalability
- **Integrate Amazon RDS** for managed database performance
- **Optimize auto-scaling policies** for peak loads

For more details, visit: [kampusreport.zeronespace.com](https://kampusreport.zeronspace.com/)

---

## 🤝 Contributing
Pull requests are welcome! For major changes, please open an issue first to discuss improvements.

Happy deploying! 🚀
