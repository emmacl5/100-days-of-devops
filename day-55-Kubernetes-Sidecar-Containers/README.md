# Day 55 - Implement the Kubernetes Sidecar Pattern

## Background

The Nautilus DevOps team needed a solution for handling Nginx access and error logs without using persistent storage. The goal was to share logs with a log aggregation process using a sidecar architecture inside a Kubernetes Pod.

---

## Insight

This task introduced one of the most important Kubernetes design patterns:

> Sidecar Pattern

A sidecar container runs alongside the main application container and performs supporting tasks such as:

* log shipping
* monitoring
* proxying
* configuration synchronization

The application container focuses only on serving traffic, while the sidecar handles operational concerns.

---

## Progress

### 1. Created Shared Volume

Used an `emptyDir` volume named:

```text id="m4q7tk"
shared-logs
```

This volume allows containers inside the Pod to share temporary log files.

---

### 2. Created Pod Manifest

```yaml id="v9k2fd"
apiVersion: v1
kind: Pod
metadata:
  name: webserver

spec:
  volumes:
    - name: shared-logs
      emptyDir: {}

  initContainers:
    - name: sidecar-container
      image: ubuntu:latest
      restartPolicy: Always
      command:
        - sh
        - -c
        - "while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx

  containers:
    - name: nginx-container
      image: nginx:latest
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx
```

---

### 3. Applied Configuration

```bash id="z7m1wr"
kubectl apply -f webserver.yaml
```

---

### 4. Verified Pod Status

```bash id="p3x8nv"
kubectl get pod webserver
```

Output:

```text id="h4q9lm"
webserver   2/2   Running
```

---

### 5. Verified Pod Details

```bash id="r5n0tc"
kubectl describe pod webserver
```

Confirmed:

* `sidecar-container` running with `ubuntu:latest`
* `nginx-container` running with `nginx:latest`
* shared `emptyDir` volume mounted successfully

---

## Explanation

A sidecar logging container was deployed alongside the Nginx web server inside the same Pod. Both containers shared an `emptyDir` volume mounted at `/var/log/nginx`.

The sidecar container continuously read Nginx access and error logs and simulated shipping them to an external logging service.

---

## What I Learned

* how the Kubernetes Sidecar Pattern works
* how containers share volumes inside a Pod
* how initContainers can behave as sidecars
* how shared logging architectures work
* how Kubernetes separates application and operational responsibilities

---

## Real-World Relevance

The sidecar pattern is widely used in production Kubernetes environments for:

* centralized logging
* service meshes
* monitoring agents
* security proxies
* configuration synchronization

---

## Improvement / Automation Idea

This architecture could be extended using:

* Fluentd or Fluent Bit
* Elasticsearch/OpenSearch
* Prometheus exporters
* service mesh sidecars like Envoy

