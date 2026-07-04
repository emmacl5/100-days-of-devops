# Day 88 - Deploying an Apache Web Server with Ansible

## Overview

Today I automated the deployment and configuration of an Apache HTTP Server across multiple Linux application servers using Ansible.

The objective was to install the Apache package, ensure the service was running, deploy a sample web page using the `blockinfile` module, and configure the required ownership and permissions.

---

# Objective

* Install the Apache HTTP Server (`httpd`) on all application servers.
* Start and enable the Apache service.
* Create and populate `/var/www/html/index.html`.
* Use the Ansible `blockinfile` module to manage the page content.
* Configure the file owner and group as `apache`.
* Set file permissions to `0755`.

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

The provided inventory file was located at:

```text
/home/thor/ansible/inventory
```

The playbook targets all application servers defined in this inventory.

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

    - name: Install httpd
      yum:
        name: httpd
        state: present

    - name: Start and enable httpd
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create index.html
      file:
        path: /var/www/html/index.html
        state: touch
        owner: apache
        group: apache
        mode: "0755"

    - name: Add web page content
      blockinfile:
        path: /var/www/html/index.html
        block: |
          Welcome to XfusionCorp!

          This is Nautilus sample file, created using Ansible!

          Please do not modify this file manually!

    - name: Configure ownership and permissions
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode: "0755"
```

---

# Understanding the Playbook

## Install Apache

```yaml
yum:
  name: httpd
  state: present
```

Ensures Apache is installed on every application server.

---

## Start the Service

```yaml
service:
  name: httpd
  state: started
  enabled: yes
```

Starts the web server and configures it to start automatically on boot.

---

## Create the Web Page

The `file` module creates the web page if it does not already exist and applies the required ownership and permissions.

---

## Deploy Content with blockinfile

The `blockinfile` module inserts a managed block into the HTML file.

Content deployed:

```text
Welcome to XfusionCorp!

This is Nautilus sample file, created using Ansible!

Please do not modify this file manually!
```

The default Ansible markers were used, as required by the lab.

---

## Configure Ownership

```text
Owner: apache
Group: apache
Permissions: 0755
```

This ensures Apache can serve the file correctly while maintaining the expected permissions.

---

# Execute the Playbook

Run:

```bash
ansible-playbook -i inventory playbook.yml
```

Example output:

```text
PLAY RECAP

stapp01 : ok=5 changed=4 failed=0
stapp02 : ok=5 changed=4 failed=0
stapp03 : ok=5 changed=4 failed=0
```

---

# Verification

Verify Apache:

```bash
ansible all -i inventory -a "systemctl status httpd --no-pager"
```

Verify the deployed page:

```bash
ansible all -i inventory -a "cat /var/www/html/index.html"
```

Verify permissions:

```bash
ansible all -i inventory -a "ls -l /var/www/html/index.html"
```

Expected:

```text
-rwxr-xr-x 1 apache apache ...
```

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
   stapp01        stapp02        stapp03
      │              │              │
 Install httpd   Install httpd  Install httpd
      │              │              │
 Start Service   Start Service  Start Service
      │              │              │
 Deploy index.html with blockinfile
      │              │              │
 Set apache ownership and 0755 permissions
```

---

# Key Learnings

* Ansible Playbooks
* `yum` Module
* `service` Module
* `file` Module
* `blockinfile` Module
* File Ownership
* File Permissions
* Service Management
* Multi-Host Automation
* Infrastructure as Code

---

# Real-World Applications

This automation pattern is commonly used for:

* Web server provisioning
* Static website deployment
* Server configuration management
* Automated infrastructure setup
* CI/CD deployment pipelines
* Standardized application environments

---

# Result

✅ Apache Installed

✅ Service Started and Enabled

✅ Web Page Created

✅ Content Managed with `blockinfile`

✅ Owner Set to `apache`

✅ Permissions Set to `0755`

✅ Playbook Executed Successfully

✅ Lab Validation Passed

---

## Skills Practiced

* Linux
* Ansible
* YAML
* Apache HTTP Server
* Configuration Management
* Infrastructure Automation
* DevOps

**#100DaysOfDevOps #Day88 #Ansible #Apache #Linux #Automation #InfrastructureAsCode #DevOps**

