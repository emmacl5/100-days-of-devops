# Day 61 - Using Init Containers in Kubernetes

## Challenge

The Nautilus DevOps team needed to test a deployment pattern where certain configuration tasks must be completed before the main application container starts.

To achieve this, an **Init Container** was used to generate a file that would later be consumed by the main application container.

---

## Objective

Create a Kubernetes Deployment named `ic-deploy-nautilus` with:

* 1 replica
* Label: `app=ic-nautilus`
* An Init Container named `ic-msg-nautilus`
* A Main Container named `ic-main-nautilus`
* A shared `emptyDir` volume named `ic-volume-nautilus`

The Init Container should write a message into a file, and the Main Container should continuously read and display that file.

---

## Solution

### Deployment Manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-nautilus
  labels:
    app: ic-nautilus

spec:
  replicas: 1

  selector:
    matchLabels:
      app: ic-nautilus

  template:
    metadata:
      labels:
        app: ic-nautilus

    spec:
      initContainers:
      - name: ic-msg-nautilus
        image: fedora:latest
        command:
        - /bin/bash
        - -c
        - echo Init Done - Welcome to xFusionCorp Industries > /ic/news

        volumeMounts:
        - name: ic-volume-nautilus
          mountPath: /ic

      containers:
      - name: ic-main-nautilus
        image: fedora:latest
        command:
        - /bin/bash
        - -c
        - while true; do cat /ic/news; sleep 5; done

        volumeMounts:
        - name: ic-volume-nautilus
          mountPath: /ic

      volumes:
      - name: ic-volume-nautilus
        emptyDir: {}
```

---

## Deployment

Apply the manifest:

```bash
kubectl apply -f ic-deploy-nautilus.yaml
```

Verify deployment:

```bash
kubectl get deployment
kubectl get pods
```

---

## Verification

Check the logs of the main container:

```bash
kubectl logs -l app=ic-nautilus -c ic-main-nautilus
```

Output:

```text
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
```

---

## How It Works

### Init Container

The Init Container executes before the application container starts.

It creates a file:

```text
/ic/news
```

with the content:

```text
Init Done - Welcome to xFusionCorp Industries
```

### Shared Volume

Both containers mount the same `emptyDir` volume:

```yaml
emptyDir: {}
```

This allows the Init Container to write data and the Main Container to read it.

### Main Container

The Main Container continuously reads the file:

```bash
while true; do
  cat /ic/news
  sleep 5
done
```

and prints the contents every 5 seconds.

---

## Key Learnings

* Understanding Init Containers
* Sharing data between containers using `emptyDir`
* Container startup sequencing in Kubernetes
* Using Deployments with Init Containers
* Real-world configuration initialization patterns

---

## Real-World Use Cases

Init Containers are commonly used for:

* Database schema migrations
* Downloading configuration files
* Preparing application environments
* Waiting for dependent services
* Generating runtime configuration before application startup

---

## Result

Successfully deployed a Kubernetes application using an Init Container to prepare configuration data before the main container started.

✅ Deployment Created
✅ Init Container Executed Successfully
✅ Shared Volume Mounted Correctly
✅ Main Container Consumed Generated Data

