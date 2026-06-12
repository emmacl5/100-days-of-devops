# Day 71 - Automating Package Installation with Jenkins

## Background

The Nautilus DevOps team wanted to automate software installation tasks across the Stratos Datacenter infrastructure using Jenkins.

To accomplish this, a parameterized Jenkins job was created that allows administrators to specify a package name and automatically install it on the Storage Server.

---

## Objective

Create a Jenkins job named:

```text
install-packages
```

Requirements:

* Add a String Parameter named `PACKAGE`
* Use the parameter value to install packages on the Storage Server
* Execute the installation remotely through Jenkins
* Verify successful execution
* Ensure repeatable and reliable builds

---

## Solution

### Step 1 - Login to Jenkins

Access Jenkins and authenticate:

```text
Username: admin
Password: Adm!n321
```

---

### Step 2 - Create a Freestyle Job

Created:

```text
install-packages
```

Job Type:

```text
Freestyle Project
```

---

### Step 3 - Add Parameter

Enabled:

```text
This project is parameterized
```

Added String Parameter:

```text
Name: PACKAGE
```

Example value:

```text
vim-enhanced
```

---

### Step 4 - Configure Build Step

Added:

```text
Execute Shell
```

Script:

```bash
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 "echo 'Bl@kW' | sudo -S yum install -y ${PACKAGE}"
```

---

## Troubleshooting

### Initial Failure

Build #1 failed with:

```text
Could not resolve hostname storage
```

Root Cause:

Incorrect hostname was used.

Incorrect:

```text
storage
```

Correct:

```text
ststor01
```

---

### Authentication Issue

Initially incorrect credentials were used.

Correct Storage Server credentials:

```text
Username: natasha
Password: Bl@kW
```

After updating the command with the correct hostname and credentials, the build succeeded.

---

## Build Execution

Executed:

```text
PACKAGE = vim-enhanced
```

Build Result:

```text
SUCCESS
```

---

## Verification

Confirmed:

* Jenkins build completed successfully
* Package installation command executed remotely
* Build can be re-run using different package names

Example:

```text
vim-enhanced
git
wget
tree
```

---

## Architecture

```text
Administrator
      │
      ▼
 Jenkins Job
 (install-packages)
      │
      ▼
 SSH Connection
      │
      ▼
 Storage Server
   (ststor01)
      │
      ▼
 yum install -y PACKAGE
```

---

## Key Learnings

* Jenkins Freestyle Jobs
* Parameterized Builds
* Execute Shell Build Steps
* SSH Automation
* Remote Server Administration
* Infrastructure Automation
* Troubleshooting Build Failures

---

## Real-World Relevance

This pattern is commonly used for:

* Package Management
* Server Provisioning
* Configuration Management
* Patch Deployment
* Infrastructure Operations
* CI/CD Automation

Many organizations use Jenkins to automate operational tasks far beyond application deployments.

---

## Result

✅ Jenkins Job Created

✅ String Parameter Added

✅ Remote SSH Execution Configured

✅ Package Installation Automated

✅ Build Executed Successfully

✅ Troubleshooting Performed

✅ Lab Validation Passed

