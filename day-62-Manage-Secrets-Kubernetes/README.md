# Day 62 - Managing Sensitive Data with Kubernetes Secrets

## Background

The Nautilus DevOps team needed a secure way to store sensitive application information such as passwords, license keys, and credentials inside a Kubernetes cluster.

Instead of embedding sensitive data directly into container images or manifests, Kubernetes Secrets were used to securely store and consume this information.

---

## Objective

The goal of this task was to:

* Create a Kubernetes Secret named `news` from an existing file.
* Deploy a Pod named `secret-devops`.
* Mount the Secret inside the container filesystem.
* Verify that the secret data was available within the running container.

---

## Solution

### Step 1: Create the Secret

The secret was created using the existing file located at:

```text
/opt/news.txt
```

Command:

```bash
kubectl create secret generic news \
  --from-file=news.txt=/opt/news.txt
```

Verify:

```bash
kubectl get secret news
```

---

### Step 2: Create the Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-devops

spec:
  containers:
  - name: secret-container-devops
    image: debian:latest

    command:
    - sleep
    - "3600"

    volumeMounts:
    - name: secret-volume
      mountPath: /opt/cluster
      readOnly: true

  volumes:
  - name: secret-volume
    secret:
      secretName: news
```

Apply the manifest:

```bash
kubectl apply -f secret-devops.yaml
```

---

## Verification

Check Pod status:

```bash
kubectl get pod secret-devops
```

Expected:

```text
secret-devops   1/1   Running
```

Verify mounted secret files:

```bash
kubectl exec secret-devops \
  -c secret-container-devops \
  -- ls -l /opt/cluster
```

View secret contents:

```bash
kubectl exec secret-devops \
  -c secret-container-devops \
  -- cat /opt/cluster/news.txt
```

---

## How It Works

### Kubernetes Secret

A Secret is a Kubernetes object designed to store sensitive information securely.

Examples include:

* Passwords
* API Keys
* Tokens
* Certificates
* License Keys

---

### Secret Volume Mount

The Secret was mounted as a volume:

```yaml
volumes:
- name: secret-volume
  secret:
    secretName: news
```

This automatically made the secret available as files inside the container.

Mounted location:

```text
/opt/cluster/news.txt
```

---

### Running Container

The container used:

```yaml
image: debian:latest
```

and remained active using:

```bash
sleep 3600
```

This allowed verification of the mounted secret.

---

## Key Learnings

* Creating Kubernetes Secrets from files
* Storing sensitive information securely
* Mounting Secrets as volumes
* Accessing secret data inside containers
* Separating configuration and credentials from application code

---

## Real-World Use Cases

Kubernetes Secrets are commonly used for:

* Database credentials
* API tokens
* TLS certificates
* Cloud provider credentials
* Application license keys
* Authentication tokens

---

## Security Benefits

Using Secrets instead of hardcoding credentials:

✅ Improves security

✅ Reduces exposure of sensitive data

✅ Simplifies credential management

✅ Supports secure application deployments

---

## Result

Successfully created and consumed a Kubernetes Secret.

✅ Secret Created Successfully

✅ Secret Mounted Inside Pod

✅ Pod Running Successfully

✅ Sensitive Data Accessible from Container

✅ Lab Validation Passed

