# Day 75 - Configuring Jenkins SSH Build Agents

## Background

The xFusionCorp DevOps team needed Jenkins to execute jobs on multiple application servers instead of running everything on the Jenkins controller.

To achieve this, all application servers were added as SSH build agents (formerly called slave nodes).

This setup allows Jenkins to distribute workloads and execute tasks directly on target servers.

---

## Objective

Add the following application servers as Jenkins SSH agents:

| Jenkins Node Name | Label   | Remote Root Directory |
| ----------------- | ------- | --------------------- |
| App_server_1      | stapp01 | /home/tony/jenkins    |
| App_server_2      | stapp02 | /home/steve/jenkins   |
| App_server_3      | stapp03 | /home/banner/jenkins  |

All agents must be online and available for builds.

---

## Solution

### Step 1 - Login to Jenkins

Access Jenkins:

```text
Username: admin
Password: Adm!n321
```

---

### Step 2 - Install Required Plugin

Verified:

```text
SSH Build Agents Plugin
```

was installed.

This plugin enables Jenkins to connect to remote servers through SSH.

---

### Step 3 - Create Credentials

Configured credentials for:

```text
stapp01
stapp02
stapp03
```

Credential Type:

```text
Username with Password
```

---

### Step 4 - Configure App_server_1

Created node:

```text
Name: App_server_1
```

Configuration:

```text
Remote Root Directory:
/home/tony/jenkins

Label:
stapp01

Launch Method:
Launch agents via SSH

Host:
stapp01
```

---

### Step 5 - Configure App_server_2

Created node:

```text
Name: App_server_2
```

Configuration:

```text
Remote Root Directory:
/home/steve/jenkins

Label:
stapp02

Launch Method:
Launch agents via SSH

Host:
stapp02
```

---

### Step 6 - Configure App_server_3

Created node:

```text
Name: App_server_3
```

Configuration:

```text
Remote Root Directory:
/home/banner/jenkins

Label:
stapp03

Launch Method:
Launch agents via SSH

Host:
stapp03
```

---

## Verification

Opened:

```text
Manage Jenkins
→ Nodes
```

Verified all agents were online.

Successful connection message:

```text
Agent successfully connected and online
```

---

## Architecture

```text
                 Jenkins Controller
                         │
         ┌───────────────┼───────────────┐
         │               │               │
         ▼               ▼               ▼

    App_server_1    App_server_2    App_server_3
      stapp01         stapp02         stapp03

   /home/tony     /home/steve     /home/banner
     /jenkins       /jenkins        /jenkins
```

---

## Why Build Agents Matter

Without agents:

```text
All builds run on Jenkins Controller
```

Problems:

* Resource bottlenecks
* Slower builds
* Poor scalability

With agents:

```text
Builds are distributed
```

Benefits:

* Horizontal scaling
* Faster execution
* Better resource utilization
* Environment-specific builds

---

## Key Learnings

* Jenkins Nodes
* Jenkins SSH Agents
* Jenkins Credentials
* Remote Build Execution
* Distributed CI/CD Architecture
* Jenkins Scalability Concepts

---

## Real-World Relevance

Enterprise Jenkins environments commonly use:

* Linux Build Agents
* Docker Build Agents
* Kubernetes Dynamic Agents
* Cloud-based Agents

Benefits:

* Parallel Builds
* Faster Pipelines
* Environment Isolation
* Improved Scalability

---

## Result

✅ App_server_1 Added

✅ App_server_2 Added

✅ App_server_3 Added

✅ Labels Configured

✅ Remote Root Directories Configured

✅ SSH Connections Established

✅ All Agents Online

✅ Lab Validation Passed

