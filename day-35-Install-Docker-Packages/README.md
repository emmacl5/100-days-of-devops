# Day 35 - Install Docker and Docker Compose

## Background

The Nautilus DevOps team planned to containerize applications as part of modernizing their infrastructure. To begin testing, Docker and Docker Compose needed to be installed and configured on App Server 2.

---

## Insight

This task introduced containerization fundamentals using Docker.

Docker allows applications to run in isolated environments called containers, ensuring consistency across development, testing, and production.

Docker Compose complements Docker by enabling:

* multi-container application management
* simplified service definitions using YAML

A key learning was that not all tools are available via package managers, and sometimes manual installation is required.

---

## Progress

### 1. Connected to App Server 2

```bash
ssh steve@stapp02
```

---

### 2. Installed dependencies

```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

---

### 3. Added Docker repository

```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

---

### 4. Installed Docker CE

```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

---

### 5. Installed Docker Compose manually

```bash
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

---

### 6. Started and enabled Docker service

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

## Verification

```bash
docker --version
docker-compose --version
sudo systemctl status docker
```

Confirmed:

* Docker installed and running
* Docker Compose installed successfully

---

## Explanation

Docker was installed from the official repository, and Docker Compose was installed manually as a binary since it was not available via the package manager.

The Docker service was then started and enabled to ensure it runs automatically on system boot.

---

## What I Learned

* how to install Docker on a Linux server
* how to manually install Docker Compose
* difference between Docker and Docker Compose
* importance of enabling services after installation
* how to verify container runtime setup

---

## Real-World Relevance

Docker is widely used in DevOps for:

* packaging applications
* ensuring consistent environments
* simplifying deployments

Docker Compose is commonly used for local development and multi-container applications.

---

## Improvement / Automation Idea

This setup can be automated using configuration management tools like Ansible or Terraform to standardize container runtime installation across environments.

