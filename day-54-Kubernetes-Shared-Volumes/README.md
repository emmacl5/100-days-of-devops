# Day 54 - Share Volumes Between Containers in a Kubernetes Pod

## Background

The Nautilus DevOps team needed to simulate a multi-container application running inside a single Kubernetes Pod. The requirement was to allow containers to share temporary data using a shared volume.

---

## Insight

This task introduced Kubernetes shared storage using:

> emptyDir volumes

An `emptyDir` volume:

* is created when the Pod starts
* exists as long as the Pod runs
* can be mounted by multiple containers inside the same Pod

This enables containers to share files and temporary data easily.

---

## Progress

### 1. Created Pod Manifest

```yaml id="q9f2wc"
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-datacenter

spec:
  containers:
    - name: volume-container-datacenter-1
      image: ubuntu:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/ecommerce

    - name: volume-container-datacenter-2
      image: ubuntu:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/cluster

  volumes:
    - name: volume-share
      emptyDir: {}
```

---

### 2. Created the Pod

```bash id="n2g4mz"
kubectl apply -f volume-share-datacenter.yaml
```

---

### 3. Verified Pod Status

```bash id="w7m9p1"
kubectl get pod volume-share-datacenter
```

Output:

```text id="v5j3xk"
2/2 Running
```

---

### 4. Created Shared File in Container 1

```bash id="x6t2qf"
kubectl exec volume-share-datacenter \
-c volume-container-datacenter-1 \
-- sh -c 'echo "Welcome to xFusionCorp Industries" > /tmp/ecommerce/ecommerce.txt'
```

---

### 5. Verified Shared Volume in Container 2

```bash id="y4k8dp"
kubectl exec volume-share-datacenter \
-c volume-container-datacenter-2 \
-- cat /tmp/cluster/ecommerce.txt
```

Output:

```text id="m8q7tr"
Welcome to xFusionCorp Industries
```

---

## Explanation

Both containers mounted the same `emptyDir` volume at different filesystem paths. Since the underlying storage was shared, files written by one container became immediately accessible to the other.

---

## What I Learned

* how Kubernetes shared volumes work
* how `emptyDir` volumes behave
* how multiple containers share storage inside a Pod
* how sidecar-style communication works
* how to mount the same volume at different paths

---

## Real-World Relevance

Shared volumes are heavily used in Kubernetes for:

* sidecar containers
* log collection
* caching
* temporary shared application data
* helper containers

---

## Improvement / Automation Idea

This setup could evolve into:

* Persistent Volumes for durable storage
* ConfigMap-mounted volumes
* Secret-mounted volumes
* sidecar logging architectures


