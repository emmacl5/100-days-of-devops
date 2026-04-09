# Day 13 - IPTables Installation and Access Control

## Task

Secure all application servers by configuring firewall rules using `iptables`.

### Requirements

* Install `iptables` and dependencies on all app servers
* Allow access to port `3001` **only from the LBR host**
* Block access to port `3001` for all other sources
* Ensure firewall rules persist after system reboot

---

## Servers

* App Server 1: `stapp01`
* App Server 2: `stapp02`
* App Server 3: `stapp03`

## Trusted Host

* LBR host: `stlb01`
* LBR IP: `10.244.49.39`

---

## Steps Performed

### 1. Install iptables

```bash
sudo yum install -y iptables iptables-services
```

---

### 2. Start and enable iptables service

```bash
sudo systemctl start iptables
sudo systemctl enable iptables
```

---

### 3. Configure firewall rules

Allowed access to port `3001` only from LBR host:

```bash
sudo iptables -A INPUT -p tcp -s 10.244.49.39 --dport 3001 -j ACCEPT
```

Blocked access to port `3001` for all other sources:

```bash
sudo iptables -A INPUT -p tcp --dport 3001 -j DROP
```

---

### 4. Preserve essential access rules

To maintain connectivity and system functionality:

```bash
sudo iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A INPUT -p icmp -j ACCEPT
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

---

### 5. Save rules persistently

```bash
sudo sh -c 'iptables-save > /etc/sysconfig/iptables'
```

This ensures firewall rules remain after reboot.

---

## Verification

### Check firewall rules

```bash
sudo iptables -L -n
```

### Check persistent configuration

```bash
cat /etc/sysconfig/iptables
```

### Check service status

```bash
sudo systemctl status iptables
```

---

## Testing

### From Jump Host (should fail)

```bash
curl -I http://stapp01:3001
curl -I http://stapp02:3001
curl -I http://stapp03:3001
```

Expected:

* Connection blocked (timeout or "No route to host")

---

### From LBR Host (should succeed)

```bash
curl -I http://stapp01:3001
curl -I http://stapp02:3001
curl -I http://stapp03:3001
```

Expected:

* HTTP response (e.g., `200 OK` or `403 Forbidden`)

---

## Explanation

* `iptables` is used to filter network traffic at the OS level
* A specific rule allows only the trusted LBR host to access port `3001`
* A general rule blocks all other incoming traffic to that port
* Rule order is critical: allow rules must appear before deny rules
* Rules are saved to `/etc/sysconfig/iptables` for persistence

---

## Issues Faced and Solutions

### Issue 1: Wrong LBR IP used

* Initially used DNS-resolved IP instead of actual source IP
* Fixed by using:

```bash
hostname -I
```

---

### Issue 2: Permission denied when saving rules

```bash
sudo iptables-save > /etc/sysconfig/iptables
```

Fix:

```bash
sudo sh -c 'iptables-save > /etc/sysconfig/iptables'
```

---

### Issue 3: iptables service not enabled

* Lab failed because service was not enabled
* Fixed with:

```bash
sudo systemctl enable iptables
```

---

### Issue 4: Apache running on wrong port

* Service was not listening on required port
* Fixed by updating:

```bash
Listen 3001
```

---

## What I Learned

* how to install and configure `iptables`
* how to restrict access based on source IP
* importance of rule order in firewall configuration
* difference between DNS IP and actual source IP
* how to persist firewall rules after reboot
* troubleshooting real-world connectivity issues

---

## Real-World Relevance

This setup reflects production security practices where backend services are exposed only to trusted systems (e.g., load balancers), while direct external access is blocked.

---

## Improvement / Automation Idea

Firewall configurations can be automated using tools like Ansible to ensure consistency across multiple servers.

---

