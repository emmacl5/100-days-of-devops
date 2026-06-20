# Day 78 - Building a Parameterized Jenkins Deployment Pipeline

## Background

The xFusionCorp development team maintains a static website in a Gitea repository called:

web_app

The DevOps team was tasked with creating a flexible Jenkins Pipeline capable of deploying different branches of the application based on user input.

Instead of creating separate deployment jobs for each branch, a parameterized pipeline was implemented.

---

## Objective

Create a Jenkins Pipeline job named:

devops-webapp-job

Requirements:

* Add a Jenkins agent named App Server 1
* Configure label:

stapp01

* Configure remote root directory:

/home/sarah/jenkins_agent

* Add a String Parameter:

BRANCH

* Create a single stage named:

Deploy

* Deploy:

master branch when BRANCH=master

feature branch when BRANCH=feature

* Deploy directly into:

/var/www/html

* Ensure the website is accessible through the load balancer root URL

---

## Environment

### Source Control

Repository:

web_app

Developer:

sarah

---

### Application Server

Host:

stapp01

Document Root:

/var/www/html

Web Server:

Apache (Port 8080)

---

### Jenkins Agent

Name:

App Server 1

Label:

stapp01

Remote Workspace:

/home/sarah/jenkins_agent

---

## Challenges Encountered

### Agent Connection Failure

Initial Issue:

App Server 1 was offline.

Pipeline Output:

Still waiting to schedule task

'App Server 1' is offline

---

### Missing Agent JAR

Issue:

agent.jar not found

Resolution:

Downloaded agent.jar again from Jenkins.

---

### Java Version Compatibility

Error:

UnsupportedClassVersionError

Root Cause:

Agent required Java 17 while App Server 1 was running Java 11.

Solution:

Installed:

java-17-openjdk

Verification:

java -version

Output:

OpenJDK 17

---

### Agent Successfully Connected

Result:

Agent successfully connected and online

---

## Pipeline Configuration

### Parameter

Name:

BRANCH

Default Value:

master

---

### Pipeline Script

```groovy
pipeline {
    agent {
        label 'stapp01'
    }

    parameters {
        string(
            name: 'BRANCH',
            defaultValue: 'master',
            description: 'master or feature'
        )
    }

    stages {
        stage('Deploy') {
            steps {
                sh '''
                    cd /var/www/html
                    git config --global --add safe.directory /var/www/html

                    if [ "$BRANCH" = "master" ]; then
                        git fetch origin
                        git checkout master
                        git pull origin master

                    elif [ "$BRANCH" = "feature" ]; then
                        git fetch origin
                        git checkout feature
                        git pull origin feature

                    else
                        echo "Invalid branch"
                        exit 1
                    fi
                '''
            }
        }
    }
}
```

---

## Build Execution

Executed:

Build with Parameters

Parameter:

BRANCH = master

Build Result:

Finished: SUCCESS

Console Output:

From http://gitea:3000/sarah/web_app

Already up to date.

Finished: SUCCESS

---

## Architecture

Developer
│
▼

Gitea Repository
(web_app)

│
▼

Parameterized Jenkins Pipeline

│

├── master

└── feature

│
▼

App Server 1

│
▼

Apache Document Root

/var/www/html

│
▼

Load Balancer

│
▼

End Users

---

## Key Learnings

* Jenkins Pipeline Jobs
* Jenkins Parameters
* Conditional Logic in Pipelines
* Branch-Based Deployments
* Jenkins Agents
* Java Troubleshooting
* Git-Based Application Delivery

---

## Real-World Relevance

This pattern is commonly used for:

* Development Deployments
* Feature Branch Testing
* Staging Environments
* Controlled Release Workflows
* CI/CD Automation

Modern organizations frequently deploy different Git branches through a single pipeline using parameters.

---

## Result

✅ Jenkins Agent Configured

✅ Java 17 Installed

✅ Agent Connected Successfully

✅ Parameterized Pipeline Created

✅ BRANCH Parameter Added

✅ Conditional Deployment Logic Implemented

✅ Deployment Successful

✅ Website Accessible

✅ Lab Validation Passed

