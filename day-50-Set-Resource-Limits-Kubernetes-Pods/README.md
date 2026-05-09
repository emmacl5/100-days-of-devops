# Day 50 - Configure Kubernetes Resource Requests and Limits

## Background

The Nautilus DevOps team identified performance issues in Kubernetes-hosted applications caused by uncontrolled resource consumption. To improve cluster stability, resource requests and limits needed to be configured for workloads.

---

## Insight

This task introduced one of the most important Kubernetes production concepts:

> Resource Requests and Limits

Kubernetes allows containers to define:

* minimum guaranteed resources (requests)
* maximum allowed resources (limits)

This prevents:

* resource starvation
* noisy neighbor problems
* unstable cluster performance

---

## Progress

### 1. Connected to Jump Host

```bash id="v0r7l3"
ssh thor@jump-host
```

---

### 2. Created Pod Manifest

```bash id="9g8j4v"
vi httpd-pod.yaml
```

---

### 3. Added Pod Configuration

```yaml id="x7m2kn"
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod

spec:
  containers:
  - name: httpd-container
    image: httpd:latest

    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"

      limits:
        memory: "20Mi"
        cpu: "100m"
```

---

### 4. Created the Pod

```bash id="2j7v8t"
kubectl apply -f httpd-pod.yaml
```

---

## Verification

Checked pod status:

```bash id="k3n4pz"
kubectl get pods
```

Verified resource configuration:

```bash id="0h8w2m"
kubectl describe pod httpd-pod
```

Confirmed:

* Requests:

  * CPU: `100m`
  * Memory: `15Mi`

* Limits:

  * CPU: `100m`
  * Memory: `20Mi`

---

## Explanation

A Kubernetes pod was deployed with resource requests and limits configured. Requests guarantee minimum resources for scheduling, while limits prevent the container from consuming excessive cluster resources.

---

## What I Learned

* how to configure Kubernetes resource requests
* how to configure resource limits
* difference between requests and limits
* importance of resource governance
* how Kubernetes manages workload stability

---

## Real-World Relevance

Resource management is essential in production Kubernetes environments because it:

* improves cluster stability
* prevents outages caused by resource exhaustion
* enables efficient workload scheduling

---

## Improvement / Automation Idea

This setup can be extended using:

* Horizontal Pod Autoscalers (HPA)
* Resource quotas
* Limit ranges
* Monitoring with Prometheus and Grafana


