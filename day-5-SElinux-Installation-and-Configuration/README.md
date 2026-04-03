# Day 5 - SELinux Installation and Configuration

## Task
- Install required SELinux packages
- Permanently disable SELinux
- No reboot required
- Ignore current runtime status

---

## Server
- App Server 1: stapp01

---

## Commands Used

### 1. Install SELinux packages
```bash
sudo yum install -y selinux-policy selinux-policy-targeted policycoreutils

#2. Edit SELinux configuration

sudo vi /etc/selinux/config

#3. Update configuration

SELINUX=disabled

#4 Verification

cat /etc/selinux/config


