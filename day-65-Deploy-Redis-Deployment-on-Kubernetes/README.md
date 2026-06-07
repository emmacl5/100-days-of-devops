# Day 65 - Deploying Redis with ConfigMaps and Volumes in Kubernetes

## Background

The Nautilus application team identified performance bottlenecks in an application running on Kubernetes. To improve response times and reduce database load, the team decided to introduce Redis as an in-memory caching solution.

Before moving to production, a Redis deployment needed to be created and tested inside the Kubernetes cluster.

---

## Objective

Deploy Redis on Kubernetes with the following requirements:

* Create a ConfigMap named `my-redis-config`
* Configure Redis memory limit:

  * `maxmemory 2mb`
* Create a Deployment named `redis-deployment`
* Use image:

  * `redis:alpine`
* Container name:

  * `redis-container`
* Replicas:

  * `1`
* CPU request:

  * `1 CPU`
* Expose:

  * Port `6379`
* Mount:

  * EmptyDir volume at `/redis-master-data`
  * ConfigMap volume at `/redis-master`

---

## Solution

### Step 1 - Create ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config

data:
  redis-config: |
    maxmemory 2mb
```

This ConfigMap stores Redis configuration separately from the container image.

---

### Step 2 - Create Redis Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment

spec:
  replicas: 1

  selector:
    matchLabels:
      app: redis

  template:
    metadata:
      labels:
        app: redis

    spec:
      containers:
      - name: redis-container
        image: redis:alpine

        ports:
        - containerPort: 6379

        resources:
          requests:
            cpu: "1"

        volumeMounts:
        - name: data
          mountPath: /redis-master-data

        - name: redis-config
          mountPath: /redis-master

      volumes:
      - name: data
        emptyDir: {}

      - name: redis-config
        configMap:
          name: my-redis-config
```

---

## Deployment

Apply the configuration:

```bash
kubectl apply -f redis-deployment.yaml
```

---

## Verification

Verify ConfigMap:

```bash
kubectl get configmap my-redis-config
```

Verify Deployment:

```bash
kubectl get deployment redis-deployment
```

Verify Pods:

```bash
kubectl get pods
```

Verify Deployment Details:

```bash
kubectl describe deployment redis-deployment
```

Expected:

```text
Deployment: redis-deployment
Image: redis:alpine
Container: redis-container
Replicas: 1
Port: 6379
CPU Request: 1
```

---

## Architecture

```text
Kubernetes Cluster
│
├── ConfigMap
│   └── my-redis-config
│       └── redis-config
│           └── maxmemory 2mb
│
└── Deployment
    └── redis-deployment
        └── redis-container
            ├── Port 6379
            ├── EmptyDir Volume
            │   └── /redis-master-data
            │
            └── ConfigMap Volume
                └── /redis-master
```

---

## How It Works

### ConfigMap

Stores Redis configuration outside the container image.

```text
maxmemory 2mb
```

This allows configuration changes without rebuilding images.

---

### EmptyDir Volume

```yaml
emptyDir: {}
```

Provides temporary storage that exists for the lifetime of the Pod.

Mounted at:

```text
/redis-master-data
```

---

### ConfigMap Volume

The ConfigMap is mounted as a volume:

```text
/redis-master
```

This makes Redis configuration files available inside the container.

---

## Key Learnings

* Creating and using ConfigMaps
* Mounting ConfigMaps as volumes
* Deploying Redis on Kubernetes
* Configuring resource requests
* Using EmptyDir volumes
* Separating application configuration from container images

---

## Real-World Relevance

Redis is commonly used in production for:

* Database query caching
* Session storage
* API response caching
* Message queues
* Real-time analytics
* Rate limiting

ConfigMaps are widely used for:

* Application configuration
* Feature flags
* Environment-specific settings
* Service tuning

---

## Result

✅ ConfigMap Created

✅ Redis Deployment Created

✅ Redis Container Running

✅ CPU Request Configured

✅ Volumes Mounted Successfully

✅ Port 6379 Exposed

✅ Lab Validation Passed

