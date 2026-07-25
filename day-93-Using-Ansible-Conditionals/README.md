# Day 93 - Using Ansible Conditionals (`when`) for Host-Specific Tasks

## Overview

Today I learned how to use **Ansible conditionals** to execute different tasks on different managed hosts within a single playbook.

Using the `ansible_nodename` fact together with the `when` statement, I deployed different files to each application server while applying the correct ownership and permissions.

---

## Objective

- Execute a single playbook against all application servers.
- Use `when` conditionals to determine which task runs on each host.
- Copy different files to different servers.
- Set the correct owner, group, and file permissions.

---

## Environment

| Component | Details |
|----------|---------|
| Controller | Jump Host (`thor`) |
| Managed Hosts | `stapp01`, `stapp02`, `stapp03` |
| Automation Tool | Ansible |

---

## Playbook

```yaml
---
- name: Copy files using Ansible conditionals
  hosts: all
  become: yes
  gather_facts: yes

  tasks:

    - name: Ensure /opt/devops exists
      file:
        path: /opt/devops
        state: directory
        mode: "0755"

    - name: Copy blog.txt to App Server 1
      copy:
        src: /usr/src/devops/blog.txt
        dest: /opt/devops/blog.txt
        owner: tony
        group: tony
        mode: "0777"
      when: ansible_nodename == "stapp01"

    - name: Copy story.txt to App Server 2
      copy:
        src: /usr/src/devops/story.txt
        dest: /opt/devops/story.txt
        owner: steve
        group: steve
        mode: "0777"
      when: ansible_nodename == "stapp02"

    - name: Copy media.txt to App Server 3
      copy:
        src: /usr/src/devops/media.txt
        dest: /opt/devops/media.txt
        owner: banner
        group: banner
        mode: "0777"
      when: ansible_nodename == "stapp03"
```

---

## Execution

Run the playbook:

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Verification

### App Server 1

```bash
ansible stapp01 -i inventory -m shell -a "ls -l /opt/devops/blog.txt && cat /opt/devops/blog.txt"
```

Output:

```text
-rwxrwxrwx 1 tony tony ...
Welcome to xFusionCorp Industries !
```

### App Server 2

```bash
ansible stapp02 -i inventory -m shell -a "ls -l /opt/devops/story.txt && cat /opt/devops/story.txt"
```

Output:

```text
-rwxrwxrwx 1 steve steve ...
Welcome to Nautilus Group !
```

### App Server 3

```bash
ansible stapp03 -i inventory -m shell -a "ls -l /opt/devops/media.txt && cat /opt/devops/media.txt"
```

Output:

```text
-rwxrwxrwx 1 banner banner ...
Welcome to KodeKloud !
```

---

## Key Learnings

- Using `when` statements in Ansible.
- Working with gathered facts.
- Using `ansible_nodename` for host-specific logic.
- Running one playbook across multiple servers.
- Applying different configurations to different hosts.
- Managing Linux file ownership and permissions.

---

## Key Ansible Modules

| Module | Purpose |
|--------|---------|
| `copy` | Copy files from the controller to managed hosts |
| `file` | Create directories and manage file attributes |

---

## Architecture

```text
                  Jump Host
             (Ansible Controller)
                      │
                      ▼
              playbook.yml (hosts: all)
                      │
      ┌───────────────┼───────────────┐
      ▼               ▼               ▼
   stapp01         stapp02        stapp03
      │               │               │
 when == stapp01  when == stapp02  when == stapp03
      │               │               │
 blog.txt         story.txt        media.txt
      │               │               │
 owner: tony      owner: steve     owner: banner
 mode: 0777       mode: 0777       mode: 0777
```

---

## Result

- ✅ Executed a single playbook for all hosts.
- ✅ Used `when` conditionals successfully.
- ✅ Copied host-specific files.
- ✅ Applied the correct ownership and permissions.
- ✅ Lab validation passed.

---

## Skills Practiced

- Ansible
- YAML
- Linux
- Ansible Facts
- Conditional Execution (`when`)
- File Management
- Configuration Management
- Infrastructure as Code (IaC)
- DevOps

---

**#100DaysOfDevOps #Day93 #Ansible #Linux #Automation #InfrastructureAsCode #ConfigurationManagement #DevOps**
