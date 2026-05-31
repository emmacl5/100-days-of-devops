# Day 57 - Configure Environment Variables in a Kubernetes Pod

## Background

The Nautilus DevOps team needed to configure environment variables inside a Kubernetes Pod for an application responsible for generating greeting messages dynamically.

The requirement was to:

* create a Pod
* configure multiple environment variables
* execute a command using those variables
* avoid restart loops

---

## Insight

This task introduced a fundamental Kubernetes application configuration mechanism:

> Environment Variables

Environment variables allow applications to:

* receive runtime configuration
* separate configuration from application code
* improve portability and flexibility

---

## Progress

### 1. Created Pod Manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting

spec:
  restartPolicy: Never

  containers:
    - name: print-env-container
      image: bash

      env:
        - name: GREETING
          value: "Welcome to"

        - name: COMPANY
          value: "xFusionCorp"

        - name: GROUP
          value: "Group"

      command:
        - "/bin/sh"
        - "-c"
        - 'echo "$(GREETING) $(COMPANY) $(GROUP)"'
```

---

### 2. Applied Configuration

```bash
kubectl apply -f print-envars-greeting.yaml
```

---

### 3. Verified Pod Logs

```bash
kubectl logs -f print-envars-greeting
```

Output:

```text
Welcome to xFusionCorp Group
```

---

## Explanation

A Pod was created using the `bash` image. Three environment variables were configured inside the container:

* GREETING
* COMPANY
* GROUP

The container executed a shell command that combined these variables into a greeting message.

The `restartPolicy: Never` setting ensured the Pod exited cleanly without entering a crash loop.

---

## What I Learned

* how Kubernetes environment variables work
* how to configure runtime application settings
* how commands execute inside containers
* importance of restart policies
* how Pods use environment-based configuration

---

## Real-World Relevance

Environment variables are heavily used in production Kubernetes environments for:

* application configuration
* database credentials
* API endpoints
* feature flags
* deployment customization

---

## Improvement / Automation Idea

This setup could be improved using:

* ConfigMaps
* Secrets
* Helm templating
* environment-specific deployment manifests

