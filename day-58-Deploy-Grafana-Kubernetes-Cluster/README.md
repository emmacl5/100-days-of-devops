# Day 58 - Deploy Grafana on Kubernetes

## Background

The Nautilus DevOps team planned to deploy Grafana on the Kubernetes cluster to support monitoring and analytics collection for applications.

The objective was to:

* deploy Grafana using Kubernetes
* expose it externally using a NodePort Service
* verify accessibility through the Grafana login page

---

## Insight

This task introduced another important production infrastructure concept:

> Observability Platforms

Grafana is commonly used to:

* visualize metrics
* analyze logs
* build dashboards
* monitor infrastructure and applications

Deploying Grafana on Kubernetes demonstrates how observability tools are integrated into containerized environments.

---

## Progress

### 1. Created Deployment and Service Manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-xfusion

spec:
  replicas: 1

  selector:
    matchLabels:
      app: grafana

  template:
    metadata:
      labels:
        app: grafana

    spec:
      containers:
        - name: grafana-container
          image: grafana/grafana:latest

          ports:
            - containerPort: 3000

---
apiVersion: v1
kind: Service
metadata:
  name: grafana-service

spec:
  type: NodePort

  selector:
    app: grafana

  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
```

---

### 2. Applied Configuration

```bash
kubectl apply -f grafana.yaml
```

---

### 3. Verified Deployment

```bash
kubectl get deployments
kubectl get pods
```

Confirmed:

* Grafana Pod running successfully

---

### 4. Verified Service

```bash
kubectl get svc
```

Confirmed:

* Service type: `NodePort`
* NodePort: `32000`

---

### 5. Accessed Grafana Login Page

Verified that the Grafana login interface was accessible through the NodePort service.

---

## Explanation

A Grafana Deployment was created on Kubernetes using the official Grafana container image. A NodePort Service exposed the application externally on port `32000`, allowing access to the Grafana web interface.

---

## What I Learned

* how to deploy observability tools on Kubernetes
* how NodePort Services expose applications externally
* how Grafana containers run inside Kubernetes
* how labels and selectors connect Deployments and Services
* basics of monitoring platform deployment

---

## Real-World Relevance

Grafana is widely used in production environments for:

* infrastructure monitoring
* Kubernetes cluster monitoring
* application observability
* dashboard visualization
* SRE operations

---

## Improvement / Automation Idea

This setup could be enhanced using:

* Prometheus integration
* Persistent Volumes for dashboard persistence
* Ingress Controllers
* Helm charts
* authentication integration

