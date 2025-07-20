![RoboShop Architecture](https://raw.githubusercontent.com/Nallagachu/roboshop-docker/main/images/docker8.png)


# 🛒 RoboShop-Docker: Containerized E-commerce Website

**RoboShop-Docker** is a Dockerized version of the **RoboShop e-commerce website**, built to demonstrate how Docker and Docker Compose simplify the deployment of complex, microservices-based platforms.

This project is part of a DevOps learning journey, showcasing how containerization enables fast, repeatable, and optimized environments — serving as a solid foundation for transitioning into Kubernetes and production-grade CI/CD pipelines.

---

## 🚀 Project Overview

* 🔧 **Provision EC2 using Terraform**
* 🐳 **Run multiple services using Docker & Docker Compose**
* 📦 **Images are pulled from [Docker Hub](https://hub.docker.com/u/durgababu-docker-567)**
* ⚡ **Single-command deployment with `docker-compose up -d`**
* 🧩 **Includes databases, messaging queues, microservices, and frontend**
* 🧑‍💻 **Non-root Docker access via a custom EC2 user (`harga`)**

---

## 🧠 Tech Stack

| Category          | Tools / Services                                            |
| ----------------- | ----------------------------------------------------------- |
| **Infra Setup**   | Terraform, AWS EC2                                          |
| **Containers**    | Docker, Docker Compose                                      |
| **Microservices** | Node.js, Python, Java                                       |
| **Databases**     | MongoDB, Redis                                              |
| **Messaging**     | RabbitMQ                                                    |
| **Frontend**      | Nginx (Static HTML hosting)                                 |
| **Registry**      | [Docker Hub](https://hub.docker.com/u/durgababu-docker-567) |

---

## 🎯 Project Purpose

> This project shows how Docker enables fast and consistent environments for running real-world applications like e-commerce websites.

* ✅ Simplify setup with Docker Compose
* 🔄 Enable reusable image-based deployments
* 💡 Optimize images with multi-stage builds
* ❌ Avoid root-based Docker use (security best practice)
* ⚙️ Automate EC2 setup with Docker pre-installed
* 🔐 Practice foundational DevOps skills for production systems

---

## 🧑‍💻 Prerequisites

Make sure you have the following tools installed:

* AWS CLI (configured with access keys)
* Terraform
* Docker + Docker Compose
* Git
* Docker Hub account (optional if you want to push your own images)

---

## ⚙️ Infrastructure Provisioning (Terraform)

This repo includes a Terraform module to provision an EC2 instance with:

* Docker pre-installed
* A user named `harga` with permission to run Docker without `sudo`
* EBS volume extended to **50GB**

### Provision with:

```bash
terraform init
terraform apply -auto-approve
```

### Connect via SSH:

```bash
ssh harga@<EC2-Public-IP>
```

---

## 📦 Deploy Microservices (Docker Compose)

### Step 1: Clone the Repository

```bash
git clone https://github.com/Nallagachu/roboshop-docker.git
cd roboshop-docker
```

### Step 2: Start All Containers

```bash
docker-compose up -d
```

### Step 3: Verify Deployment

```bash
docker ps
```

Access the website:

```bash
http://<EC2-Public-IP>
```

> You can also test APIs using `curl` commands or inspect service logs for validation.

---

## 🔢 How to Run This Project on EC2

Once your EC2 instance is up and running, follow these steps to deploy the RoboShop e-commerce website using Docker Compose.

### 🖥️ Step-by-step on EC2 Terminal:

1. **SSH into the EC2 Instance**

```bash
ssh harga@<EC2-Public-IP>
```

2. **Clone the GitHub Repo**

```bash
git clone https://github.com/Nallagachu/roboshop-docker.git
cd roboshop-docker
```

3. **Explore the Project**
   Inside the folder, you'll find a `docker-compose.yml` file. This file defines how all services work together and where to fetch the images.

4. **Deploy the Stack**

```bash
docker-compose up -d
```

> This command will automatically pull the latest versions of all service images from Docker Hub (`durgababu-docker-567`) and spin them up in the correct order.

### 🔄 Alternative Options:

* Build individual services manually (for development or debugging):

```bash
docker build -t durgababu-docker-567/<service>:latest .
docker-compose up -d
```

* If you've made code changes, rebuild the image before running:

```bash
docker-compose build
docker-compose up -d
```

---

## 🧩 Microservices Included

| Service  | Tech      | Description                              |
| -------- | --------- | ---------------------------------------- |
| MongoDB  | Database  | Stores user/catalog data                 |
| Redis    | In-Memory | Session & cache storage                  |
| RabbitMQ | Messaging | Asynchronous service communication       |
| User     | Node.js   | User management API                      |
| Cart     | Node.js   | Shopping cart service                    |
| Catalog  | Node.js   | Product listing/catalog service          |
| Shipping | Java      | Shipping and logistics API               |
| Payment  | Python    | Payment gateway simulation               |
| Frontend | Nginx     | Web interface for the e-commerce website |

---

## 📤 Building and Pushing Docker Images

You can also build and push updated images to your Docker Hub account:

```bash
docker build -t durgababu-docker-567/catalog:latest ./catalog
docker push durgababu-docker-567/catalog:latest
```

Repeat for each microservice you customize.

---

## 💰 Cost Optimization: Cleanup EC2

Once you’re done testing, destroy the infrastructure to avoid charges:

```bash
terraform destroy -auto-approve
```

---

## 📸 Image Portfolio (To Be Added)

You'll find the following in the final version:

* 🌐 Architecture diagram: EC2 + Docker + Microservices
* 🔄 Docker Compose dependency flow
* 📷 Screenshots of services running and logs

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 🙏 Credits & Inspiration

* [Docker](https://www.docker.com/)
* [Terraform](https://www.terraform.io/)
* [AWS EC2](https://aws.amazon.com/ec2/)
* DevOps community & hands-on learning path

---

## 🧭 Workflow & Service Dependencies (Docker Compose)

Docker Compose manages the full microservices stack by reading the `docker-compose.yml` file, which defines each service, its image, environment variables, networks, and most importantly — **dependencies**.

### 🔗 How it works:

* Services like `cart`, `user`, or `payment` **depend on backend services** such as `MongoDB`, `Redis`, and `RabbitMQ`.
* Compose ensures that **linked services are available** before dependent services attempt to start.
* This reduces manual configuration and allows the platform to spin up **in a correct, repeatable order**.

You can view and understand the service graph using:

```bash
docker-compose config
```

---

## 🚀 Docker Optimization Techniques Used

This repository is designed with best practices to help you build efficient, production-ready containers — making it a great learning base before advancing to orchestration tools like Kubernetes or exploring automation tools like DALL·E or GitHub Copilot.

### Key Techniques Applied:

* ✅ **Multi-Stage Builds**
  Separate build and runtime layers to reduce final image size.

* ✅ **Volume Mounting**
  Use of Docker volumes for persistent storage (where applicable).

* ✅ **Layer Caching**
  Dockerfile instructions ordered strategically to leverage layer caching and avoid unnecessary rebuilds.

* ✅ **Lean Base Images**
  Usage of minimal images (like `node:alpine`, `python:slim`) to keep containers lightweight and fast.

* ✅ **Non-root Docker Access**
  EC2 is configured to allow the `harga` user to run Docker commands without `sudo`.

* ✅ **Centralized Image Hosting**
  Images are versioned and stored in [Docker Hub](https://hub.docker.com/u/durgababu-docker-567), making reuse and testing easier.

> 💡 Reviewing this setup gives you practical insights into Docker image optimization — a critical DevOps and containerization skill.
