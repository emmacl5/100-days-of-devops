Day 91 - Deploying an Apache Web Server and Web Page with Ansible
Overview

In this lab, I automated the installation and configuration of the Apache HTTP Server (httpd) across multiple Linux application servers using Ansible.

The playbook installed and started Apache, deployed a sample web page, inserted a welcome message at the beginning of the page using the lineinfile module, and ensured the correct ownership and permissions.

Objective
Install Apache (httpd) on all application servers.
Ensure the Apache service is running and enabled.
Deploy a sample index.html page.
Insert an additional line at the top of the file using the lineinfile module.
Set the file owner to apache.
Configure file permissions to 0644.
Environment
Ansible Controller
Server	User
Jump Host	thor

Working directory:

/home/thor/ansible
Managed Nodes
App Server 1 (stapp01)
App Server 2 (stapp02)
App Server 3 (stapp03)
Playbook

File: /home/thor/ansible/playbook.yml

---
- name: Configure Apache Web Server
  hosts: all
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

    - name: Create sample web page
      copy:
        dest: /var/www/html/index.html
        content: |
          This is a Nautilus sample file, created using Ansible!
        owner: apache
        group: apache
        mode: "0644"

    - name: Add welcome message at the top
      lineinfile:
        path: /var/www/html/index.html
        line: "Welcome to xFusionCorp Industries!"
        insertbefore: BOF

    - name: Ensure ownership and permissions
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode: "0644"
Execution

Run the playbook:

ansible-playbook -i inventory playbook.yml
Verification

Verify the web page:

ansible all -i inventory -a "cat /var/www/html/index.html"

Expected output:

Welcome to xFusionCorp Industries!
This is a Nautilus sample file, created using Ansible!

Verify ownership and permissions:

ansible all -i inventory -a "ls -l /var/www/html/index.html"

Expected:

-rw-r--r-- 1 apache apache ...

Verify the Apache service:

ansible all -i inventory -a "systemctl status httpd"
Architecture
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
 Start Service  Start Service Start Service
      │              │              │
 Deploy index.html on all servers
      │              │              │
 Set owner: apache:apache
 Set mode : 0644
Key Learnings
Installing packages with the yum module.
Managing services using the service module.
Creating files using the copy module.
Editing existing files with the lineinfile module.
Managing file ownership and permissions.
Automating consistent web server deployments across multiple hosts.
Real-World Applications

This workflow is commonly used to:

Deploy internal company websites.
Provision web servers automatically.
Standardize web server configurations.
Maintain consistent file permissions across environments.
Build Infrastructure as Code (IaC) solutions for web hosting.
Result
✅ Apache installed on all application servers.
✅ Apache service started and enabled.
✅ Sample web page deployed successfully.
✅ Welcome message inserted at the top of the page.
✅ Correct ownership (apache:apache) applied.
✅ Permissions set to 0644.
✅ Lab validation passed.
Skills Practiced
Ansible
Linux
Apache HTTP Server
YAML
Package Management
Service Management
File Management
copy Module
lineinfile Module
Infrastructure as Code (IaC)
Configuration Management
DevOps

#100DaysOfDevOps #Day91 #Ansible #Apache #Linux #InfrastructureAsCode #ConfigurationManagement #Automation #DevOps
