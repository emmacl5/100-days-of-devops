# Day 16 - Install and Configure Nginx as a Load Balancer

## Task

Configure the LBR server to load balance traffic across all application servers using Nginx.

### Requirements

* Install Nginx on the LBR server if not already installed
* Configure load balancing using only `/etc/nginx/nginx.conf`
* Use all app servers as backend targets
* Do not change the existing Apache port on the app servers
* Ensure Apache is running on all app servers
* Verify access using `curl http://stlb01:80`

---

## Servers

* LBR Server: `stlb01`
* App Server 1: `stapp01`
* App Server 2: `stapp02`
* App Server 3: `stapp03`

---

## Steps Performed

### 1. Ensured Apache was running on all app servers

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

This was done on all three app servers.

---

### 2. Installed Nginx on the LBR server

```bash
sudo yum install -y nginx
```

---

### 3. Updated the main Nginx configuration

Edited:

```bash
sudo vi /etc/nginx/nginx.conf
```

Added the load balancer upstream and proxy configuration inside the `http {}` block:

```nginx
upstream nautilus_app {
    server stapp01:80;
    server stapp02:80;
    server stapp03:80;
}

server {
    listen       80;
    listen       [::]:80;
    server_name  _;

    location / {
        proxy_pass http://nautilus_app;
    }
}
```

---

### 4. Tested Nginx configuration

```bash
sudo nginx -t
```

Result:

```text
syntax is ok
test is successful
```

---

### 5. Restarted and enabled Nginx

```bash
sudo systemctl restart nginx
sudo systemctl enable nginx
```

---

## Verification

### Checked Nginx configuration

```bash
sudo nginx -t
```

### Final test from jump host

```bash
curl http://stlb01:80
```

Expected:

* successful page output served through the load balancer

---

## Explanation

* Nginx was configured as a reverse proxy and load balancer on the LBR server
* An `upstream` block was used to define all backend application servers
* Requests to `stlb01` are forwarded to one of the app servers
* The main Nginx configuration file `/etc/nginx/nginx.conf` was updated as required
* Apache on the app servers was left on its existing port, in line with the lab instructions

---

## Issues Faced and Fixes

### Issue

Nginx configuration initially failed due to malformed block structure and an extra closing brace (`}`).

### Fix

* corrected the `server` block placement
* ensured the `upstream` block was inside the `http {}` block
* removed the extra closing brace
* validated the configuration using `nginx -t`

---

## What I Learned

* how to configure Nginx as a load balancer
* how to use `upstream` and `proxy_pass`
* importance of Nginx block structure
* how to troubleshoot syntax issues in configuration files

---

## Real-World Relevance

This reflects a common production setup where a load balancer distributes incoming traffic across multiple backend application servers for improved availability and scalability.

---

## Improvement / Automation Idea

This load balancer configuration can be automated with Ansible to standardize deployments across environments.

