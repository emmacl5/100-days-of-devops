# Day 84 - Copying Files to Multiple Servers Using Ansible

## Overview

Today I used Ansible to automate copying a file from the Jump Host to multiple application servers in the Stratos Datacenter.

The goal was to configure an inventory containing all application servers and create an Ansible playbook that copies a security file to each server automatically.

---

# Objective

* Create an INI inventory file.
* Add all application servers as managed nodes.
* Create an Ansible playbook.
* Copy the following file:

```text
/usr/src/security/index.html
```

to

```text
/opt/security/index.html
```

on every application server.

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

## Managed Nodes

| Server       | Hostname | User   |
| ------------ | -------- | ------ |
| App Server 1 | stapp01  | tony   |
| App Server 2 | stapp02  | steve  |
| App Server 3 | stapp03  | banner |

---

# Step 1 - Configure Inventory

Created:

```text
/home/thor/ansible/inventory
```

Inventory:

```ini
[app_servers]
stapp01 ansible_user=tony ansible_password=<PASSWORD> ansible_become_password=<PASSWORD>
stapp02 ansible_user=steve ansible_password=<PASSWORD> ansible_become_password=<PASSWORD>
stapp03 ansible_user=banner ansible_password=<PASSWORD> ansible_become_password=<PASSWORD>
```

This inventory defines:

* All application servers
* SSH credentials
* Privilege escalation credentials

---

# Step 2 - Create the Playbook

Created:

```text
/home/thor/ansible/playbook.yml
```

Playbook:

```yaml
---
- hosts: app_servers
  become: yes

  tasks:

    - name: Create destination directory
      file:
        path: /opt/security
        state: directory
        mode: "0755"

    - name: Copy security file
      copy:
        src: /usr/src/security/index.html
        dest: /opt/security/index.html
        mode: "0644"
```

---

# Understanding the Playbook

## Hosts

```yaml
hosts: app_servers
```

Runs the playbook against every server in the `app_servers` inventory group.

---

## Become

```yaml
become: yes
```

Executes tasks with elevated privileges.

---

## File Module

Creates the destination directory if it does not already exist.

```yaml
file:
  path: /opt/security
  state: directory
```

---

## Copy Module

Copies the file from the Jump Host to every managed node.

```yaml
copy:
  src: /usr/src/security/index.html
  dest: /opt/security/index.html
```

---

# Test Connectivity

Verified connectivity using:

```bash
ansible all -i inventory -m ping
```

Expected output:

```text
stapp01 | SUCCESS
stapp02 | SUCCESS
stapp03 | SUCCESS
```

---

# Execute the Playbook

Executed:

```bash
ansible-playbook -i inventory playbook.yml
```

Example recap:

```text
PLAY RECAP

stapp01 : ok=2 changed=2 failed=0
stapp02 : ok=2 changed=2 failed=0
stapp03 : ok=2 changed=2 failed=0
```

---

# Architecture

```text
                Jump Host
                   │
                   ▼
          Ansible Inventory
                   │
      ┌────────────┼────────────┐
      ▼            ▼            ▼
  App Server 1 App Server 2 App Server 3
     stapp01      stapp02      stapp03
      │             │             │
      └─────────────┼─────────────┘
                    ▼
      /opt/security/index.html
```

---

# Key Learnings

* Ansible Inventory Groups
* Managing Multiple Hosts
* File Module
* Copy Module
* YAML Playbooks
* Privilege Escalation
* Configuration Management
* Infrastructure Automation

---

# Real-World Applications

This automation pattern is commonly used for:

* Deploying configuration files
* Distributing application assets
* Rolling out security policies
* Copying SSL certificates
* Deploying static websites
* Managing hundreds or thousands of Linux servers

---

# Result

✅ Inventory Created

✅ Three Application Servers Added

✅ Playbook Created

✅ Destination Directory Created

✅ File Successfully Copied

✅ Playbook Executed Successfully

✅ Lab Validation Passed

---

## Skills Practiced

* Linux
* Ansible
* YAML
* SSH
* Inventory Management
* Configuration Management
* Automation
* DevOps

**#100DaysOfDevOps #Day84**

