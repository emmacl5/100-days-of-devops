# Day 77 - Deploying a Static Website Using a Jenkins Pipeline

## Background

The xFusionCorp development team created a static website stored in a Gitea repository named:

web_app

The DevOps team's responsibility was to automate deployment of the latest code to App Server 1 using a Jenkins Pipeline.

The website repository was already cloned on the application server under:

/var/www/html

Apache was already installed and serving content on port 8080.

---

## Objective

Create a Jenkins Pipeline job named:

devops-webapp-job

Requirements:

* Use a Jenkins agent on App Server 1
* Deploy code from the web_app repository
* Use exactly one stage named:

Deploy

* Deploy directly to:

/var/www/html

* Ensure the application is available through the load balancer root URL

---

## Environment

### Source Control

Repository:

web_app

Developer:

sarah

---

### Jenkins Agent

Node Name:

App Server 1

Label:

stapp01

Remote Root Directory:

/home/sarah/jenkins_agent

---

### Web Server

Apache HTTP Server

Document Root:

/var/www/html

---

## Solution

### Step 1 - Configure Jenkins Agent

Created a Jenkins node:

Name:

App Server 1

Label:

stapp01

Remote Root:

/home/sarah/jenkins_agent

Launch Method:

Launch agents by connecting it to the controller

---

### Step 2 - Resolve Java Compatibility Issue

Initial connection failed:

UnsupportedClassVersionError

Root Cause:

Jenkins agent required Java 17 while App Server 1 was using Java 11.

Installed:

java-17-openjdk

Successfully launched agent.

Verification:

Agent successfully connected and online

---

### Step 3 - Install Pipeline Plugin

Pipeline option was not available during job creation.

Installed:

Pipeline Plugin

Restarted Jenkins.

---

### Step 4 - Create Pipeline Job

Created:

devops-webapp-job

Job Type:

Pipeline

---

### Step 5 - Configure Pipeline Script

Pipeline:

```groovy
pipeline {
    agent {
        label 'stapp01'
    }

    stages {
        stage('Deploy') {
            steps {
                sh '''
                    cd /var/www/html
                    git config --global --add safe.directory /var/www/html
                    git pull origin master
                '''
            }
        }
    }
}
```

---

## Pipeline Workflow

Developer Pushes Code
│
▼
Gitea
│
▼
Jenkins Pipeline
│
▼
App Server 1
│
▼
git pull
│
▼
Apache Document Root
│
▼
Application Updated

---

## Build Execution

Executed:

Build Now

Result:

Finished: SUCCESS

Console Output:

From http://gitea:3000/sarah/web_app

Already up to date.

Finished: SUCCESS

---

## Verification

Verified:

* Jenkins agent connected successfully
* Pipeline executed successfully
* Git repository updated
* Website served from document root
* Application accessible through load balancer

---

## Key Learnings

* Jenkins Pipelines
* Jenkins Agents
* Pipeline Plugin Installation
* Git-Based Deployments
* Gitea Integration
* CI/CD Fundamentals
* Automated Application Deployment

---

## Real-World Relevance

This deployment pattern is commonly used for:

* Static Websites
* Internal Portals
* Documentation Sites
* Lightweight Applications

Modern versions of this workflow often include:

* GitHub Actions
* GitLab CI/CD
* Jenkins Pipelines
* ArgoCD
* GitOps

---

## Result

✅ Jenkins Agent Configured

✅ Java Compatibility Fixed

✅ Pipeline Plugin Installed

✅ Pipeline Job Created

✅ Deploy Stage Configured

✅ Git Deployment Automated

✅ Website Accessible

✅ Lab Validation Passed

