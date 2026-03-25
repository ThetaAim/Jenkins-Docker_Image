# 🚀 DevOps CI/CD Pipeline with Jenkins, Docker & AWS

![Docker](https://img.shields.io/badge/Docker-Containerized-blue?logo=docker)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red?logo=jenkins)
![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws)
![Status](https://img.shields.io/badge/Build-Passing-brightgreen)

---

## Overview
This project demonstrates a **production-like CI/CD pipeline** that builds, tests, and deploys a containerized application using **Jenkins, Docker, and AWS**.

---

## Architecture

```
Developer → Git → Jenkins (Docker) → Build Image → Push to ECR → EC2 → ALB → Users
```

### Components
- **Jenkins** – runs inside Docker and orchestrates the pipeline  
- **Docker** – builds and runs application containers  
- **AWS ECR** – stores versioned images  
- **EC2** – hosts the running containers  
- **ALB (Application Load Balancer)** – performs health checks and routes traffic  

---

## 🔄 Pipeline Flow

### 1. Build
Jenkins builds a Docker image.

### 2. Test
Runs a temporary container inside a custom network:

```bash
docker run -d --name test-app --network app-net app-image
```

Validates service readiness:

```bash
curl -sf http://test-app
```

### 3. Deploy
Replaces the running container on EC2.

---

## Networking

- Custom Docker bridge network: **app-net**
- Container-to-container communication via DNS:

```bash
curl http://test-app
```

> No reliance on localhost or static IPs

---

## Health Checks

- ALB continuously verifies application availability:

```
GET /
User-Agent: ELB-HealthChecker/2.0
```

- Application returns **HTTP 200 OK** when healthy

---

## Limitations

- Deployment runs on a **single EC2 instance**
- Short downtime may occur during container replacement

> Infrastructure supports scaling to multiple instances, but **zero-downtime requires a rolling or blue/green deployment strategy**

---

## Future Improvements

- Multi-instance deployment behind ALB  
- Rolling / Blue-Green deployment  
- Kubernetes-based orchestration  
- Dedicated `/health` endpoint  

---

## Key Concepts Demonstrated

- Docker container lifecycle  
- Docker networking (**bridge + DNS**)  
- Jenkins running inside Docker with host socket access  
- CI/CD pipeline design  
- Health validation using `curl`  
- Basic production deployment patterns  

---

## 💬 Notes

- Containers are **ephemeral**
- Each deployment replaces the previous version
- Networking is handled via Docker networks (not host access)

---

## 🎯 Interview Pitch

> Built a CI/CD pipeline using Jenkins inside Docker, deploying containerized applications to AWS EC2 with ALB health checks and Docker-based service networking.
