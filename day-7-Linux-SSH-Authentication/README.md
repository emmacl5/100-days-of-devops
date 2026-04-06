# Day 7 - Linux SSH Authentication

## Task
Set up password-less SSH authentication from user `thor` on the jump host to all app servers using their respective users.

---

## Servers
- Jump Host: thor
- stapp01 (user: tony)
- stapp02 (user: steve)
- stapp03 (user: banner)

---

## Commands Used

### 1. Generate SSH key on jump host
```bash
ssh-keygen -t rsa -b 2048

### 2. Copy key to each app server
```bash
ssh-copy-id tony@stapp01

ssh-copy-id steve@stapp02

ssh-copy-id banner@stapp03



