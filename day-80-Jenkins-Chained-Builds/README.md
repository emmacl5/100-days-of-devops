# Day 80 - Jenkins Chained Builds for Automated Service Management

## Background

The xFusionCorp DevOps team wanted a mechanism to restart Apache automatically after a successful deployment.

Instead of mixing deployment and operational tasks into a single Jenkins job, they decided to use Jenkins chained builds.

This approach improves maintainability and follows CI/CD best practices.

---

## Objective

Create two Jenkins jobs:

### Upstream Job

xfusion-app-deployment

Responsibilities:

* Pull latest code from the master branch.
* Deploy updates to App Server 1.

### Downstream Job

manage-services

Responsibilities:

* Restart Apache (httpd) service.
* Run only when deployment succeeds.

---

## Environment

### Source Repository

Repository:

web

Owner:

sarah

Branch:

master

Repository URL:

http://gitea:3000/sarah/web.git

---

### Application Server

Host:

stapp01

Deployment Directory:

/var/www/html

Web Server:

Apache HTTPD

Port:

8080

---

## Solution

### Step 1 - Create Deployment Job

Created:

xfusion-app-deployment

Type:

Freestyle Project

---

### Build Step

```bash
sshpass -p 'Sarah_pass123' ssh -o StrictHostKeyChecking=no sarah@stapp01 "
cd /var/www/html
git config --global --add safe.directory /var/www/html
git pull origin master
"
```

Purpose:

* Connect to App Server 1
* Pull latest code from Git repository
* Update deployed application

---

### Step 2 - Create Service Management Job

Created:

manage-services

Type:

Freestyle Project

---

### Build Step

```bash
sshpass -p 'Sarah_pass123' ssh -o StrictHostKeyChecking=no sarah@stapp01 "
echo 'Sarah_pass123' | sudo -S systemctl restart httpd
echo 'Sarah_pass123' | sudo -S systemctl status httpd --no-pager
"
```

Purpose:

* Restart Apache
* Verify service status

---

### Step 3 - Configure Chained Build

Inside:

xfusion-app-deployment

Added:

Post-build Actions

→ Build other projects

Project:

manage-services

Condition:

Trigger only if build is stable

---

## Build Flow

Developer Push

```
  │

  ▼
```

Git Repository

```
  │

  ▼
```

xfusion-app-deployment

```
  │

  ▼
```

Successful Build

```
  │

  ▼
```

manage-services

```
  │

  ▼
```

Restart Apache

```
  │

  ▼
```

Application Available

---

## Build Results

### Deployment Job

Output:

```text
Updating repository...
Fast-forward
Triggering a new build of manage-services
Finished: SUCCESS
```

---

### Downstream Job

Output:

```text
httpd restarted successfully
Finished: SUCCESS
```

---

## Verification

Application loaded successfully through the load balancer.

Deployment completed successfully.

Apache restarted automatically.

Lab validation passed.

---

## Key Learnings

* Jenkins Chained Builds
* Upstream Jobs
* Downstream Jobs
* Post-build Actions
* Service Automation
* CI/CD Workflow Design
* Apache Service Management

---

## Real-World Relevance

Organizations commonly use downstream jobs for:

* Service Restarts
* Smoke Testing
* Integration Testing
* Security Scans
* Notifications
* Monitoring Validation
* Production Deployment Approval Chains

Benefits:

* Separation of Responsibilities
* Easier Troubleshooting
* Better Reusability
* Cleaner CI/CD Pipelines

---

## Result

✅ Deployment Job Created

✅ Service Management Job Created

✅ Chained Build Configured

✅ Apache Restart Automated

✅ Downstream Trigger Verified

✅ Application Accessible

✅ Lab Validation Passed

