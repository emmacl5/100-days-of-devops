# Day 18 - MariaDB Installation and Database Configuration

## Task

Set up a MariaDB database server and configure a database with user access.

### Requirements

* Install and configure MariaDB server
* Create a database `kodekloud_db10`
* Create a user `kodekloud_top`
* Set password for the user
* Grant full privileges on the database to the user

---

## Server

* Database Server: `stdb01`

---

## Steps Performed

### 1. Installed MariaDB server

```bash
sudo yum install -y mariadb-server
```

---

### 2. Started and enabled MariaDB service

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

---

### 3. Logged into MariaDB shell

```bash
sudo mysql
```

---

### 4. Created database

```sql
CREATE DATABASE kodekloud_db10;
```

---

### 5. Created database user

```sql
CREATE USER 'kodekloud_top'@'localhost' IDENTIFIED BY 'B4zNgHA7Ya';
```

---

### 6. Granted privileges to user

```sql
GRANT ALL PRIVILEGES ON kodekloud_db10.* TO 'kodekloud_top'@'localhost';
```

---

### 7. Applied privilege changes

```sql
FLUSH PRIVILEGES;
```

---

### 8. Exited MariaDB shell

```sql
EXIT;
```

---

## Verification

### Check databases

```bash
sudo mysql -e "SHOW DATABASES;"
```

### Check users

```bash
sudo mysql -e "SELECT User, Host FROM mysql.user;"
```

Expected:

* Database `kodekloud_db10` exists
* User `kodekloud_top` exists
* User has access to the database

---

## Explanation

* MariaDB is a MySQL-compatible relational database system
* A dedicated database was created for application use
* A user was created with restricted host access (`localhost`)
* Full privileges were granted on the specific database
* `FLUSH PRIVILEGES` ensures changes take effect immediately

---

## What I Learned

* how to install and configure MariaDB
* how to create databases and users
* how to manage privileges in MySQL/MariaDB
* differences between PostgreSQL and MariaDB syntax

---

## Real-World Relevance

Database configuration is a critical step in application deployment. Proper user access and privilege management ensure both functionality and security.

---

## Improvement / Automation Idea

Database provisioning and user management can be automated using tools like Ansible to maintain consistency across environments.

