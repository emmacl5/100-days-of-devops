# Day 66 - Deploying MySQL with Persistent Storage and Secrets in Kubernetes

## Background

The Nautilus DevOps team needed to deploy a MySQL database on Kubernetes while ensuring:

* Persistent data storage
* Secure credential management
* External accessibility
* Environment-based configuration

To achieve this, PersistentVolumes, PersistentVolumeClaims, Kubernetes Secrets, Deployments, and Services were used.

---

## Objective

Deploy a MySQL database with:

### Storage

* PersistentVolume: `mysql-pv`
* Capacity: `250Mi`

### PersistentVolumeClaim

* Name: `mysql-pv-claim`
* Request: `250Mi`

### Secrets

#### mysql-root-pass

```text id="1q4vwe"
password: YUIidhb667
```

#### mysql-user-pass

```text id="y36vya"
username: kodekloud_rin
password: TmPcZjtRQx
```

#### mysql-db-url

```text id="4r4v8m"
database: kodekloud_db2
```

### Deployment

* Name: `mysql-deployment`
* Image: `mysql:latest`
* Mount Path:

```text id="r2tzmb"
/var/lib/mysql
```

### Service

* Name: `mysql`
* Type: `NodePort`
* NodePort:

```text id="ifd7bx"
30007
```

---

## Solution

### Step 1 - Create PersistentVolume

```yaml id="jpt8p7"
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv

spec:
  capacity:
    storage: 250Mi

  accessModes:
    - ReadWriteOnce

  hostPath:
    path: /mnt/mysql-data

  persistentVolumeReclaimPolicy: Retain
```

---

### Step 2 - Create PersistentVolumeClaim

```yaml id="vhs68m"
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim

spec:
  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 250Mi
```

---

### Step 3 - Create Secrets

#### Root Password Secret

```yaml id="3v4nsl"
apiVersion: v1
kind: Secret

metadata:
  name: mysql-root-pass

stringData:
  password: YUIidhb667
```

#### Application User Secret

```yaml id="jl8tfz"
apiVersion: v1
kind: Secret

metadata:
  name: mysql-user-pass

stringData:
  username: kodekloud_rin
  password: TmPcZjtRQx
```

#### Database Secret

```yaml id="dx0obd"
apiVersion: v1
kind: Secret

metadata:
  name: mysql-db-url

stringData:
  database: kodekloud_db2
```

---

### Step 4 - Create MySQL Deployment

Environment variables are injected from Secrets:

```yaml id="pxb3pi"
env:
- name: MYSQL_ROOT_PASSWORD
  valueFrom:
    secretKeyRef:
      name: mysql-root-pass
      key: password

- name: MYSQL_DATABASE
  valueFrom:
    secretKeyRef:
      name: mysql-db-url
      key: database

- name: MYSQL_USER
  valueFrom:
    secretKeyRef:
      name: mysql-user-pass
      key: username

- name: MYSQL_PASSWORD
  valueFrom:
    secretKeyRef:
      name: mysql-user-pass
      key: password
```

Persistent storage mounted at:

```yaml id="fpgcnl"
volumeMounts:
- name: mysql-storage
  mountPath: /var/lib/mysql
```

---

### Step 5 - Create Service

```yaml id="5drimj"
apiVersion: v1
kind: Service

metadata:
  name: mysql

spec:
  type: NodePort

  ports:
  - port: 3306
    targetPort: 3306
    nodePort: 30007
```

---

## Verification

Verify storage:

```bash id="nxl0e7"
kubectl get pv
kubectl get pvc
```

Verify secrets:

```bash id="5rmdxv"
kubectl get secrets
```

Verify deployment:

```bash id="4lsfj0"
kubectl get deployment mysql-deployment
```

Verify pods:

```bash id="0rqf22"
kubectl get pods
```

Verify service:

```bash id="31xcn7"
kubectl get svc mysql
```

Expected:

```text id="mcb35g"
mysql-pv               Bound
mysql-pv-claim         Bound
mysql-deployment       Ready
mysql                  NodePort
```

---

## Architecture

```text id="0k5jhh"
Client
   │
   ▼
NodePort Service (30007)
   │
   ▼
MySQL Deployment
   │
   ▼
MySQL Container
   │
   ├── Secrets
   │     ├── mysql-root-pass
   │     ├── mysql-user-pass
   │     └── mysql-db-url
   │
   └── PersistentVolumeClaim
           │
           ▼
      PersistentVolume
```

---

## Key Learnings

* Creating PersistentVolumes
* Creating PersistentVolumeClaims
* Using Secrets for database credentials
* Injecting Secrets as environment variables
* Deploying stateful workloads
* Exposing applications using NodePort services
* Managing persistent data in Kubernetes

---

## Real-World Relevance

This deployment pattern is widely used in production environments:

* MySQL Databases
* PostgreSQL Databases
* MongoDB Deployments
* Stateful Applications
* Enterprise Workloads

Kubernetes Secrets help keep credentials out of manifests and container images, while PersistentVolumes ensure database data survives Pod restarts.

---

## Result

✅ PersistentVolume Created

✅ PersistentVolumeClaim Bound

✅ Secrets Created Successfully

✅ MySQL Deployment Running

✅ Persistent Storage Mounted

✅ Service Exposed on NodePort 30007

✅ Lab Validation Passed

