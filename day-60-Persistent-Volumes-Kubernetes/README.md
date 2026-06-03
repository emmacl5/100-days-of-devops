# Day 60 - Deploy an Application Using Persistent Volumes in Kubernetes

## Background

The Nautilus DevOps team needed a Kubernetes setup that could store web application data persistently. The requirement was to create a PersistentVolume, bind it using a PersistentVolumeClaim, mount it into a Pod, and expose the application externally using a NodePort Service.

---

## Insight

This task introduced one of the most important Kubernetes concepts:

> Persistent Storage

Unlike ephemeral container storage, Persistent Volumes allow application data to survive Pod restarts and rescheduling.

This is critical for:

* databases
* CMS platforms
* uploaded files
* logs
* application persistence

---

## Progress

### 1. Created PersistentVolume

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-devops

spec:
  storageClassName: manual

  capacity:
    storage: 4Gi

  accessModes:
    - ReadWriteOnce

  hostPath:
    path: /mnt/dba
```

---

### 2. Created PersistentVolumeClaim

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-devops

spec:
  storageClassName: manual

  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 1Gi
```

---

### 3. Created Pod Using Persistent Storage

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-devops
  labels:
    app: devops-web

spec:
  containers:
    - name: container-devops
      image: httpd:latest

      volumeMounts:
        - name: devops-storage
          mountPath: /usr/local/apache2/htdocs

  volumes:
    - name: devops-storage
      persistentVolumeClaim:
        claimName: pvc-devops
```

---

### 4. Created NodePort Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-devops

spec:
  type: NodePort

  selector:
    app: devops-web

  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
```

---

### 5. Applied Configuration

```bash
kubectl apply -f devops-pv-pvc-pod-svc.yaml
```

---

### 6. Verified Resources

```bash
kubectl get pv
kubectl get pvc
kubectl get pods
kubectl get svc
```

Confirmed:

* PV successfully created
* PVC successfully bound
* Pod running successfully
* Service exposed via NodePort 30008

---

## Explanation

A PersistentVolume was created using a hostPath backend. A PersistentVolumeClaim requested storage from the PV. The Pod mounted the claimed storage directly into the Apache web server document root.

A NodePort Service exposed the application externally.

This setup allowed the web server to use persistent storage instead of temporary container storage.

---

## What I Learned

* how PersistentVolumes work
* how PVCs bind to PVs
* how Pods consume persistent storage
* how Kubernetes separates storage from workloads
* how stateful storage integrates with applications

---

## Real-World Relevance

Persistent storage is essential in production Kubernetes environments for:

* databases
* user uploads
* CMS systems
* logs
* backups
* application persistence

---

## Improvement / Automation Idea

This setup could evolve using:

* dynamic provisioning
* StorageClasses
* cloud storage backends
* StatefulSets
* CSI drivers

