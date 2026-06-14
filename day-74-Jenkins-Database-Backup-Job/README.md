# Day 74 - Automating MySQL Database Backups with Jenkins

## Background

The xFusionCorp DevOps team needed an automated solution to back up a production database regularly.

To avoid manual backups and reduce operational risk, a Jenkins job was created to:

* Generate a database dump
* Name it using the current date
* Transfer it to a centralized backup location
* Execute automatically on a schedule

---

## Objective

Create a Jenkins job named:

```text
database-backup
```

Requirements:

### Database Details

Server:

```text
stapp01
```

Database:

```text
kodekloud_db01
```

Database User:

```text
kodekloud_roy
```

Password:

```text
asdfgdsd
```

---

### Backup Naming Convention

```text
db_$(date +%F).sql
```

Example:

```text
db_2026-06-11.sql
```

---

### Backup Destination

Storage Server:

```text
ststor01
```

Target Directory:

```text
/home/natasha/db_backups
```

---

### Schedule

Run every:

```text
10 minutes
```

Cron Expression:

```text
*/10 * * * *
```

---

## Solution

### Step 1 - Create Jenkins Job

Created a Freestyle Project:

```text
database-backup
```

---

### Step 2 - Configure Build Trigger

Enabled:

```text
Build periodically
```

Cron:

```text
*/10 * * * *
```

---

### Step 3 - Configure Build Step

Added:

```text
Execute Shell
```

Shell Script:

```bash
BACKUP_FILE=db_$(date +%F).sql

sshpass -p 'Thor123' ssh -o StrictHostKeyChecking=no tony@stapp01 \
"mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > /tmp/${BACKUP_FILE}"

sshpass -p 'Thor123' scp -o StrictHostKeyChecking=no \
tony@stapp01:/tmp/${BACKUP_FILE} /tmp/${BACKUP_FILE}

sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 \
"mkdir -p /home/natasha/db_backups"

sshpass -p 'Bl@kW' scp -o StrictHostKeyChecking=no \
/tmp/${BACKUP_FILE} natasha@ststor01:/home/natasha/db_backups/
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
ls -l /home/natasha/db_backups
```

Verified:

```text
db_YYYY-MM-DD.sql
```

Example:

```text
db_2026-06-11.sql
```

Backup file successfully copied.

---

## Architecture

```text
MySQL Database
   (stapp01)
        │
        ▼
   mysqldump
        │
        ▼
     Jenkins
        │
        ▼
 Storage Server
   (ststor01)
        │
        ▼
/home/natasha/db_backups
```

---

## Key Learnings

* Jenkins Scheduled Jobs
* Database Backup Automation
* mysqldump Usage
* SSH Automation
* SCP File Transfers
* Backup Retention Foundations
* Infrastructure Operations

---

## Real-World Relevance

Automated database backups are critical for:

* Disaster Recovery
* Business Continuity
* Data Protection
* Compliance Requirements
* Incident Response

Production environments typically automate backups through:

* Jenkins
* Cron Jobs
* Ansible
* Cloud Backup Services
* Enterprise Backup Solutions

---

## Result

✅ Jenkins Job Created

✅ Scheduled Trigger Configured

✅ Database Dump Automated

✅ Backup Naming Standard Implemented

✅ Backup Copied to Storage Server

✅ Manual Execution Verified

✅ Lab Validation Passed

