# Day 14 - Linux Process Troubleshooting

## Task

Identify the faulty app server where Apache was unavailable, fix the issue, and ensure Apache is running on port `5002` on all app servers.

---

## Servers

* App Server 1: `stapp01`
* App Server 2: `stapp02`
* App Server 3: `stapp03`

---

## Problem

The monitoring system reported Apache service unavailability on one of the app servers.

---

## Investigation

### 1. Checked Apache service status on all app servers

```bash
sudo systemctl status httpd
```

### 2. Checked Apache port configuration

```bash
sudo grep -n '^Listen' /etc/httpd/conf/httpd.conf
```

### 3. Checked listening processes on port 5002

```bash
sudo ss -tlnp | grep 5002
```

---

## Issue Identified

On `stapp01`, Apache failed to start because port `5002` was already in use.

```text
Address already in use: could not bind to address 0.0.0.0:5002
```

Further investigation showed that `sendmail` was listening on port `5002`:

```bash
sudo ss -tlnp | grep 5002
```

Output:

```text
LISTEN 0 10 127.0.0.1:5002 ... sendmail
```

---

## Fix Applied

### 1. Stopped and disabled the conflicting service

```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```

### 2. Started and enabled Apache

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

### 3. Ensured Apache was configured on port 5002

```bash
sudo sed -i 's/^Listen .*/Listen 5002/' /etc/httpd/conf/httpd.conf
sudo systemctl restart httpd
```

---

## Verification

### Checked Apache status

```bash
sudo systemctl status httpd
```

### Confirmed Apache is listening on port 5002

```bash
sudo ss -tlnp | grep 5002
```

Expected:

```text
httpd ... *:5002
```

---

## Explanation

Apache was unavailable because another service (`sendmail`) was already using the required port `5002`. After removing the conflict and restarting Apache, the service started successfully. The same port configuration was then verified on all app servers.

---

## What I Learned

* how to troubleshoot service startup failures
* how to identify port conflicts using `ss`
* how to resolve process conflicts between services
* how to verify service availability and listening ports

---

## Real-World Relevance

Port conflicts are a common cause of service outages in production systems. Quickly identifying the conflicting process and restoring the intended service is an important DevOps troubleshooting skill.

---

## Improvement / Automation Idea

This type of validation can be automated with monitoring and configuration management tools to detect conflicts early and enforce consistent service ports.

