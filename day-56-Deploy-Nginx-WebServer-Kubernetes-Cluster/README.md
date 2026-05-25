# Day 56 - Deploy a Scalable Nginx Application with a NodePort Service

## Background

The Nautilus development team needed to deploy a static website on Kubernetes with high availability and scalability. To meet these requirements, a Deployment with multiple replicas and a NodePort Service were created.

---

## Insight

This task introduced a core Kubernetes production architecture:

> Deployments + Services

The Deployment ensures:

* scalability
* self-healing
* high availability

The Service provides:

* stable networking
* external accessibility
* traffic distribution across Pods

---

## Progress

### 1. Created Deployment and Service Manifest

```yaml id="p4x8nk"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx-app

  template:
    metadata:
      labels:
        app: nginx-app

    spec:
      containers:
        - name: nginx-container
          image: nginx:latest

---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service

spec:
  type: NodePort

  selector:
    app: nginx-app

  ports:
    - port: 80
      targetPort: 80
      nodePort: 30011
```

---

### 2. Applied Configuration

```bash id="u7m3qf"
kubectl apply -f nginx-deployment.yaml
```

---

### 3. Verified Deployment

```bash id="r2n5xp"
kubectl get deployments
kubectl get pods
```

Confirmed:

* 3 replicas running successfully

---

### 4. Verified Service

```bash id="h9v6kd"
kubectl get svc nginx-service
```

Confirmed:

* Service type: `NodePort`
* NodePort: `30011`

---

## Explanation

A Kubernetes Deployment named `nginx-deployment` was created using the `nginx:latest` image with 3 replicas for scalability and availability.

A NodePort Service named `nginx-service` exposed the application externally on port `30011`, enabling traffic access to the Nginx Pods.

---

## What I Learned

* how Deployments provide scalability
* how Kubernetes Services expose applications
* how labels/selectors connect Services to Pods
* how NodePort networking works
* how Kubernetes distributes traffic across replicas

---

## Real-World Relevance

This architecture is widely used in production Kubernetes environments because it provides:

* scalability
* redundancy
* service discovery
* traffic balancing
* easier application management

---

## Improvement / Automation Idea

This setup could evolve further using:

* LoadBalancer Services
* Ingress Controllers
* Horizontal Pod Autoscaling (HPA)
* CI/CD deployment pipelines


