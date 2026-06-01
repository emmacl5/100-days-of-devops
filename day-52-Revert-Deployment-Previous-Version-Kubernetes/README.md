# Day 52 - Rollback a Kubernetes Deployment

## Background

After deploying a new application release, the Nautilus DevOps team received reports of a production issue caused by the latest version. To restore stability quickly, the deployment needed to be rolled back to the previous revision.

---

## Insight

This task introduced another critical Kubernetes production feature:

> Rollbacks

Kubernetes maintains deployment revision history, allowing teams to revert applications to previously stable versions with minimal downtime.

Key benefits:

* rapid recovery from failed releases
* safer production deployments
* minimal service disruption

---

## Progress

### 1. Connected to Jump Host

```bash id="k5m3x7"
ssh thor@jump-host
```

---

### 2. Checked deployment history

```bash id="h8p1r4"
kubectl rollout history deployment/nginx-deployment
```

Verified previous revisions existed.

---

### 3. Rolled back deployment

```bash id="m2v8n1"
kubectl rollout undo deployment/nginx-deployment
```

---

### 4. Verified rollout status

```bash id="w4q7t6"
kubectl rollout status deployment/nginx-deployment
```

Output:

```text id="r9d0k2"
deployment "nginx-deployment" successfully rolled out
```

---

### 5. Verified running Pods

```bash id="y7m4c8"
kubectl get pods
```

Confirmed:

* all Pods returned to Running state

---

### 6. Verified reverted image

```bash id="n5f2w9"
kubectl describe deployment nginx-deployment | grep -i image
```

Confirmed deployment reverted to the previous image version.

---

## Verification

Commands used:

```bash id="j6q8z1"
kubectl rollout history deployment/nginx-deployment
kubectl rollout status deployment/nginx-deployment
kubectl get pods
```

All checks passed successfully.

---

## Explanation

A rollback operation was performed on the `nginx-deployment` deployment. Kubernetes automatically replaced the problematic Pods with Pods from the previous stable revision.

This ensured quick service recovery without rebuilding or recreating the deployment manually.

---

## What I Learned

* how Kubernetes deployment revisions work
* how to perform rollbacks safely
* importance of deployment history
* how Kubernetes simplifies production recovery
* how rolling updates and rollbacks complement each other

---

## Real-World Relevance

Rollbacks are critical in production because:

* releases can introduce bugs
* deployments may fail unexpectedly
* quick recovery reduces downtime and customer impact

---

## Improvement / Automation Idea

This workflow can be enhanced with:

* automated health checks
* canary deployments
* blue/green deployment strategies
* automated rollback triggers

