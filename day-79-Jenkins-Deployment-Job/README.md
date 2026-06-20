# Day 79 - Automated Web Application Deployment with Jenkins

## Background

The xFusionCorp development team wanted to automate deployment of their web application whenever changes were pushed to the Git repository.

To achieve this, Jenkins was configured to monitor the repository and deploy the latest version of the application automatically to App Server 1.

---

## Objective

Create a Jenkins job named:

nautilus-app-deployment

Requirements:

* Monitor the Git repository for changes.
* Automatically trigger deployments when new commits are pushed to the master branch.
* Deploy the entire repository content to:

/var/www/html

* Ensure ownership of the deployment directory is assigned to user:

sarah

* Ensure Apache (httpd) is running.
* Verify deployment through the Load Balancer URL.

---

## Environment

### Jenkins

Server:

Jenkins Controller

Job:

nautilus-app-deployment

---

### Git Repository

Repository:

web

Repository Owner:

sarah

Repository URL:

http://gitea:3000/sarah/web.git

Branch:

master

---

### Application Server

Host:

stapp01

Deployment Directory:

/var/www/html

Web Server:

Apache HTTPD (Port 8080)

---

## Step 1 - Update Application Code

Logged in to App Server 1:

```bash
ssh sarah@stapp01
```

Updated:

```bash
/home/sarah/web/index.html
```

Content:

```html
Welcome to the xFusionCorp Industries
```

Committed and pushed changes:

```bash
git add .
git commit -m "Update homepage content"
git push origin master
```

---

## Step 2 - Create Jenkins Job

Created:

nautilus-app-deployment

Job Type:

Freestyle Project

---

## Step 3 - Configure Git Source

Source Code Management:

Git

Repository:

```text
http://gitea:3000/sarah/web.git
```

Branch:

```text
*/master
```

---

## Step 4 - Configure Auto Build Trigger

Enabled:

Poll SCM

Schedule:

```text
* * * * *
```

This checks the repository every minute for new commits.

---

## Step 5 - Configure Deployment

Added Execute Shell build step:

```bash
sshpass -p 'Sarah_pass123' ssh -o StrictHostKeyChecking=no sarah@stapp01 "
echo 'Sarah_pass123' | sudo -S chown -R sarah:sarah /var/www/html
echo 'Sarah_pass123' | sudo -S systemctl start httpd
cp -r /home/sarah/web/. /var/www/html/
"
```

---

## Troubleshooting

### Initial Failure

Error:

```text
sudo: a terminal is required to read the password
```

Cause:

sudo required a password but Jenkins was running non-interactively.

---

### Solution

Used:

```bash
echo 'Sarah_pass123' | sudo -S
```

to pass the password securely to sudo.

Result:

```text
Finished: SUCCESS
```

---

## Deployment Flow

Developer Commit
│
▼

```
Gitea

   │
   ▼
```

Jenkins Poll SCM

```
   │
   ▼
```

Jenkins Build

```
   │
   ▼
```

Deploy to App Server

```
   │
   ▼
```

Apache HTTPD

```
   │
   ▼
```

Load Balancer

```
   │
   ▼
```

End Users

---

## Verification

Application URL loaded successfully.

Homepage displayed:

```text
Welcome to the xFusionCorp Industries
```

Build Status:

```text
Finished: SUCCESS
```

---

## Key Learnings

* Jenkins Freestyle Jobs
* SCM Polling
* Git Integration
* Automated Deployments
* SSH Automation
* Apache Web Server Management
* Linux File Ownership
* Continuous Deployment Concepts

---

## Real-World Relevance

This deployment pattern is commonly used for:

* Internal Web Applications
* Documentation Portals
* Small Business Websites
* Legacy Deployment Pipelines

Modern implementations often replace Poll SCM with:

* Webhooks
* GitHub Actions
* GitLab CI/CD
* Jenkins Pipelines
* ArgoCD

---

## Result

✅ Git Repository Integrated

✅ Automatic Build Trigger Configured

✅ Deployment Automated

✅ Apache Service Verified

✅ Ownership Corrected

✅ Application Updated

✅ Website Accessible

✅ Lab Validation Passed

