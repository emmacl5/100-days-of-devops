# Day 90 - Managing File ACLs with Ansible

# Overview

Today I automated file creation and Access Control List (ACL) management across multiple Linux application servers using Ansible.

The objective was to create different files on each application server, ensure they were owned by `root`, and assign specific ACL permissions to different users and groups using the `ansible.posix.acl` module.

---

# Objective

* Create files on different application servers.
* Ensure every file is owned by `root`.
* Configure Linux Access Control Lists (ACLs).
* Grant host-specific permissions using Ansible.

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

| Server       | Hostname | File                  |
| ------------ | -------- | --------------------- |
| App Server 1 | stapp01  | `/opt/data/blog.txt`  |
| App Server 2 | stapp02  | `/opt/data/story.txt` |
| App Server 3 | stapp03  | `/opt/data/media.txt` |

---

# Step 1 - Use Existing Inventory

The inventory file already existed:

```text
/home/thor/ansible/inventory
```

The playbook targeted all managed nodes.

---

# Step 2 - Create the Playbook

Created:

```text
/home/thor/ansible/playbook.yml
```

Playbook:

```yaml
---
- name: Configure ACLs on application servers
  hosts: all
  become: yes

  tasks:

    - name: Install ACL package
      yum:
        name: acl
        state: present

    - name: Ensure /opt/data exists
      file:
        path: /opt/data
        state: directory
        owner: root
        group: root
        mode: "0755"

    - name: Create blog.txt
      file:
        path: /opt/data/blog.txt
        state: touch
        owner: root
        group: root
        mode: "0600"
      when: inventory_hostname == "stapp01"

    - name: Grant read permission to group tony
      ansible.posix.acl:
        path: /opt/data/blog.txt
        entity: tony
        etype: group
        permissions: r
        state: present
      when: inventory_hostname == "stapp01"

    - name: Create story.txt
      file:
        path: /opt/data/story.txt
        state: touch
        owner: root
        group: root
        mode: "0600"
      when: inventory_hostname == "stapp02"

    - name: Grant read/write permission to user steve
      ansible.posix.acl:
        path: /opt/data/story.txt
        entity: steve
        etype: user
        permissions: rw
        state: present
      when: inventory_hostname == "stapp02"

    - name: Create media.txt
      file:
        path: /opt/data/media.txt
        state: touch
        owner: root
        group: root
        mode: "0600"
      when: inventory_hostname == "stapp03"

    - name: Grant read/write permission to group banner
      ansible.posix.acl:
        path: /opt/data/media.txt
        entity: banner
        etype: group
        permissions: rw
        state: present
      when: inventory_hostname == "stapp03"
```

---

# Understanding the Playbook

## File Module

Creates the required files while ensuring ownership remains:

```text
Owner: root
Group: root
```

---

## ACL Module

Used:

```yaml
ansible.posix.acl
```

This module allows granting permissions without changing the file owner.

Configured permissions:

| Server       | Entity | Type  | Permission          |
| ------------ | ------ | ----- | ------------------- |
| App Server 1 | tony   | Group | Read (`r`)          |
| App Server 2 | steve  | User  | Read + Write (`rw`) |
| App Server 3 | banner | Group | Read + Write (`rw`) |

---

# Execute the Playbook

Run:

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

Check ACLs:

```bash
ansible stapp01 -i inventory -a "getfacl /opt/data/blog.txt"

ansible stapp02 -i inventory -a "getfacl /opt/data/story.txt"

ansible stapp03 -i inventory -a "getfacl /opt/data/media.txt"
```

Example:

```text
group:tony:r--
user:steve:rw-
group:banner:rw-
```

Verify ownership:

```bash
ansible all -i inventory -a "ls -l /opt/data"
```

Output:

```text
-rw-r-----+ root root blog.txt
-rw-rw----+ root root story.txt
-rw-rw----+ root root media.txt
```

The `+` indicates extended ACL entries are present.

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
 blog.txt       story.txt      media.txt
      │              │              │
 root:root      root:root      root:root
      │              │              │
 group:tony:r   user:steve:rw  group:banner:rw
```

---

# Key Learnings

* Linux Access Control Lists (ACLs)
* `ansible.posix.acl` Module
* File Ownership
* Conditional Tasks (`when`)
* Multi-Host Automation
* Configuration Management
* Infrastructure as Code

---

# Real-World Applications

ACLs are commonly used to:

* Grant temporary access to shared files.
* Allow application users limited access.
* Secure sensitive configuration files.
* Provide fine-grained permissions without changing ownership.
* Manage enterprise Linux environments.

---

# Result

✅ Files Created

✅ Owner Set to `root`

✅ ACLs Configured

✅ Host-Specific Permissions Applied

✅ Playbook Executed Successfully

✅ Lab Validation Passed

---

## Skills Practiced

* Linux
* Ansible
* YAML
* Access Control Lists (ACLs)
* File Management
* Conditional Logic
* Configuration Management
* Infrastructure Automation
* DevOps

**#100DaysOfDevOps #Day90 #Ansible #Linux #ACL #Automation #InfrastructureAsCode #DevOps**

