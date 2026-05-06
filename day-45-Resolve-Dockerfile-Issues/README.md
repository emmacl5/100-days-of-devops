# Day 45 - Fix Dockerfile Build Issues

## Background

The Nautilus DevOps team was creating a custom Docker image on App Server 1. A Dockerfile already existed under `/opt/docker`, but the image build was failing due to configuration issues.

The task was to fix the Dockerfile without changing:

* the base image
* valid existing configuration
* application data such as `index.html`

---

## Insight

This task reinforced an important Docker principle:

> A Dockerfile must match the filesystem structure of the base image.

The base image was:

```dockerfile
FROM httpd:2.4.43
```

In the official Apache HTTPD image, the main configuration file is located at:

```text
/usr/local/apache2/conf/httpd.conf
```

The original Dockerfile referenced incorrect paths such as:

```text
/usr/local/apache2/conf.d/httpd.conf
conf.d/httpd.conf
```

Those paths caused the build to fail.

---

## Progress

### 1. Connected to App Server 1

```bash
ssh tony@stapp01
```

### 2. Navigated to Dockerfile directory

```bash
cd /opt/docker
```

### 3. Reviewed the Dockerfile

```bash
cat Dockerfile
```

### 4. Fixed invalid Apache configuration paths

Corrected all `sed` commands to target:

```text
/usr/local/apache2/conf/httpd.conf
```

### 5. Built the Docker image

```bash
sudo docker build -t test-image .
```

---

## Corrected Dockerfile

```dockerfile
FROM httpd:2.4.43

RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule ssl_module modules\/mod_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' /usr/local/apache2/conf/httpd.conf

RUN sed -i '/Include conf\/extra\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf

COPY certs/server.crt /usr/local/apache2/conf/server.crt

COPY certs/server.key /usr/local/apache2/conf/server.key

COPY html/index.html /usr/local/apache2/htdocs/
```

---

## Verification

```bash
sudo docker build -t test-image .
```

Result:

```text
FINISHED
naming to docker.io/library/test-image
```

The image built successfully.

---

## Explanation

The Dockerfile failed because it attempted to modify Apache configuration files using incorrect paths. After updating the paths to match the official `httpd:2.4.43` image structure, the build completed successfully.

---

## What I Learned

* how to troubleshoot Dockerfile build failures
* how base image filesystem structure affects Dockerfile commands
* importance of reading build errors carefully
* how to fix configuration paths without changing valid requirements
* how to validate Docker image builds

---

## Real-World Relevance

Dockerfile troubleshooting is a common DevOps task. Understanding how base images are structured helps engineers write reliable, reproducible builds and fix failed CI/CD pipelines.

---

## Improvement / Automation Idea

This process can be improved by:

* linting Dockerfiles before build
* documenting base image paths
* adding CI checks to validate image builds automatically


