# Day 64 - Troubleshooting a Kubernetes Python Application Deployment

## Background

The Nautilus DevOps team discovered that a Python Flask application deployed on Kubernetes was not starting correctly.

The deployment and service resources had already been created, but the application was unavailable through the configured NodePort service.

The task was to investigate the deployment, identify the root cause, and restore application availability.

---

## Objective

Fix the Kubernetes deployment and ensure the Python Flask application is accessible through:

```text
NodePort: 32345
```

Requirements:

* Deployment name: `python-deployment-xfusion`
* Service name: `python-service-xfusion`
* Correct image: `poroko/flask-demo-app`
* NodePort: `32345`
* Target Port: Flask default port (`5000`)

---

## Investigation

### Check Existing Service

```bash
kubectl get svc
```

Output:

```text
python-service-xfusion   NodePort   10.43.x.x   8080:32345/TCP
```

The NodePort configuration looked correct.

---

### Inspect Deployment

```bash
kubectl describe deployment python-deployment-xfusion
```

Container configuration:

```text
Container Name:
python-container-xfusion

Image:
poroko/flask-app-demo
```

The image name did not match the required application image.

---

## Root Cause

The deployment was using an incorrect container image:

```text
poroko/flask-app-demo
```

Required image:

```text
poroko/flask-demo-app
```

Because the image was invalid, the application pods could not start successfully.

---

## Solution

Update the deployment image:

```bash
kubectl set image deployment/python-deployment-xfusion \
python-container-xfusion=poroko/flask-demo-app
```

---

## Verification

Monitor rollout:

```bash
kubectl rollout status deployment/python-deployment-xfusion
```

Verify pods:

```bash
kubectl get pods
```

Verify deployment image:

```bash
kubectl describe deployment python-deployment-xfusion | grep -i image
```

Verify service:

```bash
kubectl describe svc python-service-xfusion
```

Expected:

```text
NodePort: 32345
TargetPort: 5000
```

---

## How It Works

### Deployment

Runs the Flask application container:

```text
poroko/flask-demo-app
```

The application listens on:

```text
5000/TCP
```

---

### Service

The NodePort service exposes the application externally:

```text
Client
   │
   ▼
NodePort 32345
   │
   ▼
Service
   │
   ▼
Pod Port 5000
```

---

## Key Learnings

* Troubleshooting Kubernetes deployments
* Inspecting deployment configurations
* Identifying incorrect container images
* Understanding NodePort networking
* Validating service-to-pod connectivity
* Using rollout status during recovery

---

## Real-World Relevance

This is one of the most common production incidents:

* Wrong container image
* Incorrect image tag
* Failed deployment rollout
* Service unavailable after deployment

The ability to quickly identify and correct deployment configuration issues is a critical DevOps and SRE skill.

---

## Result

✅ Deployment fixed

✅ Correct image deployed

✅ Application pods running

✅ Service configured correctly

✅ Application accessible via NodePort 32345

✅ Lab validation passed


