# Day 73 - Automating Apache Log Collection with Jenkins

## Background

The xFusionCorp DevOps team is preparing to implement a centralized logging solution.

While the full logging platform is still under development, the team needs a temporary automated mechanism to collect Apache logs from application servers and store them centrally for troubleshooting and analysis.

A Jenkins job was created to periodically copy Apache logs from App Server 2 to the Storage Server.

---

## Objective

Create a Jenkins job named:

```text
copy-logs
```

Requirements:

### Source Server

```text
stapp02
```

Apache log files:

```text
/var/log/httpd/access_log
/var/log/httpd/error_log
```

### Destination Server

```text
ststor01
```

Destination directory:

```text
/usr/src/data
```

### Scheduling

Run automatically every:

```text
6 minutes
```

### Validation

Run the job manually at least once and verify that both log files are copied successfully.

---

## Solution

### Step 1 - Create Jenkins Job

Created a Freestyle Project:

```text
copy-logs
```

---

### Step 2 - Configure Build Trigger

Enabled:

```text
Build periodically
```

Cron schedule:

```text
*/6 * * * *
```

Meaning:

```text
Every 6 minutes
```

---

### Step 3 - Configure Build Step

Added:

```text
Execute Shell
```

Script:

```bash
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 "mkdir -p /usr/src/data"

sshpass -p 'Steve_pass123' scp -o StrictHostKeyChecking=no steve@stapp02:/var/log/httpd/access_log /tmp/access_log

sshpass -p 'Steve_pass123' scp -o StrictHostKeyChecking=no steve@stapp02:/var/log/httpd/error_log /tmp/error_log

sshpass -p 'Bl@kW' scp -o StrictHostKeyChecking=no /tmp/access_log natasha@ststor01:/usr/src/data/access_log

sshpass -p 'Bl@kW' scp -o StrictHostKeyChecking=no /tmp/error_log natasha@ststor01:/usr/src/data/error_log
```

---

## Build Execution

Triggered:

```text
Build Now
```

Build Result:

```text
SUCCESS
```

---

## Verification

Connected to Storage Server:

```bash
ssh natasha@ststor01
```

Checked:

```bash
ls -l /usr/src/data
```

Verified:

```text
access_log
error_log
```

Both Apache log files were successfully copied.

---

## Architecture

```text
Apache Server (stapp02)
        │
        │ access_log
        │ error_log
        ▼
      Jenkins
        │
        ▼
 Storage Server (ststor01)
        │
        ▼
   /usr/src/data
```

---

## Key Learnings

* Jenkins Scheduled Jobs
* Cron Expressions
* Build Triggers
* SSH Automation
* SCP File Transfers
* Log Collection Automation
* Infrastructure Monitoring Foundations

---

## Real-World Relevance

This pattern is commonly used when:

* Centralized logging platforms are not yet available
* Temporary log aggregation is needed
* Teams require historical logs for troubleshooting
* Operational data must be collected automatically

Modern alternatives include:

* ELK Stack
* OpenSearch
* Grafana Loki
* Splunk
* Fluentd
* Fluent Bit

---

## Result

✅ Jenkins Job Created

✅ Scheduled Execution Configured

✅ Apache Logs Collected

✅ Logs Copied to Storage Server

✅ Manual Build Successful

✅ Validation Passed

✅ Lab Completed Successfully

