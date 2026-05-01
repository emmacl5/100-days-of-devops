# Day 37 - Copy File from Host to Docker Container

## Background

The Nautilus DevOps team needed to securely transfer an encrypted file from the host system into a running container on Application Server 3.

The file:
`/tmp/nautilus.txt.gpg`

The container:
`ubuntu_latest`

The requirement was to copy the file without modifying its contents.

---

## Insight

This task introduced file transfer between a host and a running container.

Docker provides a built-in command:

```bash
docker cp
```

This allows:

* copying files into containers
* copying files out of containers
* preserving file integrity during transfer

Key idea:

> Containers are isolated, but Docker provides controlled ways to interact with them.

---

## Progress

### 1. Connected to Application Server 3

```bash
ssh banner@stapp03
```

---

### 2. Verified container is running

```bash
sudo docker ps
```

Confirmed container:

```text
ubuntu_latest
```

---

### 3. Copied file into container

```bash
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/home/
```

---

### 4. Verified file inside container

```bash
sudo docker exec ubuntu_latest ls -l /home/nautilus.txt.gpg
```

---

### 5. (Optional) Verified file integrity

```bash
sha256sum /tmp/nautilus.txt.gpg
sudo docker exec ubuntu_latest sha256sum /home/nautilus.txt.gpg
```

---

## Verification

* File exists in `/home/` inside container
* File name unchanged
* File content preserved

---

## Explanation

The `docker cp` command was used to transfer the encrypted file from the host into the container. Since the file was copied directly without modification, its integrity was preserved.

---

## What I Learned

* how to copy files into a running container
* how to use `docker cp`
* how to verify file presence inside a container
* how to ensure data integrity during transfer

---

## Real-World Relevance

This technique is widely used in DevOps for:

* transferring configuration files
* injecting secrets into containers
* debugging and troubleshooting container environments

---

## Improvement / Automation Idea

This process can be automated using:

* Docker volumes
* Kubernetes secrets
* CI/CD pipelines for secure file distribution

