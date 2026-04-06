# Day 6 - Create a Cron Job

## Task
- Install `cronie` package on all app servers
- Start and enable `crond` service
- Add a cron job for root user to run periodically

---

## Servers
- stapp01
- stapp02
- stapp03

---

## Commands Used

### 1. Install cronie
```bash
sudo yum install -y cronie

### 2. Start and enable service
```bash
sudo systemctl start crond
sudo systemctl enable crond

### 3. Add cron job for root
```bash
sudo crontab -e
Added this line:
*/5 * * * * echo hello > /tmp/cron_text


