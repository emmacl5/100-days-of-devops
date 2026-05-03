# Day 40 - Configure Apache Inside a Docker Container

## Background

The Nautilus DevOps team needed to complete pending configuration work on a running container named `kkloud` on Application Server 2. The task involved installing and configuring Apache inside the container to run on a custom port.

---

## Insight

This task demonstrated that containers behave like lightweight servers where services can be installed and configured dynamically.

Key concepts:

* containers can run full services (like Apache)
* configuration inside containers follows the same principles as traditional servers
* minimal containers may lack common tools (like editors)

Important idea:

> Containers are isolated environments, but they can be configured just like real servers.

---

## Progress

### 1. Connected to Application Server 2

```bash id="6w4b9g"
ssh steve@stapp02
```

---

### 2. Accessed the container

```bash id="z2nq4m"
sudo docker exec -it kkloud bash
```

---

### 3. Installed Apache

```bash id="xtk9mq"
apt update
apt install -y apache2
```

---

### 4. Changed Apache listening port

```bash id="iqr2zw"
sed -i 's/Listen 80/Listen 3001/' /etc/apache2/ports.conf
```

---

### 5. Updated virtual host configuration

```bash id="c4rk3m"
sed -i 's/<VirtualHost \*:80>/<VirtualHost *:3001>/' /etc/apache2/sites-available/000-default.conf
```

---

### 6. Started Apache service

```bash id="fb0h8z"
service apache2 start
```

---

### 7. Verified service

```bash id="8y3qv1"
curl http://localhost:3001
```

Output confirmed Apache default page:

```text id="6u6o5r"
Apache2 Ubuntu Default Page - It works
```

---

### 8. Ensured container remains running

```bash id="h36h1k"
sudo docker ps
```

---

## Verification

* Apache installed successfully
* Apache running on port 3001
* Default page accessible
* Container remains active

---

## Explanation

Apache was installed inside the running container and reconfigured to listen on port 3001 instead of the default port 80. Since the container environment is minimal, configuration was done using command-line tools instead of editors.

---

## What I Learned

* how to install services inside containers
* how to modify service configuration
* how to work in minimal container environments
* how to verify services using curl
* how containers behave like lightweight servers

---

## Real-World Relevance

This approach is used when:

* debugging containers
* testing configurations
* running temporary service setups

However, in production, such configurations are typically defined in Dockerfiles or orchestration tools for consistency.

---

## Improvement / Automation Idea

This setup can be improved by:

* creating a Dockerfile with Apache preconfigured
* using Docker Compose or Kubernetes for service management

