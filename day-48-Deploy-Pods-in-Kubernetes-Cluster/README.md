# Day 48 - Create a Kubernetes Pod

## Background

The Nautilus DevOps team started working with Kubernetes for application management. The task was to create a pod running an Apache HTTPD container with specific labels and container naming requirements.

---

## Insight

This task introduced the fundamental Kubernetes object:

> Pod

A pod is the smallest deployable unit in Kubernetes and can contain one or more containers.

Key concepts learned:

* pod creation
* labels
* container naming
* declarative configuration using YAML

---

## Progress

### 1. Connected to Jump Host

```bash
ssh thor@jump-host
```

---

### 2. Generated pod manifest

```bash
kubectl run pod-httpd \
  --image=httpd:latest \
  --labels=app=httpd_app \
  --restart=Never \
  --dry-run=client -o yaml > pod.yaml
```

---

### 3. Updated container name in YAML

Modified:

```yaml
containers:
- image: httpd:latest
  name: httpd-container
```

---

### 4. Created the pod

```bash
kubectl apply -f pod.yaml
```

---

## Verification

```bash
kubectl get pods --show-labels
```

Output:

```text
pod-httpd   Running   app=httpd_app
```

Detailed verification:

```bash
kubectl describe pod pod-httpd
```

Confirmed:

* image: `httpd:latest`
* container name: `httpd-container`

---

## Explanation

A Kubernetes pod running Apache HTTPD was deployed using a YAML manifest. Labels were applied for identification and future service selection.

---

## What I Learned

* how to create Kubernetes pods
* importance of labels
* difference between pod name and container name
* how to use declarative YAML configuration
* how to inspect Kubernetes resources

---

## Real-World Relevance

Pods are the foundation of Kubernetes workloads. Labels are heavily used in:

* service discovery
* monitoring
* scaling
* deployments

---

## Improvement / Automation Idea

This setup can evolve into:

* Deployments for scaling
* Services for networking
* Helm charts for reusable templates

