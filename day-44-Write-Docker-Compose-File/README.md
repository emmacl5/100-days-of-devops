# Day 44 - Deploy Apache Container Using Docker Compose

## Background

The Nautilus application development team needed to deploy a static website using a containerized Apache (httpd) server. The requirement was to use Docker Compose to define and run the container with proper port and volume mappings.

---

## Insight

This task introduced **Docker Compose**, a tool used to define and manage multi-container applications using a YAML file.

Key idea:

> Docker Compose allows you to define infrastructure as code for containers.

It simplifies:

* container creation
* networking
* volume management

---

## Progress

### 1. Connected to Application Server 3

```bash id="5y5n3f"
ssh banner@stapp03
```

---

### 2. Created Docker Compose file

```bash id="qv8wq9"
sudo vi /opt/docker/docker-compose.yml
```

---

### 3. Defined service configuration

```yaml id="w7l4z3"
version: "3"

services:
  web:
    image: httpd:latest
    container_name: httpd
    ports:
      - "3004:80"
    volumes:
      - /opt/itadmin:/usr/local/apache2/htdocs
```

---

### 4. Installed Docker Compose manually

```bash id="8kx9kl"
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

---

### 5. Started container using Compose

```bash id="z2g0dp"
sudo /usr/local/bin/docker-compose up -d
```

---

## Verification

```bash id="4e9tp9"
sudo docker ps
```

Confirmed:

* container name: `httpd`
* port mapping: `3004->80`
* volume mounted correctly

Optional test:

```bash id="8c2ykq"
curl http://localhost:3004
```

---

## Explanation

A Docker Compose file was used to define the Apache container. The container was configured to:

* use the `httpd` image
* expose port 3004
* mount host directory `/opt/itadmin` as the web root

This allowed static content to be served directly from the host system.

---

## What I Learned

* how to use Docker Compose
* how to define containers using YAML
* how to map ports and volumes
* how to manage containers declaratively
* difference between `docker run` and `docker-compose`

---

## Real-World Relevance

Docker Compose is widely used in DevOps for:

* local development environments
* multi-container applications
* simplified deployment workflows

---

## Improvement / Automation Idea

This setup can be extended by:

* adding multiple services (database, backend, frontend)
* integrating with CI/CD pipelines
* migrating to Kubernetes for large-scale deployments

