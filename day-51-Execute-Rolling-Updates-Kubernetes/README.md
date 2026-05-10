# Day 51 - Perform a Kubernetes Rolling Update

## Background

The Nautilus application development team released an updated version of their Nginx application using the image `nginx:1.19`. The task was to update the running Kubernetes deployment without downtime using a rolling update strategy.

---

## Insight

This task introduced a key Kubernetes production capability:

> Rolling Updates

Rolling updates allow applications to be updated gradually without taking the entire service offline.

Benefits include:

* zero downtime deployments
* controlled rollout of new versions
* automatic replacement of old Pods
* safer production updates

---

## Progress

### 1. Connected to Jump Host

```bash id="z8r2v4"
ssh thor@jump-host
```

---

### 2. Inspected existing deployment

```bash id="k2j7l1"
kubectl describe deployment nginx-deployment
```

Discovered:

* deployment name: `nginx-deployment`
* container name: `nginx-container`
* current image: `nginx:1.16`

---

### 3. Executed rolling update

```bash id="n4x0m7"
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.19
```

---

### 4. Monitored rollout status

```bash id="g8u6r2"
kubectl rollout status deployment/nginx-deployment
```

Result:

```text id="r5m8q1"
deployment "nginx-deployment" successfully rolled out
```

---

### 5. Verified updated Pods

```bash id="h7w3d9"
kubectl get pods
```

Confirmed:

* all Pods running successfully

---

### 6. Verified updated image

```bash id="x2q6f4"
kubectl describe deployment nginx-deployment | grep -i image
```

Output:

```text id="m0n4p8"
Image: nginx:1.19
```

---

## Explanation

A rolling update was performed on the `nginx-deployment` deployment. Kubernetes gradually terminated Pods using the old image and replaced them with Pods running `nginx:1.19`.

This ensured continuous availability during the update process.

---

## What I Learned

* how to perform rolling updates in Kubernetes
* how Deployments manage application lifecycle
* importance of container names during updates
* how Kubernetes replaces Pods automatically
* how to monitor rollout status

---

## Real-World Relevance

Rolling updates are widely used in production Kubernetes environments because they:

* minimize downtime
* reduce deployment risk
* allow safe application upgrades

---

## Improvement / Automation Idea

This deployment process can be enhanced with:

* automated CI/CD pipelines
* canary deployments
* blue/green deployments
* rollback automation


