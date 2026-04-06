# Day 10 - Linux Bash Script for Website Backup

## Task

Create a bash script to automate backup of a website on **App Server 2** with the following requirements:

* Create a zip archive of `/var/www/html/news`
* Save the archive in `/backup/` on App Server 2
* Copy the archive to the Nautilus Storage Server `/backup/`
* Ensure the script runs without password prompts
* Do not use `sudo` inside the script
* Place the script in `/scripts/news_backup.sh`

---

## Servers Involved

* App Server 2: `stapp02` (user: steve)
* Storage Server: `ststor01` (user: natasha)

---

## Steps Performed

### 1. Created required directories

```bash
sudo mkdir -p /scripts /backup
sudo chown steve:steve /scripts /backup
```

---

### 2. Set up password-less SSH authentication

```bash
ssh-keygen -t rsa -b 2048
ssh-copy-id natasha@ststor01
```

This allows secure, non-interactive file transfer between servers.

---

### 3. Created the backup script

```bash
vi /scripts/news_backup.sh
```

Script content:

```bash
#!/bin/bash
zip -r /backup/xfusioncorp_news.zip /var/www/html/news
scp /backup/xfusioncorp_news.zip natasha@ststor01:/backup/
```

---

### 4. Made the script executable

```bash
chmod 755 /scripts/news_backup.sh
```

---

### 5. Executed the script

```bash
/scripts/news_backup.sh
```

---

## Verification

### On App Server 2:

```bash
ls -l /backup/xfusioncorp_news.zip
```

### On Storage Server:

```bash
ls -l /backup/xfusioncorp_news.zip
```

---

## Explanation

* `zip -r` creates a compressed archive of the website directory
* `/backup/` acts as temporary storage on the application server
* `scp` securely copies the archive to the storage server
* SSH key-based authentication ensures the script runs without password prompts
* The script runs as a regular user (`steve`), avoiding the use of `sudo`

---

## Key Learning

* How to automate backups using bash scripting
* How to transfer files securely between servers using `scp`
* How SSH key-based authentication enables automation
* Importance of correct file ownership and permissions
* Difference between running scripts as root vs normal users

---

## Issue Faced & Fix

### Issue

The script initially prompted for a password during file transfer.

### Root Cause

The script was executed using `sudo`, which caused it to run as `root`, and root did not have SSH keys configured.

### Fix

* Executed the script as the correct user (`steve`)
* Ensured SSH keys were configured for `steve`, not root

---

## Real-World Relevance

This task simulates real DevOps workflows where:

* Application data must be backed up regularly
* Backups are transferred to centralized storage
* Automation must run without manual intervention

---

## Improvement / Automation Idea

* Schedule the script using cron for periodic backups
* Replace script with an Ansible playbook for scalability
* Add logging and error handling to the script
* Use rsync instead of scp for incremental backups

