# Day 87 - Installing Packages on Multiple Servers Using Ansible

## Overview

Today I automated the installation of the Apache HTTP Server (`httpd`) on multiple application servers using Ansible.

The objective was to create an inventory containing all application servers and develop an Ansible playbook that installs the `httpd` package using the `yum` module.

---

# Objective

* Create an Ansible inventory.
* Add all application servers as managed nodes.
* Create an Ansible playbook.
* Install the `httpd` package on all application servers.
* Ensure the playbook can be executed by the `thor` user without additional arguments.

---

# Environment

## Ansible Controller

| Server    | User |
| --------- | ---- |
| Jump Host | thor |

Working directory:

```text
/home/thor/playbook
```

---

## Managed Nodes

| Server       | Hostname | User   |
| ------------ | -------- | ------ |
| App Server 1 | stapp01  | tony   |
| App Server 2 | stapp02  | steve  |
| App Server 3 | stapp03  | banner |

---

# Step 1 - Create the Inventory

Created:

```text
/home/thor/playbook/inventory
```

Inventory:

```ini
[app_servers]
stapp01 ansible_user=tony ansible_ssh_pass=<TONY_PASSWORD>
stapp02 ansible_user=steve ansible_ssh_pass=<STEVE_PASSWORD>
stapp03 ansible_user=banner ansible_ssh_pass=<BANNER_PASSWORD>
```

---

# Step 2 - Create the Playbook

Created:

```text
/home/thor/playbook/playbook.yml
```

Playbook:

```yaml
---
- hosts: app_servers
  become: yes

  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: present
```

---

# Understanding the Playbook

## Hosts

```yaml
hosts: app_servers
```

Targets every server in the inventory group.

---

## Become

```yaml
become: yes
```

Executes the installation with elevated privileges.

---

## Yum Module

```yaml
yum:
  name: httpd
  state: present
```

The `yum` module:

* Installs the package if it is missing.
* Leaves it unchanged if already installed.
* Ensures idempotent execution.

---

# Test Connectivity

Verified connectivity before deployment:

```bash
ansible all -i inventory -m ping
```

Result:

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

Output:

```text
PLAY RECAP

stapp01 : ok=2 changed=1 failed=0
stapp02 : ok=2 changed=1 failed=0
stapp03 : ok=2 changed=1 failed=0
```

---

# Verification

Verified package installation:

```bash
ansible all -i inventory -m shell -a "rpm -q httpd"
```

Output:

```text
stapp01
httpd-2.4.62-14.el9.x86_64

stapp02
httpd-2.4.62-14.el9.x86_64

stapp03
httpd-2.4.62-14.el9.x86_64
```

This confirmed that Apache HTTP Server was successfully installed on all application servers.

---

# Architecture

```text
               Jump Host (thor)
                      │
                      ▼
             Ansible Inventory
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
    stapp01       stapp02       stapp03
        │             │             │
        └─────────────┼─────────────┘
                      ▼
          Install httpd using yum
```

---

# Key Learnings

* Ansible Inventory Management
* YAML Playbooks
* Yum Module
* Package Management
* Multi-Host Automation
* Privilege Escalation
* Infrastructure Automation
* Idempotent Configuration

---

# Real-World Applications

This automation pattern is commonly used to:

* Provision new Linux servers
* Install web servers
* Deploy application dependencies
* Standardize server configurations
* Automate infrastructure setup
* Reduce manual administration tasks

---

# Result

✅ Inventory Created

✅ All Application Servers Added

✅ Playbook Created

✅ Apache HTTP Server Installed

✅ Package Verified on All Servers

✅ Playbook Executed Successfully

✅ Lab Validation Passed

---

## Skills Practiced

* Linux
* Ansible
* YAML
* Yum Package Management
* SSH
* Infrastructure Automation
* Configuration Management
* DevOps

**#100DaysOfDevOps #Day87 #Ansible #Linux #Automation #DevOps #YAML #ConfigurationManagement**

