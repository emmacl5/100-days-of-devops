# Day 85 - Managing File Ownership and Permissions with Ansible

## Overview

Today I used Ansible to automate the creation and configuration of a file across multiple Linux servers.

The objective was to create a blank file on every application server while assigning different ownership based on each target host and enforcing the required file permissions.

---

# Objective

* Create an Ansible inventory containing all application servers.
* Create an Ansible playbook.
* Create the file:

```text
/tmp/appdata.txt
```

on all application servers.

* Configure the file permissions to:

```text
0655
```

* Assign ownership as follows:

| Server       | Owner  | Group  |
| ------------ | ------ | ------ |
| App Server 1 | tony   | tony   |
| App Server 2 | steve  | steve  |
| App Server 3 | banner | banner |

---

# Environment

## Jump Host

User:

```text
thor
```

Working Directory:

```text
~/playbook
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
~/playbook/inventory
```

Inventory:

```ini
[app_servers]
stapp01 ansible_user=tony ansible_password=<PASSWORD> ansible_become_password=<PASSWORD>
stapp02 ansible_user=steve ansible_password=<PASSWORD> ansible_become_password=<PASSWORD>
stapp03 ansible_user=banner ansible_password=<PASSWORD> ansible_become_password=<PASSWORD>
```

---

# Step 2 - Create the Playbook

Created:

```text
~/playbook/playbook.yml
```

Playbook:

```yaml
---
- hosts: app_servers
  become: yes

  tasks:

    - name: Create blank file
      file:
        path: /tmp/appdata.txt
        state: touch

    - name: Configure App Server 1
      file:
        path: /tmp/appdata.txt
        owner: tony
        group: tony
        mode: "0655"
      when: inventory_hostname == "stapp01"

    - name: Configure App Server 2
      file:
        path: /tmp/appdata.txt
        owner: steve
        group: steve
        mode: "0655"
      when: inventory_hostname == "stapp02"

    - name: Configure App Server 3
      file:
        path: /tmp/appdata.txt
        owner: banner
        group: banner
        mode: "0655"
      when: inventory_hostname == "stapp03"
```

---

# Understanding the Playbook

## Hosts

```yaml
hosts: app_servers
```

Executes the playbook on every server in the inventory group.

---

## Become

```yaml
become: yes
```

Allows tasks to execute with elevated privileges.

---

## File Module

Used to:

* Create files
* Modify permissions
* Change ownership
* Manage directories

---

## Conditional Tasks

Each ownership task is executed only for its corresponding server.

Example:

```yaml
when: inventory_hostname == "stapp01"
```

This ensures:

* `tony` owns the file on App Server 1.
* `steve` owns the file on App Server 2.
* `banner` owns the file on App Server 3.

---

# Test Connectivity

Verified all managed nodes:

```bash
ansible all -i inventory -m ping
```

Output:

```text
stapp01 | SUCCESS
stapp02 | SUCCESS
stapp03 | SUCCESS
```

---

# Execute the Playbook

Ran:

```bash
ansible-playbook -i inventory playbook.yml
```

Example output:

```text
PLAY RECAP

stapp01 : ok=4 changed=3 failed=0
stapp02 : ok=4 changed=3 failed=0
stapp03 : ok=4 changed=3 failed=0
```

---

# Verification

Verified the file on all application servers:

```bash
ansible all -i inventory -a "ls -l /tmp/appdata.txt"
```

Expected:

```text
-rw-r-xr-x
```

Permission:

```text
0655
```

Ownership:

* App Server 1 → `tony:tony`
* App Server 2 → `steve:steve`
* App Server 3 → `banner:banner`

---

# Architecture

```text
                 Jump Host
                    │
                    ▼
           Ansible Inventory
                    │
     ┌──────────────┼──────────────┐
     ▼              ▼              ▼
  stapp01        stapp02        stapp03
    │               │               │
Create /tmp/appdata.txt on each server
    │               │               │
Owner: tony     Owner: steve    Owner: banner
Mode: 0655      Mode: 0655      Mode: 0655
```

---

# Key Learnings

* Ansible Inventory Management
* YAML Playbooks
* File Module
* Conditional Task Execution
* `when` Statements
* File Permissions
* File Ownership
* Privilege Escalation

---

# Real-World Applications

This approach is commonly used to:

* Configure application files
* Deploy configuration files
* Manage user-specific resources
* Apply server-specific settings
* Automate Linux administration
* Enforce security policies across infrastructure

---

# Result

✅ Inventory Created

✅ Three Servers Added

✅ Blank File Created

✅ Permissions Set to **0655**

✅ Correct Owner Assigned on Each Server

✅ Conditional Tasks Executed Successfully

✅ Playbook Executed Successfully

✅ Lab Validation Passed

---

## Skills Practiced

* Linux
* Ansible
* YAML
* SSH
* File Management
* Conditional Logic
* Configuration Management
* Infrastructure Automation
* DevOps

**#100DaysOfDevOps #Day85**

