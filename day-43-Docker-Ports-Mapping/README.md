# Day 43 - Deploy Nginx Container with Port Mapping

## Background

The Nautilus DevOps team needed to deploy an application using an Nginx-based container on Application Server 1. The requirement included pulling a specific image, creating a container, and exposing it through a custom port.

---

## Insight

This task introduced **port mapping**, a critical concept in containerized environments.

Port mapping allows external systems to access services running inside containers.

Key idea:

> Containers are isolated, but port mapping connects them to the outside world.

---

## Progress

### 1. Connected to Application Server 1

```bash id="5o3i3l"
ssh tony@stapp01
```

---

### 2. Pulled required image

```bash id="7v3r0p"
sudo docker pull nginx:alpine-perl
```

---

### 3. Created and started container

```bash id="g3l6y1"
sudo docker run -d \
  --name cluster \
  -p 3003:80 \
  nginx:alpine-perl
```

---

## Verification

```bash id="4g6j7v"
sudo docker ps
```

Output showed:

* container name: `cluster`
* port mapping: `3003->80`
* status: running

Optional test:

```bash id="x9p8m1"
curl http://localhost:3003
```

---

## Explanation

An Nginx container was deployed using the `nginx:alpine-perl` image. The container’s internal port 80 was mapped to host port 3003, allowing access to the service from outside the container.

---

## What I Learned

* how to deploy containers with custom images
* how to map ports between host and container
* how to expose containerized services
* how to verify running containers
* importance of port mapping in deployments

---

## Real-World Relevance

Port mapping is essential in DevOps for:

* exposing web applications
* connecting services
* enabling communication between systems

---

## Improvement / Automation Idea

This setup can be enhanced using:

* Docker Compose for multi-container deployments
* reverse proxies for better routing
* Kubernetes services for scalable exposure

