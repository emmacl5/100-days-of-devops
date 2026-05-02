# Day 39 - Create Docker Image from Running Container

## Background

A Nautilus developer was testing changes inside a running container and wanted to preserve those changes. The DevOps team was tasked with creating a new Docker image from the running container as a backup.

---

## Insight

This task introduced the `docker commit` command.

Unlike building images from a Dockerfile, `docker commit` allows you to:

* capture the current state of a container
* create a reusable image from it

Key idea:

> `docker commit` creates a snapshot of a container at a given point in time.

---

## Progress

### 1. Connected to Application Server 2

```bash
ssh steve@stapp02
```

---

### 2. Verified running container

```bash
sudo docker ps
```

Confirmed container:

```text
ubuntu_latest
```

---

### 3. Created image from container

```bash
sudo docker commit ubuntu_latest media:datacenter
```

---

### 4. Verified new image

```bash
sudo docker images
```

Output showed:

```text
media   datacenter
```

---

## Verification

* Image `media:datacenter` exists
* Image created from running container
* Container state successfully captured

---

## Explanation

The `docker commit` command was used to create a new image from the running container. This captured all changes made inside the container, allowing them to be reused later.

---

## What I Learned

* how to create images from running containers
* difference between `docker commit` and `docker build`
* how to verify Docker images
* how container state can be preserved

---

## Real-World Relevance

This method is useful for:

* debugging environments
* creating quick backups
* capturing experimental changes

However, for production environments, Dockerfiles are preferred for reproducibility.

---

## Improvement / Automation Idea

Instead of manual commits, teams can:

* use Dockerfiles for consistent builds
* automate image creation via CI/CD pipelines

