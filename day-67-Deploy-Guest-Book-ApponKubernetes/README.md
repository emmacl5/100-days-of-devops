# Day 67 - Deploying a Multi-Tier Guestbook Application on Kubernetes

## Background

The Nautilus Application Development team completed a Guestbook application and requested its deployment on the Kubernetes cluster.

The architecture consisted of:

* Frontend Tier (PHP Guestbook)
* Redis Master
* Redis Slave Replicas
* Internal Services
* External Access via NodePort

This deployment demonstrates a classic multi-tier application architecture commonly found in production environments.

---

## Objective

Deploy a Guestbook application stack consisting of:

### Backend Tier

#### Redis Master

* Deployment Name: `redis-master`
* Replicas: `1`
* Container: `master-redis-xfusion`
* Image: `redis`
* CPU Request: `100m`
* Memory Request: `100Mi`
* Port: `6379`

Service:

* Name: `redis-master`
* Port: `6379`

---

#### Redis Slave

* Deployment Name: `redis-slave`
* Replicas: `2`
* Container: `slave-redis-xfusion`
* Image:

```text id="sl5x2p"
gcr.io/google_samples/gb-redisslave:v3
```

Resources:

```text id="8jlwm8"
CPU: 100m
Memory: 100Mi
```

Environment Variable:

```text id="spynia"
GET_HOSTS_FROM=dns
```

Port:

```text id="hd4tzn"
6379
```

Services:

* `redis-slave`
* `redis-follower`

---

### Frontend Tier

Deployment:

* Name: `frontend`
* Replicas: `3`
* Container: `php-redis-xfusion`

Image:

```text id="xh67w0"
gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
```

Resources:

```text id="l4v4n3"
CPU: 100m
Memory: 100Mi
```

Environment Variable:

```text id="2dkqnl"
GET_HOSTS_FROM=dns
```

Port:

```text id="gfslbi"
80
```

---

### Frontend Service

* Name: `frontend`
* Type: `NodePort`
* Port: `80`
* NodePort: `30009`

---

## Solution

### Redis Master Deployment

```yaml id="5thx2j"
kind: Deployment
metadata:
  name: redis-master
```

Container:

```yaml id="d5kj4k"
image: redis
```

Resources:

```yaml id="6k0s7z"
resources:
  requests:
    cpu: "100m"
    memory: "100Mi"
```

---

### Redis Slave Deployment

```yaml id="4g4sqe"
kind: Deployment
metadata:
  name: redis-slave
```

Environment Variable:

```yaml id="xz0j4a"
env:
- name: GET_HOSTS_FROM
  value: dns
```

Replicas:

```yaml id="5mvlc5"
replicas: 2
```

---

### Frontend Deployment

```yaml id="x8j5r0"
kind: Deployment
metadata:
  name: frontend
```

Replicas:

```yaml id="vfgw3v"
replicas: 3
```

Container:

```yaml id="s6i86z"
name: php-redis-xfusion
```

---

### Frontend Service

```yaml id="i4yczz"
type: NodePort
nodePort: 30009
```

This service exposes the application externally.

---

## Verification

Verify deployments:

```bash id="s2tr8m"
kubectl get deployments
```

Verify pods:

```bash id="1jlwm1"
kubectl get pods
```

Verify services:

```bash id="umjpzu"
kubectl get svc
```

Expected:

```text id="nb0nci"
redis-master    1/1
redis-slave     2/2
frontend        3/3
```

Frontend Service:

```text id="s9cbjw"
NodePort: 30009
```

---

## Architecture

```text id="80q6mo"
                    Internet
                        │
                        ▼
              NodePort Service
                    (30009)
                        │
                        ▼
                Frontend Pods
                   (3 Replicas)
                        │
                        ▼
               Redis Slave Service
                        │
                        ▼
                 Redis Slaves
                 (2 Replicas)
                        │
                        ▼
                  Redis Master
                   (1 Replica)
```

---

## How It Works

### Redis Master

Acts as the primary Redis server handling writes.

---

### Redis Slaves

Replicate data from the Redis Master and can serve read requests.

---

### Frontend Application

The Guestbook application communicates with Redis through Kubernetes services.

Environment variable:

```text id="5fqgkx"
GET_HOSTS_FROM=dns
```

enables service discovery through Kubernetes DNS.

---

### NodePort Service

Exposes the Guestbook application externally on:

```text id="eynmpc"
30009
```

allowing browser access.

---

## Key Learnings

* Building multi-tier applications
* Deploying Redis master/slave architecture
* Creating multiple deployments
* Using Kubernetes service discovery
* Managing frontend and backend tiers
* Configuring resource requests
* Exposing applications using NodePort services

---

## Real-World Relevance

This architecture is commonly used in:

* Guestbook Applications
* Web Portals
* E-Commerce Platforms
* Microservices Architectures
* High Availability Redis Environments

Key concepts include:

* Service Discovery
* Application Scaling
* Load Distribution
* Backend Replication
* Multi-Tier Design

---

## Result

✅ Redis Master Deployed

✅ Redis Slave Replicas Running

✅ Frontend Deployment Running

✅ Internal Services Created

✅ NodePort Service Exposed

✅ Guestbook Application Accessible

✅ Lab Validation Passed

