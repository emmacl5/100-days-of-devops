# Day 81 - Deploying a Static Website Using a Jenkins Pipeline

## Overview

Today I implemented a complete CI/CD workflow using Jenkins Pipeline, Gitea, and Apache HTTP Server.

The objective was to deploy a static website hosted in a Gitea repository to App Server 1 and verify that the deployment was successful using an automated test stage.

---

## Objectives

* Update website content in the Git repository.
* Push changes to the master branch.
* Configure App Server 1 as a Jenkins Agent.
* Create a Jenkins Pipeline job named `deploy-job`.
* Deploy the website automatically from the Git repository.
* Verify the deployment using a testing stage.
* Ensure the application is accessible through the Load Balancer.

---

## Environment

### Jenkins

* Username: `admin`
* Pipeline Job: `deploy-job`

### Gitea Repository

* Repository: `sarah/web`
* Branch: `master`

### Application Server

* Hostname: `stapp01`
* Document Root: `/var/www/html`
* Web Server: Apache HTTPD
* Port: `8080`

### Jenkins Agent

* Name: `App Server 1`
* Label: `stapp01`
* Remote Root Directory:

```text
/home/sarah/jenkins_agent
```

---

## Step 1: Update Website Content

Connected to App Server 1:

```bash
ssh sarah@stapp01
```

Moved into the repository:

```bash
cd /var/www/html
```

Updated the homepage:

```bash
echo "Welcome to xFusionCorp Industries" > index.html
```

Committed and pushed changes:

```bash
git add index.html
git commit -m "Update homepage content"
git push origin master
```

---

## Step 2: Configure Jenkins Agent

Created a Jenkins agent named:

```text
App Server 1
```

Configuration:

```text
Name: App Server 1
Label: stapp01
Remote Root Directory: /home/sarah/jenkins_agent
Launch Method: Launch agents via SSH
Host: stapp01
User: sarah
```

Installed Java 17 on App Server 1:

```bash
sudo yum install -y java-17-openjdk
```

Verified the agent status:

```text
Agent successfully connected and online
```

---

## Step 3: Create Jenkins Pipeline

Created a Pipeline job named:

```text
deploy-job
```

Pipeline script:

```groovy
pipeline {
    agent { label 'stapp01' }

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

        stage('Test') {
            steps {
                sh '''
                    curl -f http://stlb01:8091
                '''
            }
        }
    }
}
```

---

## Pipeline Stages

### Deploy Stage

Responsibilities:

* Navigate to the application directory.
* Configure Git safe directory.
* Pull latest changes from the master branch.

Commands:

```bash
cd /var/www/html
git config --global --add safe.directory /var/www/html
git pull origin master
```

---

### Test Stage

Responsibilities:

* Verify the application is accessible through the Load Balancer.

Command:

```bash
curl -f http://stlb01:8091
```

The build fails automatically if the website is unavailable.

---

## Build Result

Successful pipeline execution:

```text
Deploy → SUCCESS
Test → SUCCESS
Finished: SUCCESS
```

---

## Verification

Application URL:

```text
http://stlb01:8091
```

Output:

```text
Welcome to xFusionCorp Industries
```

---

## Challenges Encountered

### Java Compatibility Issue

Error:

```text
UnsupportedClassVersionError
```

Cause:

Jenkins agent required Java 17 while the server was using an older Java version.

Resolution:

```bash
sudo yum install -y java-17-openjdk
```

---

### Agent Connectivity Issues

Initially, the Jenkins agent failed to connect.

Resolution:

* Configured SSH credentials correctly.
* Installed Java 17.
* Verified SSH launch configuration.
* Confirmed agent status was online.

---

## Architecture

```text
Developer
    │
    ▼
Gitea Repository
(sarah/web)
    │
    ▼
Jenkins Pipeline
(deploy-job)
    │
    ▼
App Server 1
(stapp01)
    │
    ▼
Apache HTTPD
    │
    ▼
Load Balancer
(stlb01:8091)
    │
    ▼
End Users
```

---

## Key Learnings

* Jenkins Pipeline Jobs
* Jenkins SSH Agents
* Gitea Integration
* Git-Based Deployments
* Apache Web Server Management
* CI/CD Automation
* Application Health Validation
* Troubleshooting Jenkins Agent Connectivity

---

## Real-World Use Cases

This deployment pattern is commonly used for:

* Static Website Deployments
* Internal Documentation Sites
* Development Environments
* Automated Application Delivery
* Continuous Deployment Workflows

---

## Result

✅ Jenkins Agent Configured

✅ Java 17 Installed

✅ Repository Updated

✅ Pipeline Job Created

✅ Deploy Stage Implemented

✅ Test Stage Implemented

✅ Application Successfully Deployed

✅ Website Accessible Through Load Balancer

✅ Lab Validation Passed

#100DaysOfDevOps #Day81

