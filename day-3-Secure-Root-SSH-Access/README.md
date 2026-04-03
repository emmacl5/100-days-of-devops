# Day 3 - Secure Root SSH Access

## Task
Disable direct SSH root login on all application servers within the datacenter.

---

## Servers Involved
- App Server 1: stapp01
- App Server 2: stapp02
- App Server 3: stapp03

---

## Commands Used

### 1. Connect to the server
```bash
ssh tony@stapp01

### 2. Edit SSH configuration
´```bash
sudo vi /etc/ssh/sshd_config

### 3. Update the configuration
PermitRootLogin no

### 4. Restart SSH service
```bash
sudo systemctl restart sshd


