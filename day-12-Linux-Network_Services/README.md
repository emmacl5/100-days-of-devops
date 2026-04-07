# Day 12 - Apache Service Troubleshooting (Network & Firewall)

## Task

Investigate and fix an issue where the Apache service on **App Server 1** was not reachable on port `8085` from the jump host.

---

## Server

* App Server 1: `stapp01`

---

## Problem

The monitoring system reported that the Apache service was not accessible on port `8085`.
Attempting to connect from the jump host resulted in:

```bash
curl http://stapp01:8085
```

Error:

```text
No route to host
```

---

## Investigation

### 1. Checked Apache service status

```bash
sudo systemctl status httpd
```

Result:

* Service failed to start initially

---

### 2. Identified port conflict

```bash
sudo ss -tlnp | grep 8085
```

Output showed:

```text
sendmail was using port 8085
```

---

### 3. Checked firewall rules

```bash
sudo iptables -L -n --line-numbers
```

Observed:

* A `REJECT` rule blocking incoming traffic
* No explicit allow rule for port `8085`

---

## Fix Applied

### 1. Stopped conflicting service

```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```

---

### 2. Configured Apache to use port 8085

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Updated:

```apache
Listen 8085
```

---

### 3. Restarted Apache

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

---

### 4. Allowed traffic through firewall

```bash
sudo iptables -I INPUT 1 -p tcp --dport 8085 -j ACCEPT
```

---

## Verification

### Confirm Apache is listening

```bash
sudo ss -tlnp | grep 8085
```

Output:

```text
httpd ... *:8085
```

---

### Test locally

```bash
curl http://localhost:8085
```

---

### Test from jump host

```bash
curl http://stapp01:8085
```

Result:

* Apache service became accessible successfully

---

## Explanation

The issue was caused by **multiple layers of failure**:

1. **Port conflict**

   * `sendmail` was already using port `8085`, preventing Apache from starting

2. **Firewall restriction**

   * `iptables` had a `REJECT` rule blocking external access

3. **Rule ordering issue**

   * Even though general ACCEPT rules existed, the REJECT rule blocked traffic
   * Adding an explicit ACCEPT rule at the top resolved the issue

---

## What I Learned

* How to troubleshoot service failures using `systemctl` and logs
* How to detect port conflicts using `ss`
* How firewall rules and their order affect connectivity
* How to debug “No route to host” errors
* How to restore service availability step-by-step

---

## Real-World Relevance

This scenario mirrors real production incidents where services become unreachable due to:

* port conflicts
* firewall misconfiguration
* incorrect service setup

Quick diagnosis and layered troubleshooting are essential DevOps skills.

---

## Improvement / Automation Idea

* Use monitoring tools to detect port conflicts automatically
* Manage firewall rules using Ansible for consistency
* Implement alerting for service downtime and connectivity issues
* Standardize service ports to avoid conflicts

---

