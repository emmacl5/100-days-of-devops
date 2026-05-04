# Day 42 - Create Custom Docker Network

## Background

The Nautilus DevOps team needed to prepare networking environments for containerized applications. The task was to create a custom Docker network that would be used for future deployments.

---

## Insight

This task introduced **Docker networking**, specifically custom bridge networks.

Docker networks allow containers to:

* communicate with each other
* isolate traffic
* define controlled IP ranges

Key idea:

> Custom networks provide better control and isolation than default networking.

---

## Progress

### 1. Connected to Application Server 2

```bash id="6j9d2q"
ssh steve@stapp02
```

---

### 2. Created custom Docker network

```bash id="i7ndbz"
sudo docker network create \
  --driver bridge \
  --subnet 192.168.0.0/24 \
  --ip-range 192.168.0.0/24 \
  blog
```

---

### 3. Verified network creation

```bash id="rt9dqo"
sudo docker network ls
```

---

### 4. Inspected network configuration

```bash id="q1g9t3"
sudo docker network inspect blog
```

Confirmed:

* driver: bridge
* subnet: 192.168.0.0/24
* ip-range: 192.168.0.0/24

---

## Verification

* Network `blog` exists
* Correct driver (bridge)
* Correct subnet and IP range
* Network is ready for container communication

---

## Explanation

A custom Docker bridge network was created using specific subnet and IP range settings. This allows containers to be deployed within a controlled network environment, improving isolation and communication between services.

---

## What I Learned

* how to create custom Docker networks
* how to define subnet and IP ranges
* importance of bridge driver
* how containers communicate within networks
* how to inspect Docker networks

---

## Real-World Relevance

Custom Docker networks are widely used in DevOps for:

* microservices communication
* service isolation
* managing container networking

---

## Improvement / Automation Idea

This setup can be extended using:

* Docker Compose for multi-container apps
* Kubernetes for advanced networking and orchestration

