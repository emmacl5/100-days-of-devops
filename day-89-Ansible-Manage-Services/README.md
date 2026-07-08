# Day 89 - Installing and Managing VSFTPD with Ansible

## Overview

Today I automated the installation and configuration of the **VSFTPD (Very Secure FTP Daemon)** service on multiple Linux application servers using Ansible.

The objective was to install the package, ensure the service was started, configure it to start automatically at boot, and verify the deployment across all managed nodes.

---

# Objective

* Create an Ansible playbook.
* Install the **vsftpd** package on all application servers.
* Start the VSFTPD service.
* Enable the service to start automatically on boot.
* Execute the playbook using the existing inventory.

---

# Environment

## Ansible Controller

| Server    | User |
| --------- | ---- |
| Jump Host | thor |

Working directory:

```text
/home/thor/ansible
```

---

## Managed Nodes

| Server       | Hostname |
| ------------ | -------- |
| App Server 1 | stapp01  |
| App Server 2 | stapp02  |
| App Server 3 | stapp03  |

---

# Step 1 - Use Existing Inventory

The inventory file was already available:

```text
/home/thor/ansible/inventory
```

The playbook targeted all managed application servers.

---

# Step 2 - Create the Playbook

Created:

```text
/home/thor/ansible/playbook.yml
```

Playbook:

```yaml
---
- hosts: all
  become: yes

  tasks:

    - name: Install vsftpd
      yum:
        name: vsftpd
        state: present

    - name: Start and enable vsftpd
      service:
        name: vsftpd
        state: started
        enabled: yes
```

---

# Understanding the Playbook

## Install Package

```yaml
yum:
  name: vsftpd
  state: present
```

The `yum` module ensures the VSFTPD package is installed on every managed node.

Because Ansible is idempotent, running the playbook multiple times does not reinstall an already installed package.

---

## Start and Enable the Service

```yaml
service:
  name: vsftpd
  state: started
  enabled: yes
```

This task:

* Starts the VSFTPD service.
* Configures it to start automatically after every system reboot.

---

# Execute the Playbook

Run:

```bash
ansible-playbook -i inventory playbook.yml
```

Example output:

```text
PLAY RECAP

stapp01 : ok=3 changed=2 failed=0
stapp02 : ok=3 changed=2 failed=0
stapp03 : ok=3 changed=2 failed=0
```

---

# Verification

Verify the package:

```bash
ansible all -i inventory -a "rpm -q vsftpd"
```

Example:

```text
vsftpd-3.x.x
```

Verify the service:

```bash
ansible all -i inventory -m shell -a "systemctl is-enabled vsftpd && systemctl is-active vsftpd"
```

Output:

```text
enabled
active
```

This confirms the service is:

* Installed
* Running
* Enabled at boot

---

# Architecture

```text
                 Jump Host
            (Ansible Controller)
                     │
                     ▼
             Ansible Playbook
                     │
      ┌──────────────┼──────────────┐
      ▼              ▼              ▼
   App Server 1   App Server 2   App Server 3
      │              │              │
 Install VSFTPD  Install VSFTPD  Install VSFTPD
      │              │              │
 Start Service   Start Service   Start Service
      │              │              │
 Enable at Boot  Enable at Boot  Enable at Boot
```

---

# Key Learnings

* Ansible Playbooks
* `yum` Module
* `service` Module
* Linux Service Management
* Package Management
* Multi-Host Automation
* Idempotent Configuration
* Infrastructure as Code

---

# Real-World Applications

Automating package installation and service management is a common DevOps task used for:

* FTP server deployment
* Infrastructure provisioning
* Application dependency installation
* Standardized server configuration
* Automated environment setup
* Large-scale configuration management

---

# Result

✅ VSFTPD Installed

✅ Service Started

✅ Service Enabled at Boot

✅ Playbook Executed Successfully

✅ Verified on All Application Servers

✅ Lab Validation Passed

---

## Skills Practiced

* Linux
* Ansible
* YAML
* VSFTPD
* Package Management
* Service Management
* Infrastructure Automation
* DevOps

**#100DaysOfDevOps #Day89 #Ansible #Linux #Automation #DevOps #YAML #ConfigurationManagement #VSFTPD**

