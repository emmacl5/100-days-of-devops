# Day 86 - Configuring Passwordless SSH for Ansible

## Overview

Today I configured passwordless SSH authentication between the Ansible controller (Jump Host) and App Server 2.

The objective was to ensure Ansible could communicate with the managed node using SSH key-based authentication and successfully execute the Ansible `ping` module.

---

# Objective

* Configure passwordless SSH authentication.
* Use the existing Ansible inventory.
* Verify connectivity from the Jump Host to App Server 2.
* Successfully execute the Ansible `ping` module.

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

## Managed Node

| Server                 | User  |
| ---------------------- | ----- |
| App Server 2 (stapp02) | steve |

---

# Step 1 - Review Inventory

Checked the existing inventory:

```bash
cat inventory
```

Initial inventory:

```ini
stapp01 ansible_ssh_pass=Ir0nM@n
stapp02 ansible_ssh_pass=steve
stapp03 ansible_ssh_pass=BigGr33n
```

Issue:

The inventory forced Ansible to use password authentication for `stapp02`, even though passwordless SSH had already been configured.

---

# Step 2 - Verify Passwordless SSH

Tested direct SSH connectivity:

```bash
ssh steve@stapp02
```

Result:

```text
[steve@stapp02 ~]$
```

This confirmed that SSH key authentication was working correctly.

---

# Step 3 - Update Inventory

Removed the password entry for App Server 2 and specified only the SSH user:

```ini
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n
stapp02 ansible_user=steve
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n
```

This allows Ansible to use the existing SSH key instead of attempting password authentication.

---

# Step 4 - Test Ansible Connectivity

Executed:

```bash
ansible stapp02 -i inventory -m ping
```

Output:

```text
stapp02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

The successful response confirmed that:

* SSH key authentication was used.
* Ansible connected successfully.
* Python was detected automatically on the remote host.

---

# Troubleshooting

## Initial Error

```text
Invalid/incorrect password:
Permission denied
```

Cause:

The inventory explicitly specified an incorrect password:

```ini
ansible_ssh_pass=steve
```

Ansible always prefers the configured password over SSH keys.

---

## Solution

Removed the password configuration and allowed Ansible to authenticate using the existing SSH key pair.

---

# Architecture

```text
          Jump Host (thor)
                 │
                 │ SSH Key Authentication
                 ▼
          App Server 2 (steve)
                 │
                 ▼
         Ansible Ping Module
                 │
                 ▼
             SUCCESS (pong)
```

---

# Key Learnings

* SSH Key Authentication
* Passwordless SSH
* Ansible Inventory Configuration
* Ansible Ping Module
* Managed Nodes
* SSH Authentication Order
* Infrastructure Automation

---

# Real-World Applications

Passwordless SSH is commonly used for:

* Ansible Automation
* CI/CD Pipelines
* Remote Server Management
* Configuration Management
* Infrastructure Provisioning
* Automated Deployments

Using SSH keys is more secure and scalable than storing passwords in inventory files.

---

# Result

✅ Passwordless SSH Verified

✅ Inventory Updated

✅ Ansible Connected Successfully

✅ Ping Module Executed

✅ Managed Node Reachable

✅ Lab Validation Passed

---

## Skills Practiced

* Linux
* SSH
* Public Key Authentication
* Ansible
* Inventory Management
* Remote Administration
* Infrastructure Automation
* DevOps

**#100DaysOfDevOps #Day86 #Ansible #Linux #SSH #Automation #DevOps #InfrastructureAsCode**

