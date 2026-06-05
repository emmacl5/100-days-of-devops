# Day 63 - Deploying a Multi-Tier Application on Kubernetes

## Background

The Nautilus DevOps team needed to deploy the Iron Gallery application on a Kubernetes cluster. The application consists of two components:

* Iron Gallery (Frontend)
* IronDB (Database)

The goal was to deploy both applications inside a dedicated namespace, configure storage volumes, define resource limits, and expose the services appropriately.

---

## Objective

Create and configure the following Kubernetes resources:

### Namespace

* `iron-namespace-xfusion`

### Frontend Deployment

* Deployment: `iron-gallery-deployment-xfusion`
* Image: `kodekloud/irongallery:2.0`
* Replicas: 1
* Label: `run=iron-gallery`
* Resource Limits:

  * Memory: `100Mi`
  * CPU: `50m`
* Volumes:

  * `config`
  * `images`

### Database Deployment

* Deployment: `iron-db-deployment-xfusion`
* Image: `kodekloud/irondb:2.0`
* Replicas: 1
* Label: `db=mariadb`
* Environment Variables:

  * `MYSQL_DATABASE=database_apache`
  * `MYSQL_ROOT_PASSWORD=<custom password>`
  * `MYSQL_USER=<custom user>`
  * `MYSQL_PASSWORD=<custom password>`
* Volume:

  * `db`

### Services

* `iron-db-service-xfusion`

  * Type: ClusterIP
  * Port: 3306

* `iron-gallery-service-xfusion`

  * Type: NodePort
  * Port: 80
  * NodePort: 32678

---

## Solution

### Create Namespace

```bash
kubectl create namespace iron-namespace-xfusion
```

---

### Deploy Iron Gallery

```yaml
resources:
  limits:
    memory: "100Mi"
    cpu: "50m"
```

Volume mounts:

```yaml
volumeMounts:
- name: config
  mountPath: /usr/share/nginx/html/data

- name: images
  mountPath: /usr/share/nginx/html/uploads
```

Volumes:

```yaml
volumes:
- name: config
  emptyDir: {}

- name: images
  emptyDir: {}
```

---

### Deploy IronDB

Environment variables:

```yaml
env:
- name: MYSQL_DATABASE
  value: database_apache

- name: MYSQL_ROOT_PASSWORD
  value: RootPass123!

- name: MYSQL_USER
  value: ironuser

- name: MYSQL_PASSWORD
  value: UserPass123!
```

Volume:

```yaml
volumes:
- name: db
  emptyDir: {}
```

---

### Create Database Service

```yaml
kind: Service
type: ClusterIP
port: 3306
targetPort: 3306
```

Purpose:

* Internal communication inside the cluster.
* Database remains inaccessible from outside.

---

### Create Gallery Service

```yaml
kind: Service
type: NodePort
port: 80
targetPort: 80
nodePort: 32678
```

Purpose:

* Expose the Iron Gallery application externally.

---

## Verification

Verify namespace:

```bash
kubectl get ns
```

Verify deployments:

```bash
kubectl get deployments -n iron-namespace-xfusion
```

Verify pods:

```bash
kubectl get pods -n iron-namespace-xfusion
```

Verify services:

```bash
kubectl get svc -n iron-namespace-xfusion
```

Expected:

```text
iron-gallery-deployment-xfusion   1/1
iron-db-deployment-xfusion        1/1

iron-gallery-service-xfusion      NodePort
iron-db-service-xfusion           ClusterIP
```

---

## Architecture

```text
iron-namespace-xfusion
│
├── iron-gallery-deployment-xfusion
│   └── iron-gallery-container-xfusion
│       ├── config (emptyDir)
│       └── images (emptyDir)
│
├── iron-db-deployment-xfusion
│   └── iron-db-container-xfusion
│       └── db (emptyDir)
│
├── iron-gallery-service-xfusion
│   └── NodePort: 32678
│
└── iron-db-service-xfusion
    └── ClusterIP: 3306
```

---

## Key Learnings

* Creating and managing Kubernetes namespaces
* Deploying multi-tier applications
* Using resource limits to control container consumption
* Configuring environment variables for databases
* Using EmptyDir volumes
* Exposing applications with NodePort services
* Internal service communication using ClusterIP
* Organizing workloads using labels and selectors

---

## Real-World Relevance

This architecture closely resembles production deployments where:

* Frontend applications are exposed externally.
* Databases remain internal to the cluster.
* Resource limits prevent resource starvation.
* Namespaces isolate workloads.
* Volumes provide writable storage.

---

## Result

✅ Namespace created successfully

✅ Frontend deployment running

✅ Database deployment running

✅ Resource limits configured

✅ Services exposed correctly

✅ Application accessible through NodePort

✅ Lab validation passed

