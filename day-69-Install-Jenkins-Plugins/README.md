# Day 69 - Installing Git and GitLab Plugins in Jenkins

## Background

The Nautilus DevOps team recently deployed a Jenkins server to support CI/CD pipelines.

Before creating build and deployment jobs, Jenkins needed additional plugins to integrate with source code repositories and Git-based workflows.

The objective was to install the Git and GitLab plugins using the Jenkins web interface.

---

## Objective

Install the following Jenkins plugins:

### Git Plugin

Provides:

* Git repository integration
* Source code checkout
* Branch management
* Git SCM support for pipelines

### GitLab Plugin

Provides:

* GitLab integration
* Webhook support
* Merge request integration
* Build status reporting

---

## Prerequisites

Access Jenkins UI using:

```text
Username: admin
Password: Adm!n321
```

---

## Solution

### Step 1 - Login to Jenkins

Open Jenkins dashboard from the lab environment.

Authenticate using:

```text
Username: admin
Password: Adm!n321
```

---

### Step 2 - Navigate to Plugin Manager

From the Jenkins dashboard:

```text
Manage Jenkins
    ↓
Plugins
```

---

### Step 3 - Install Git Plugin

Search:

```text
Git
```

Select:

```text
Git Plugin
```

Install plugin.

---

### Step 4 - Install GitLab Plugin

Search:

```text
GitLab
```

Select:

```text
GitLab Plugin
```

Install plugin.

---

### Step 5 - Restart Jenkins

If prompted:

```text
Restart Jenkins when installation is complete and no jobs are running
```

Wait for Jenkins to restart completely.

---

### Step 6 - Verify Installation

Navigate to:

```text
Manage Jenkins
    ↓
Plugins
    ↓
Installed Plugins
```

Verify:

```text
Git
GitLab
```

Both plugins should appear in the installed plugins list.

---

## Architecture

```text
Developer
    │
    ▼
 Git Repository
    │
    ▼
 Jenkins
    │
    ├── Git Plugin
    │      └── Source Code Integration
    │
    └── GitLab Plugin
           └── GitLab Integration
```

---

## Why These Plugins Matter

### Git Plugin

Enables Jenkins to:

* Clone repositories
* Pull source code
* Trigger builds from Git commits
* Work with branches and tags

---

### GitLab Plugin

Enables Jenkins to:

* Connect to GitLab projects
* Receive webhook notifications
* Trigger CI/CD pipelines automatically
* Report build status back to GitLab

---

## Key Learnings

* Managing Jenkins plugins
* Extending Jenkins functionality
* Preparing Jenkins for CI/CD workflows
* Integrating Jenkins with source control systems
* Understanding plugin-based architecture

---

## Real-World Relevance

Most Jenkins environments rely heavily on plugins.

Common enterprise plugins include:

* Git
* GitLab
* Docker
* Kubernetes
* Pipeline
* Blue Ocean
* SonarQube
* Slack
* Credentials Binding

Plugin management is a core Jenkins administration responsibility.

---

## Result

✅ Jenkins Login Successful

✅ Git Plugin Installed

✅ GitLab Plugin Installed

✅ Jenkins Restarted Successfully

✅ Plugin Installation Verified

✅ Lab Validation Passed


