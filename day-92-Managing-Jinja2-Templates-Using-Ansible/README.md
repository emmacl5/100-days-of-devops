# Day 92 - Using Ansible Roles and Jinja2 Templates

## Overview

Today I worked with **Ansible Roles** and **Jinja2 Templates** to automate the deployment of a web page on an Apache web server.

Instead of hardcoding values, I used the `inventory_hostname` variable to dynamically generate server-specific content. This approach makes the automation reusable, scalable, and easier to maintain.

---

## Objective

- Execute an existing Ansible role on App Server 3.
- Create a Jinja2 template for `index.html`.
- Deploy the template using the `template` module.
- Dynamically display the target server's hostname.
- Set the correct ownership and file permissions.

---

## Environment

| Component | Details |
|----------|---------|
| Controller | Jump Host (`thor`) |
| Target Host | `stapp03` |
| Web Server | Apache HTTP Server (`httpd`) |
| Automation Tool | Ansible |

---

## Project Structure

```text
ansible/
├── inventory
├── playbook.yml
└── role/
    └── httpd/
        ├── tasks/
        │   └── main.yml
        └── templates/
            └── index.html.j2
```

---

## Playbook

```yaml
---
- name: Configure Apache using Ansible Role
  hosts: stapp03
  become: yes

  roles:
    - role/httpd
```

---

## Jinja2 Template

**templates/index.html.j2**

```jinja2
This file was created using Ansible on {{ inventory_hostname }}
```

The `inventory_hostname` variable automatically inserts the hostname of the managed server during deployment.

---

## Role Task

**tasks/main.yml**

```yaml
- name: Deploy web page from template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
    owner: banner
    group: banner
    mode: "0655"
```

---

## Run the Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Verification

### Verify the generated web page

```bash
ansible stapp03 -i inventory -a "cat /var/www/html/index.html"
```

Output:

```text
This file was created using Ansible on stapp03
```

### Verify ownership and permissions

```bash
ansible stapp03 -i inventory -a "stat -c '%U %G %a' /var/www/html/index.html"
```

Output:

```text
banner banner 655
```

---

## What I Learned

- How to organize automation using **Ansible Roles**
- Creating reusable **Jinja2 templates**
- Using the `template` module
- Dynamically rendering values with `inventory_hostname`
- Managing Linux file ownership and permissions
- Building reusable Infrastructure as Code (IaC)

---

## Key Ansible Modules

| Module | Purpose |
|--------|---------|
| `template` | Deploy Jinja2 templates |
| `become` | Execute tasks with elevated privileges |

---

## Architecture

```text
                 Jump Host
              (Ansible Controller)
                       │
                       ▼
                 playbook.yml
                       │
                       ▼
                 HTTPD Role
                       │
         ┌─────────────┴─────────────┐
         ▼                           ▼
   tasks/main.yml            templates/index.html.j2
         │                           │
         └─────────────┬─────────────┘
                       ▼
                App Server 3
                  (stapp03)
                       │
                       ▼
      /var/www/html/index.html
```

---

## Result

- ✅ Executed an existing Ansible role
- ✅ Created a reusable Jinja2 template
- ✅ Dynamically rendered the server hostname
- ✅ Deployed the web page successfully
- ✅ Configured the correct ownership and permissions
- ✅ Lab validation passed

---

## Skills Practiced

- Ansible
- Ansible Roles
- Jinja2
- Apache HTTP Server
- Linux Administration
- Infrastructure as Code (IaC)
- Configuration Management
- DevOps

---

**#100DaysOfDevOps #Day92 #Ansible #Jinja2 #Automation #Linux #Apache #DevOps #InfrastructureAsCode**
