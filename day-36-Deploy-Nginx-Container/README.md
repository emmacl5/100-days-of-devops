# Day 36 - Deploy Nginx Container with Docker

## Background

The Nautilus DevOps team needed to test application deployment using containers on Application Server 3. The requirement was to deploy an Nginx container using a lightweight image and ensure it was running successfully.

---

## Insight

This task introduced practical container deployment using Docker.

Containers allow applications to run in isolated environments, making deployments:

* faster
* consistent
* portable

Using the `alpine` tag reduces image size and improves efficiency.

Key idea:

> Containers are lightweight, fast, and ideal for modern application deployment.

---

## Progress

### 1. Connected to Application Server 3

```bash
ssh banner@stapp03
```

---

### 2. Verified Docker service

```bash
sudo systemctl status docker
```

---

### 3. Pulled Nginx Alpine image

```bash
sudo docker pull nginx:alpine
```

---

### 4. Created and started container

```bash
sudo docker run -d --name nginx_3 nginx:alpine
```

---

## Verification

```bash
sudo docker ps
```

Output showed:

* container name: `nginx_3`
* status: `Up`

---

## Explanation

An Nginx container was created using the `nginx:alpine` image. The container was started in detached mode (`-d`) to run in the background. This ensured that the container remained active and available for testing.

---

## What I Learned

* how to pull Docker images
* how to run containers using specific tags
* importance of naming containers
* how to verify container status
* benefits of lightweight images like Alpine

---

## Real-World Relevance

Containers are widely used in DevOps for deploying applications quickly and consistently. Nginx containers are commonly used for:

* web servers
* reverse proxies
* load balancing

---

## Improvement / Automation Idea

This deployment can be automated using Docker Compose or Kubernetes for scaling and orchestration.


