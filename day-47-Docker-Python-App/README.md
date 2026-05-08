# Day 47 - Dockerize and Deploy a Python Application

## Background

The Nautilus application team needed a Python application to be containerized and deployed on App Server 2. The application dependencies were already provided in a `requirements.txt` file under `/python_app/src`.

---

## Insight

This task demonstrated how to package a Python application into a Docker image.

A Dockerfile defines:

* the base runtime environment
* dependency installation
* application files
* exposed ports
* startup command

Key idea:

> Dockerizing an app makes it portable, repeatable, and easier to deploy.

---

## Progress

### 1. Created Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY src/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY src/ .

EXPOSE 3002

CMD ["python", "server.py"]
```

### 2. Built Docker image

```bash
sudo docker build -t nautilus/python-app /python_app
```

### 3. Created and started container

```bash
sudo docker run -d --name pythonapp_nautilus -p 8095:3002 nautilus/python-app
```

---

## Verification

```bash
sudo docker ps
```

Confirmed:

```text
pythonapp_nautilus
0.0.0.0:8095->3002/tcp
```

Tested the app:

```bash
curl http://localhost:8095/
```

Output:

```text
Welcome to xFusionCorp Industries!
```

---

## Explanation

The Python application was packaged into a Docker image using a Python base image. Dependencies were installed from `requirements.txt`, and the application was started using `server.py`.

The container port `3002` was mapped to host port `8095`, making the application accessible from the host.

---

## What I Learned

* how to write a Dockerfile for a Python app
* how to install dependencies inside an image
* how to expose application ports
* how to run a containerized Python service
* how to test a containerized app with `curl`

---

## Real-World Relevance

Dockerizing applications is a core DevOps skill. It ensures applications run consistently across development, testing, and production environments.

---

## Improvement / Automation Idea

This build and deployment process can be automated using CI/CD pipelines to build images, run tests, and deploy containers automatically.


