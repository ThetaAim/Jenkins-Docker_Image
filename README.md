🚀 DevOps CI/CD Pipeline with Jenkins, Docker & AWS

📌 Overview

This project demonstrates a production-like CI/CD pipeline that builds, tests, and deploys a containerized application using Jenkins, Docker, and AWS.

⸻

🏗️ Architecture

Developer → Git → Jenkins (Docker) → Build Image → Push to ECR → EC2 → ALB → Users

Components
	•	Jenkins – runs inside Docker and orchestrates the pipeline
	•	Docker – builds and runs application containers
	•	AWS ECR – stores versioned images
	•	EC2 – hosts the running containers
	•	ALB (Application Load Balancer) – performs health checks and routes traffic

⸻

🔄 Pipeline Flow

1. Build

Jenkins builds a Docker image.

⸻

2. Test

Runs a temporary container inside a custom network:

docker run -d --name test-app --network app-net app-image

Validates service readiness:

curl -sf http://test-app


⸻

3. Deploy

Replaces the running container on EC2.

⸻

🌐 Networking
	•	Custom Docker bridge network: app-net
	•	Enables container-to-container communication via DNS:

curl http://test-app

No reliance on localhost or static IPs

⸻

❤️ Health Checks
	•	ALB continuously verifies application availability:

GET /
User-Agent: ELB-HealthChecker/2.0

	•	Application returns HTTP 200 OK when healthy

⸻

⚠️ Limitations
	•	Deployment currently runs on a single EC2 instance
	•	Short downtime may occur during container replacement

Infrastructure supports scaling to multiple instances, but zero-downtime requires a rolling or blue/green deployment strategy

⸻

🔮 Future Improvements
	•	Multi-instance deployment behind ALB
	•	Rolling / Blue-Green deployment strategy
	•	Kubernetes-based orchestration
	•	Dedicated /health endpoint

⸻

🧠 Key Concepts Demonstrated
	•	Docker container lifecycle
	•	Docker networking (bridge + DNS)
	•	Jenkins running inside Docker with host socket access
	•	CI/CD pipeline design
	•	Health-based validation using curl
	•	Basic production deployment patterns

⸻

💬 Notes
	•	Containers are treated as ephemeral
	•	Each deployment replaces the previous version
	•	Networking is handled through Docker networks instead of host-based access

⸻

🎯 Interview Pitch

Built a CI/CD pipeline using Jenkins inside Docker, deploying containerized applications to AWS EC2 with ALB health checks and Docker-based service networking.
