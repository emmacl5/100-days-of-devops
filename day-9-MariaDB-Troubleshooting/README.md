## Issue Identified
MariaDB service failed to start because the data directory `/var/lib/mysql` and runtime directory `/run/mariadb` were missing.

## Fix Applied
```bash
sudo mkdir -p /var/lib/mysql
sudo chown -R mysql:mysql /var/lib/mysql
sudo mkdir -p /run/mariadb
sudo chown -R mysql:mysql /run/mariadb
sudo systemctl start mariadb
sudo systemctl status mariadb


Note:
I debugged a MariaDB outage caused by a missing data directory and runtime directory.
I recreated the directories,fixed ownership and restored the service.
