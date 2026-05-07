# Day 46 - Deploy Multi-Container Stack Using Docker Compose

## Background

The Nautilus application development team completed an application and needed a containerized environment to test it before production deployment. The requirement was to deploy both the application and its database using Docker Compose.

---

## Insight

This task introduced **multi-container architecture** using Docker Compose.

Instead of running containers individually, multiple services were defined and managed together.

Key idea:

> Modern applications are not a single container — they are a system of services working together.

---

## Progress

### 1. Connected to Application Server 3

```bash id="d8y9pa"
ssh banner@stapp03
```

---

### 2. Created Compose file

```bash id="x4nbv1"
sudo vi /opt/devops/docker-compose.yml
```

---

### 3. Defined services

```yaml id="q2k9zv"
version: "3"

services:
  web:
    image: php:apache
    container_name: php_host
    ports:
      - "3001:80"
    volumes:
      - /var/www/html:/var/www/html
    depends_on:
      - db

  db:
    image: mariadb:latest
    container_name: mysql_host
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_host
      MYSQL_USER: app_user
      MYSQL_PASSWORD: StrongPass123!
      MYSQL_ROOT_PASSWORD: RootPass123!
```

---

### 4. Installed Docker Compose

```bash id="1q5o7k"
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

---

### 5. Started the stack

```bash id="j4u7vk"
cd /opt/devops
sudo /usr/local/bin/docker-compose up -d
```

---

## Verification

```bash id="n8a2pb"
sudo docker ps
```

Confirmed:

* `php_host` container running
* `mysql_host` container running

Tested application:

```bash id="z6w4fq"
curl http://localhost:3001
```

---

## Explanation

Docker Compose was used to define and deploy both the web application and database services. The web container uses PHP with Apache, while the database container uses MariaDB. Volumes ensure data persistence, and port mappings allow external access.

---

## What I Learned

* how to deploy multi-container applications
* how services communicate in Docker Compose
* how to configure database environment variables
* how to map volumes for persistence
* how to manage complete application stacks

---

## Real-World Relevance

Most modern applications use:

* frontend service
* backend service
* database service

Docker Compose is widely used to:

* replicate production environments locally
* test integrations
* simplify deployments

---

## Improvement / Automation Idea

This setup can be extended by:

* adding networking configurations
* introducing environment-specific configs
* migrating to Kubernetes for scalability

