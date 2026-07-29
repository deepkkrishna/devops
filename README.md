# DevOps Engineering Assignment

## Deployment of Real-Time WebSocket Application (Docker + NGINX + CI/CD)

## Project Overview

This project demonstrates the deployment of a real-time WebSocket chat application in a production-style environment.

The assignment focused on debugging infrastructure issues, container networking, reverse proxy configuration, cloud deployment, and CI/CD automation rather than backend development.

The application is fully containerized by using Docker and Docker Compose, deployed on an AWS EC2 Ubuntu instance, and automatically redeployed using GitHub Actions on every push to the `main` branch.

---

# Live Application

**Public IP**

```
http://13.200.215.155
```

---

# 🏗️ Architecture Diagram

```
                GitHub Repository
                       │
                       ▼
              GitHub Actions (CI/CD)
                       │
                SSH into AWS EC2
                       │
                git pull origin main
                       │
          docker compose up --build -d
                       │
        ┌──────────────┴──────────────┐
        │                             │
        ▼                             ▼
  NGINX Container             FastAPI Backend
        │                             │
        └──────────────┬──────────────┘
                       │
                  Docker Network
                       │
                    Browser
```

---

#  Technologies Used

- Docker
- Docker Compose
- NGINX
- FastAPI
- WebSockets
- GitHub Actions
- AWS EC2 (Ubuntu)
- Git

---

# Docker Setup

The application consists of two containers:

### Backend Container

- Runs FastAPI using Uvicorn
- Exposes port 8000 internally
- Handles WebSocket connections

### NGINX Container

- Serves frontend files
- Acts as a reverse proxy
- Forwards WebSocket requests to the backend container

Docker Compose manages both containers and creates the required Docker network automatically.

---

# Docker Networking

Both containers communicate through Docker's internal network.

Instead of using:

```
localhost:8000
```

NGINX communicates with the backend using:

```
backend:8000
```

This allows reliable communication between containers.

---

# NGINX Reverse Proxy

NGINX is configured to:

- Serve frontend static files
- Forward WebSocket traffic
- Handle WebSocket upgrade headers
- Route requests to the backend container

---

# WebSocket Communication

The frontend establishes a WebSocket connection through NGINX.

Communication flow:

```
Browser
    │
    ▼
NGINX
    │
    ▼
FastAPI Backend
```

Multiple browser tabs can communicate with each other in real time.

---

# CI/CD Pipeline

Every push to the `main` branch automatically triggers GitHub Actions.

Pipeline flow:

```
Developer Push
        │
        ▼
GitHub Repository
        │
        ▼
GitHub Actions
        │
        ▼
SSH into EC2
        │
        ▼
git pull
        │
        ▼
docker compose up --build -d
        │
        ▼
Application Updated
```

No manual deployment is required.

---

# Issues Identified and Fixed

### Dockerfile

**Issue**

- Backend bound to `127.0.0.1`

**Fix**

- Changed host to `0.0.0.0`

---

### docker-compose.yml

**Issue**

- Frontend volume mapping was commented

**Fix**

- Enabled frontend volume mount

---

### nginx.conf

**Issues**

- Reverse proxy pointed to `localhost`
- Missing WebSocket upgrade headers

**Fixes**

- Changed upstream to `backend:8000`
- Added Upgrade and Connection headers

---

# Deployment Steps

Clone repository

```bash
git clone https://github.com/deepkkrishna/devops.git
cd devops
```

Run application

```bash
docker compose up --build -d
```

Access application

```
http://<EC2-PUBLIC-IP>
```

---

# Repository Structure

```
.
├── backend/
├── frontend/
├── Dockerfile
├── docker-compose.yml
├── nginx.conf
├── README.md
└── .github
    └── workflows
        └── deploy.yml
```

---

# Assignment Objectives Achieved

- Fixed Docker deployment
- Fixed Docker networking
- Configured NGINX Reverse Proxy
- Enabled WebSocket communication
- Deployed on AWS EC2
- Automated deployment using GitHub Actions
- Configured Docker Compose
- Live application accessible via public IP

---

GitHub:
https://github.com/deepkkrishna