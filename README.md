# docker-k8s-lab

Dockerized and Kubernetes deployment of fastapi-notes-api using minikube.

## What this project covers
- Containerizing a FastAPI app with Docker
- Managing containers with docker-compose
- Deploying to a local Kubernetes cluster using minikube
- Kubernetes Deployments, Services, and ConfigMaps


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

### Commands for minikube
```bash
minikube start --driver=docker
minikube status
minikube image build -t notes-api:latest .
minikube image ls
kubectl apply -f k8s/configmap.yml
kubectl apply -f k8s/deployment.yml
kubectl apply -f k8s/service.yml
kubectl get deployments
kubectl get pods
kubectl get services
kubectl logs <pod-name>
kubectl describe pod <pod-name>
kubectl delete -f k8s/
minikube stop
```

### Get app URL
```bash
minikube service notes-service --url
```

Open the URL in your browser with /docs at the end.

---
<!-- CI/CD pipeline active -->
