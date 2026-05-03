# Day 41 - Create Custom Docker Image Using Dockerfile

## Background

The Nautilus application development team required a custom Docker image for testing purposes. The requirement was to define the environment using a Dockerfile instead of manually configuring containers.

---

## Insight

This task introduced the concept of **Dockerfiles**, which allow us to define container environments as code.

Instead of manually installing and configuring services inside a container, we can automate the entire process.

Key idea:

> Dockerfile enables reproducible and consistent environments.

---

## Progress

### 1. Connected to Application Server 2

```bash id="4jxfxg"
ssh steve@stapp02
```

---

### 2. Created directory for Dockerfile

```bash id="6t0x5p"
sudo mkdir -p /opt/docker
```

---

### 3. Created Dockerfile

```bash id="nqnn0u"
sudo vi /opt/docker/Dockerfile
```

---

### 4. Added configuration

```dockerfile id="xk2n5d"
FROM ubuntu:24.04

RUN apt-get update && apt-get install -y apache2

RUN sed -i 's/80/6100/g' /etc/apache2/ports.conf \
 && sed -i 's/*:80/*:6100/g' /etc/apache2/sites-available/000-default.conf

CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
```

---

## Verification

```bash id="r9csqa"
cat /opt/docker/Dockerfile
```

Confirmed:

* correct base image
* Apache installation included
* port changed to 6100
* container runs in foreground

---

## Explanation

The Dockerfile defines a custom image based on Ubuntu 24.04. Apache is installed and configured to listen on port 6100. The container runs Apache in the foreground to ensure it stays active.

---

## What I Learned

* how to create Dockerfiles
* how to automate container setup
* difference between manual configuration and infrastructure as code
* importance of reproducible environments
* how to configure services inside Dockerfile

---

## Real-World Relevance

Dockerfiles are essential in DevOps for:

* building consistent environments
* deploying applications
* integrating with CI/CD pipelines

---

## Improvement / Automation Idea

This Dockerfile can be extended by:

* adding custom application code
* integrating with CI/CD pipelines for automated builds
* versioning images for production deployments

