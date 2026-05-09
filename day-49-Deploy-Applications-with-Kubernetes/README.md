# Day 49 - Create a Kubernetes Deployment

## Background

The Nautilus DevOps team continued exploring Kubernetes for application management. The requirement was to deploy an Apache HTTPD application using a Kubernetes Deployment resource instead of a standalone Pod.

---

## Insight

This task introduced one of the most important Kubernetes resources:

> Deployment

Unlike Pods, Deployments provide:

* self-healing
* scaling
* rolling updates
* replica management

Key idea:

> Pods are temporary. Deployments manage Pods reliably in production environments.

---

## Progress

### 1. Connected to Jump Host

```bash id="t0q0x7"
ssh thor@jump-host
```

---

### 2. Created Kubernetes Deployment

```bash id="1o9n5f"
kubectl create deployment httpd --image=httpd:latest
```

---

### 3. Verified deployment

```bash id="4sz8x2"
kubectl get deployments
```

Output:

```text id="9wgm1m"
httpd
```

---

### 4. Verified pod creation

```bash id="7z0v0h"
kubectl get pods
```

Confirmed Kubernetes automatically created Pods for the deployment.

---

### 5. Inspected deployment details

```bash id="7h6aqj"
kubectl describe deployment httpd
```

Verified:

* image: `httpd:latest`
* deployment active and running

---

## Verification

Deployment status:

```bash id="2m5m8r"
kubectl get deployments
```

Pod status:

```bash id="sq9w1v"
kubectl get pods
```

Confirmed:

* deployment running successfully
* pod automatically managed by Kubernetes

---

## Explanation

A Kubernetes Deployment named `httpd` was created using the `httpd:latest` image. The Deployment controller automatically created and managed the application Pod.

This approach provides a more reliable and scalable way to run workloads compared to standalone Pods.

---

## What I Learned

* how to create Kubernetes Deployments
* difference between Pods and Deployments
* how Deployments manage Pods automatically
* how to inspect Kubernetes resources
* importance of declarative orchestration

---

## Real-World Relevance

Deployments are the standard way to run applications in Kubernetes because they support:

* automated recovery
* rolling updates
* horizontal scaling
* high availability

---

## Improvement / Automation Idea

This setup can evolve further with:

* Services for networking
* ConfigMaps and Secrets
* Helm charts
* CI/CD deployment pipelines


