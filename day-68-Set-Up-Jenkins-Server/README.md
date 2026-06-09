# Day 68 - Installing and Configuring Jenkins for CI/CD

## Background

The DevOps team at xFusionCorp Industries decided to use Jenkins as their Continuous Integration and Continuous Delivery (CI/CD) platform.

The objective was to install Jenkins on a dedicated server, troubleshoot startup issues, and configure the initial administrator account.

---

## Objective

Install and configure Jenkins with the following requirements:

### Jenkins Installation

* Install Jenkins using the `apt` package manager
* Start Jenkins using the `service` command

### Administrator Account

| Setting   | Value                                                                                 |
| --------- | ------------------------------------------------------------------------------------- |
| Username  | theadmin                                                                              |
| Password  | Adm!n321                                                                              |
| Full Name | Javed                                                                                 |
| Email     | [javed@jenkins.stratos.xfusioncorp.com](mailto:javed@jenkins.stratos.xfusioncorp.com) |

---

## Environment

### Access Jenkins Server

From the jump host:

```bash id="lf3f6x"
ssh root@jenkins
```

Password:

```text id="on3e3v"
S3curePass
```

---

## Installation

### Update Package Repository

```bash id="qct8h6"
apt update
```

### Install Jenkins

```bash id="v0zqqf"
apt install -y jenkins
```

---

## Troubleshooting

### Initial Startup Failure

Attempting to start Jenkins:

```bash id="mhvm9n"
service jenkins start
```

Result:

```text id="cr5v97"
Failed to start Jenkins
```

### Check Service Status

```bash id="1pf0sh"
service jenkins status
```

### Review Jenkins Logs

```bash id="ttjwej"
tail -n 50 /var/log/jenkins/jenkins.log
```

Error:

```text id="hnm8nz"
Running with Java 17...
minimum required version is Java 21
```

---

## Resolution

The installed Jenkins version required Java 21.

Install Java 21:

```bash id="vgbp7w"
apt install -y openjdk-21-jre
```

Verify:

```bash id="b0g4zt"
java -version
```

Expected:

```text id="az2lhy"
openjdk version "21"
```

---

## Start Jenkins

```bash id="5nj8e6"
service jenkins start
```

Verify:

```bash id="ggmyj5"
service jenkins status
```

Expected:

```text id="m9cwnh"
jenkins is running
```

---

## Jenkins Initial Setup

Retrieve unlock password:

```bash id="xz0z5w"
cat /var/lib/jenkins/secrets/initialAdminPassword
```

Open Jenkins web interface and:

1. Enter unlock password
2. Install suggested plugins
3. Create administrator account

---

## Administrator Configuration

Configured:

```text id="wm4h11"
Username: theadmin
Password: Adm!n321
Full Name: Javed
Email: javed@jenkins.stratos.xfusioncorp.com
```

Successfully logged into Jenkins dashboard.

---

## Verification

Verify service:

```bash id="n0s0m3"
service jenkins status
```

Verify web access:

* Jenkins dashboard accessible
* Admin login successful

---

## Architecture

```text id="m4rjvh"
Developer
    │
    ▼
 Jenkins Server
    │
    ├── Build Jobs
    ├── Pipelines
    ├── Plugins
    ├── Source Control Integration
    └── Automated Deployments
```

---

## Key Learnings

* Installing Jenkins using apt
* Managing Linux services
* Reading service logs
* Troubleshooting Java dependency issues
* Configuring Jenkins administrators
* Understanding Jenkins startup requirements
* Initial CI/CD platform setup

---

## Real-World Relevance

Jenkins is one of the most widely used CI/CD platforms for:

* Application Builds
* Automated Testing
* Infrastructure Automation
* Continuous Delivery
* Continuous Deployment
* DevOps Pipelines

A large percentage of DevOps environments still use Jenkins in some capacity.

---

## Result

✅ Jenkins Installed

✅ Java Compatibility Issue Resolved

✅ Jenkins Service Running

✅ Jenkins Web UI Accessible

✅ Administrator Account Created

✅ Login Verified

✅ Lab Validation Passed


