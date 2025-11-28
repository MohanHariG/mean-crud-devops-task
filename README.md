🚀 MEAN CRUD Application – DevOps Assignment

This project is a complete DevOps implementation of a MEAN (MongoDB, Express, Angular, Node.js) full-stack application.
It includes:

Containerization using Docker & Docker Compose

Deployment on an AWS EC2 Linux machine

MongoDB database (Docker or native)

Nginx reverse proxy

Automated build & deployment using GitHub Actions CI/CD

Docker images pushed to Docker Hub

This README explains the architecture, setup steps, commands used, and screenshots required for submission.

📁 Project Structure
crud-dd-task-mean-app/
│
├── backend/            # Node.js + Express API
├── frontend/           # Angular Application
├── nginx/
│   └── default.conf    # Nginx Reverse Proxy Config
├── docker-compose.yml  # Multi-container Deployment
└── README.md

🏗️ Architecture Overview
User → Nginx (port 80) → Angular Frontend (Docker)
                 ↓
                API → Node Backend (Docker) → MongoDB (Docker)

🐳 Dockerization
✔ Backend Dockerfile

Located in: backend/Dockerfile

✔ Frontend Dockerfile

Located in: frontend/Dockerfile

✔ Nginx Reverse Proxy

Located in: nginx/default.conf

🧩 Nginx Reverse Proxy Configuration

nginx/default.conf:

server {
    listen 80;

    location / {
        proxy_pass http://mean-frontend:80;
    }

    location /api/ {
        proxy_pass http://mean-backend:3000/;
    }
}


This makes the entire app available at:

http://<EC2_PUBLIC_IP>/

🐳 Docker Compose Setup

docker-compose.yml manages all services:

services:
  mongo:
    image: mongo:6
    container_name: mean-mongo
    restart: always
    volumes:
      - mongo-data:/data/db
    ports:
      - "27017:27017"

  backend:
    image: ******/mean-backend:latest
    container_name: mean-backend
    restart: always
    environment:
      - MONGO_URI=mongodb://mongo:27017/mean-crud-db
      - PORT=3000
    depends_on:
      - mongo

  frontend:
    image: *****/mean-frontend:latest
    container_name: mean-frontend
    restart: always
    depends_on:
      - backend

  nginx:
    image: nginx:latest
    container_name: mean-nginx
    restart: always
    ports:
      - "80:80"
    depends_on:
      - frontend
      - backend
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf:ro

volumes:
  mongo-data:

☁️ EC2 Deployment
1️⃣ Spin up an EC2 instance

Instance Type: t3.micro or t3.small

OS: Ubuntu 22.04 LTS

Allow inbound:

SSH (22)

HTTP (80)

2️⃣ Install Docker + Docker Compose
sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker $USER
exit  # Log out and login again
sudo apt install docker-compose-plugin -y

🚀 Deploy Application
cd ~/mean-crud-devops-task/crud-dd-task-mean-app
docker compose pull
docker compose up -d
docker ps


You should see:

mean-nginx
mean-frontend
mean-backend
mean-mongo

🔄 CI/CD Pipeline Using GitHub Actions

GitHub Actions workflow file:
.github/workflows/ci-cd.yml

Workflow does:

✔ Build backend image
✔ Build frontend image
✔ Push to Docker Hub
✔ SSH into EC2
✔ Pull latest images
✔ Restart containers

You must add these secrets:

Secret Name	Value
DOCKERHUB_USERNAME	your DockerHub username
DOCKERHUB_TOKEN	Your Docker access token
SSH_HOST	EC2 Public IP
SSH_USER	ubuntu
SSH_KEY	Content of your .pem file
🖼️ Screenshots Required (Insert Below)
1️⃣ GitHub Repository Structure


2️⃣ Dockerfiles (Backend & Frontend)

(Screenshot)

3️⃣ Nginx Config

(Screenshot)

4️⃣ docker-compose.yml

(Screenshot)

5️⃣ Docker Hub Images

mean-backend:latest

mean-frontend:latest
(Screenshot)

6️⃣ GitHub Actions Workflow

(Screenshot)

7️⃣ GitHub Actions Successful Run

(Screenshot)

8️⃣ EC2 Dashboard

(Screenshot)

9️⃣ SSH into EC2 (Terminal View)

(Screenshot)

🔟 docker ps on EC2

(Screenshot)

1️⃣1️⃣ App Running in Browser

(Screenshot)

🎯 How to Access the App
http://<EC2_PUBLIC_IP>/


Nginx routes:

/ → Angular frontend

/api → Node backend
