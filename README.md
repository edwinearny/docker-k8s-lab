# docker-k8s-lab

Dockerized and Kubernetes deployment of fastapi-notes-api using minikube.

## What this project covers
- Containerizing a FastAPI app with Docker
- Managing containers with docker-compose
- Deploying to a local Kubernetes cluster using minikube
- Kubernetes Deployments, Services, and ConfigMaps

## Project Structure
docker-k8s-lab/
├── app/                        # FastAPI app (from fastapi-notes-api)
├── k8s/
│   ├── configmap.yml           # Environment variables for Kubernetes
│   ├── deployment.yml          # Tells Kubernetes how to run the app
│   └── service.yml             # Exposes the app on port 30080
├── Dockerfile                  # Builds the app image
├── docker-compose.yml          # Runs the app locally with Docker
└── requirements.txt

## Prerequisites
- Docker Desktop (running)
- minikube
- kubectl

## Setup

```bash
git clone https://github.com/edwinearny/docker-k8s-lab.git
cd docker-k8s-lab
```

---

## Docker

### Commands for image
```bash
docker build -t fastapi-notes-api:latest .
docker run -d -p 8000:8000 --name notes-app fastapi-notes-api:latest
docker stop notes-app
docker rm notes-app
docker start notes-app
docker ps
docker logs notes-app
```
---
## Docker Compose


```bash
docker-compose up -d
docker-compose down
docker-compose up -d --build
docker-compose logs -f
docker-compose ps
```

API docs: http://localhost:8000/docs

---

## Kubernetes (minikube)

### Start minikube
```bash
minikube start --driver=docker
```

### Check minikube status
```bash
minikube status
```

### Build image inside minikube
```bash
minikube image build -t notes-api:latest .
```

### Verify image is loaded
```bash
minikube image ls
```

### Apply all manifests
```bash
kubectl apply -f k8s/configmap.yml
kubectl apply -f k8s/deployment.yml
kubectl apply -f k8s/service.yml
```

### Check deployment
```bash
kubectl get deployments
```

### Check pods
```bash
kubectl get pods
```

### Check services
```bash
kubectl get services
```

### Get app URL
```bash
minikube service notes-service --url
```

Open the URL in your browser with /docs at the end.

### View pod logs
```bash
kubectl logs <pod-name>
```

### Describe a pod (for debugging)
```bash
kubectl describe pod <pod-name>
```

### Delete all resources
```bash
kubectl delete -f k8s/
```

### Stop minikube
```bash
minikube stop
```

---

## How the Kubernetes pieces connect

ConfigMap (notes-config)
└── feeds env vars into → Deployment (notes-deployment)
└── creates Pods labeled → app: notes-app
↑
Service (notes-service) ── finds Pods by label ──────────────────
└── exposes them on port 30080.
The label `app: notes-app` is the glue.
The Deployment stamps it onto every Pod.
The Service uses it to find and route traffic to those Pods.