# Day 83 - Creating an Ansible Playbook to Manage Files

## Overview

Today I configured an Ansible inventory and created a simple Ansible playbook to automate file creation on a remote Linux server.

The objective was to update the inventory so that the playbook targets App Server 3 and then create an empty file on the remote host using Ansible.

---

# Objective

* Configure an INI inventory file.
* Target App Server 3 (`stapp03`).
* Create an Ansible playbook.
* Create an empty file:

```text
/tmp/file.txt
```

* Ensure the playbook runs successfully without additional arguments.

---

# Environment

## Jump Host

User:

```text
thor
```

Working Directory:

```text
/home/thor/ansible
```

---

## Target Server

Hostname:

```text
stapp03
```

Remote User:

```text
banner
```

---

# Step 1 - Configure Inventory

Updated:

```text
/home/thor/ansible/inventory
```

Inventory:

```ini
[app]
stapp03 ansible_user=banner ansible_password=<PASSWORD> ansible_become_password=<PASSWORD>
```

This inventory provides:

* Target host
* SSH credentials
* Sudo (become) credentials

---

# Step 2 - Create the Playbook

Created:

```text
/home/thor/ansible/playbook.yml
```

Contents:

```yaml
---
- hosts: all
  become: yes

  tasks:
    - name: Create empty file
      file:
        path: /tmp/file.txt
        state: touch
```

---

# Understanding the Playbook

## Hosts

```yaml
hosts: all
```

Targets every host defined in the inventory.

---

## Become

```yaml
become: yes
```

Executes tasks with elevated privileges.

---

## File Module

The `file` module manages files and directories.

Task:

```yaml
file:
  path: /tmp/file.txt
  state: touch
```

Result:

* Creates the file if it doesn't exist.
* Updates the timestamp if it already exists.

---

# Testing Connectivity

Verified connectivity:

```bash
ansible all -i inventory -m ping
```

Output:

```text
stapp03 | SUCCESS
```

---

# Execute the Playbook

Ran:

```bash
ansible-playbook -i inventory playbook.yml
```

Output:

```text
PLAY RECAP

stapp03 : ok=2 changed=1 failed=0
```

---

# Verification

Confirmed that:

```text
/tmp/file.txt
```

was successfully created on App Server 3.

---

# Architecture

```text
Jump Host (thor)
        │
        ▼
 Ansible Inventory
        │
        ▼
    App Server 3
      (stapp03)
        │
        ▼
Create /tmp/file.txt
```

---

# Key Learnings

* Ansible Inventory
* INI Inventory Format
* Ansible Playbooks
* YAML Syntax
* File Module
* Privilege Escalation
* Infrastructure Automation

---

# Real-World Applications

This automation pattern is commonly used for:

* Server provisioning
* Configuration management
* Log file creation
* Configuration file deployment
* Automated software installation
* Linux server administration

---

# Result

✅ Inventory Updated

✅ App Server 3 Added

✅ Playbook Created

✅ Empty File Created

✅ Playbook Executed Successfully

✅ Lab Validation Passed

---

## Skills Practiced

* Linux
* Ansible
* YAML
* SSH
* Configuration Management
* Infrastructure Automation
* DevOps

**#100DaysOfDevOps #Day83**

