# Day 82 - Creating an Ansible Inventory for Automated Server Configuration

## Overview

Today I worked on configuring an Ansible inventory file to enable automated management of App Server 2 in the Nautilus infrastructure.

The objective was to create an INI-style inventory file that allows an existing Ansible playbook to connect to App Server 2 and execute tasks without requiring additional command-line arguments.

---

## Objective

Create an Ansible inventory file:

```text
/home/thor/playbook/inventory
```

Requirements:

* Use INI inventory format.
* Add App Server 2 (`stapp02`) to the inventory.
* Configure authentication variables.
* Ensure Ansible can execute the provided playbook successfully.
* Allow validation to run using:

```bash
ansible-playbook -i inventory playbook.yml
```

without additional parameters.

---

## Environment

### Jump Host

User:

```text
thor
```

Working Directory:

```text
/home/thor/playbook
```

### Target Server

Hostname:

```text
stapp02
```

User:

```text
steve
```

---

## Step 1: Verify App Server 2 Hostname

Checked the hostname using:

```bash
getent hosts stapp02
```

Output:

```text
10.244.195.50 stapp02.f5xxbmxtjgxkur7b.svc.cluster.local
```

Confirmed hostname:

```text
stapp02
```

---

## Step 2: Review the Playbook

Examined the existing playbook:

```bash
cat playbook.yml
```

Playbook:

```yaml
---
- hosts: all
  become: yes
  become_user: root

  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: installed

    - name: Start service httpd
      service:
        name: httpd
        state: started
```

Observations:

* Targets all hosts.
* Uses privilege escalation (`become: yes`).
* Requires SSH authentication.
* Requires sudo privileges.

---

## Step 3: Create Inventory File

Created:

```text
/home/thor/playbook/inventory
```

Content:

```ini
[app]
stapp02 ansible_user=steve ansible_password=<PASSWORD> ansible_become_password=<PASSWORD>
```

Purpose:

* Define target host.
* Define SSH credentials.
* Define privilege escalation credentials.

---

## Step 4: Test Connectivity

Executed:

```bash
ansible all -i inventory -m ping
```

Output:

```text
stapp02 | SUCCESS => {
    "ping": "pong"
}
```

Result:

✅ Ansible successfully connected to App Server 2.

---

## Step 5: Execute the Playbook

Ran:

```bash
ansible-playbook -i inventory playbook.yml
```

Output:

```text
PLAY [all]

TASK [Gathering Facts]
ok: [stapp02]

TASK [Install httpd package]
changed: [stapp02]

TASK [Start service httpd]
changed: [stapp02]

PLAY RECAP

stapp02 : ok=3 changed=2 unreachable=0 failed=0
```

Result:

✅ Apache HTTP Server installed.

✅ Apache service started.

✅ Playbook executed successfully.

---

## Inventory Structure

```ini
[app]
stapp02 ansible_user=steve ansible_password=<PASSWORD> ansible_become_password=<PASSWORD>
```

---

## Architecture

```text
Jump Host (thor)
        │
        ▼
Ansible Inventory
        │
        ▼
App Server 2 (stapp02)
        │
        ▼
Install httpd
        │
        ▼
Start httpd Service
```

---

## Key Learnings

* Ansible Inventory Configuration
* INI Inventory Format
* Host Variables
* SSH Authentication
* Privilege Escalation
* Ansible Connectivity Testing
* Ansible Playbook Execution

---

## Real-World Relevance

Ansible inventories are used to:

* Manage Linux servers at scale.
* Organize infrastructure into groups.
* Store connection parameters.
* Define environment-specific variables.
* Automate configuration management.

Examples:

* Web Servers
* Database Servers
* Kubernetes Nodes
* Application Servers
* Cloud Infrastructure

---

## Result

✅ Inventory File Created

✅ App Server 2 Added

✅ Authentication Variables Configured

✅ Ansible Connectivity Verified

✅ Playbook Executed Successfully

✅ Apache Installed

✅ Apache Service Started

✅ Lab Validation Passed

#100DaysOfDevOps #Day82 #Ansible #Automation #Linux #DevOps

