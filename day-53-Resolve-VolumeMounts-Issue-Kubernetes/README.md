# Day 53 - Troubleshoot Nginx + PHP-FPM in Kubernetes

## Background

The Nautilus DevOps team encountered an issue with a Kubernetes-hosted Nginx and PHP-FPM application stack. The website became inaccessible after configuration problems affected communication between Nginx and PHP-FPM.

The task was to:

* investigate the issue
* fix the Nginx configuration
* restore application functionality
* deploy the PHP application file correctly

---

## Insight

This task introduced a real-world Kubernetes troubleshooting workflow involving:

* ConfigMaps
* multi-container Pods
* Nginx + PHP-FPM integration
* shared document roots
* runtime debugging

Key lesson:

> In containerized environments, both services must agree on filesystem paths and runtime configuration.

---

## Progress

### 1. Investigated Pod and ConfigMap

```bash
kubectl describe pod nginx-phpfpm
kubectl get configmap nginx-config -o yaml
```

Discovered issues:

* invalid Nginx document root
* incorrect request handling for `/`
* PHP-FPM unable to locate PHP files

---

### 2. Fixed Nginx Configuration

Updated ConfigMap values:

```nginx
root /usr/share/nginx/html;

location / {
  try_files $uri $uri/ /index.php;
}
```

Ensured PHP requests were forwarded correctly:

```nginx
fastcgi_pass 127.0.0.1:9000;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
```

---

### 3. Reloaded Nginx

```bash
kubectl exec nginx-phpfpm -c nginx-container -- nginx -s reload
```

---

### 4. Copied Application File

Copied `index.php` into both containers:

```bash
kubectl cp /home/thor/index.php nginx-phpfpm:/usr/share/nginx/html/index.php -c nginx-container

kubectl cp /home/thor/index.php nginx-phpfpm:/usr/share/nginx/html/index.php -c php-fpm-container
```

---

## Verification

Verified:

* Pod status:

```bash
kubectl get pods
```

Output:

```text
2/2 Running
```

Website became accessible successfully using the Website button.

---

## Explanation

The root cause was a mismatch between:

* Nginx document root
* PHP-FPM accessible file path

Additionally, Nginx routing for `/` did not properly redirect requests to `index.php`.

Once:

* the ConfigMap was corrected
* the document root aligned
* PHP files copied to both containers

the application worked successfully.

---

## What I Learned

* how ConfigMaps affect running containers
* troubleshooting Nginx + PHP-FPM integration
* importance of shared filesystem paths
* debugging multi-container Pods
* how Kubernetes networking works inside a Pod

---

## Real-World Relevance

This type of issue is extremely common in production:

* bad ConfigMap updates
* incorrect document roots
* PHP-FPM path mismatches
* Nginx routing problems

Troubleshooting these issues is a core DevOps/SRE skill.

---

## Improvement / Automation Idea

This setup could be improved using:

* shared persistent volumes
* readiness probes
* automated configuration validation
* CI/CD configuration testing

