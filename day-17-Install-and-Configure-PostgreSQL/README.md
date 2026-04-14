# Day 17 - PostgreSQL Database Setup and User Management

## Task

Prepare the PostgreSQL database server for a new application deployment.

### Requirements

* Create a database user `kodekloud_rin`
* Set password for the user
* Create a database `kodekloud_db5`
* Grant full privileges on the database to the user
* Do **not** restart PostgreSQL service

---

## Server

* Database Server: `stdb01`

---

## Steps Performed

### 1. Switched to PostgreSQL system user

```bash
sudo su - postgres
```

---

### 2. Accessed PostgreSQL shell

```bash
psql
```

---

### 3. Created database user

```sql
CREATE USER kodekloud_rin WITH PASSWORD 'dCV3szSGNA';
```

---

### 4. Created database

```sql
CREATE DATABASE kodekloud_db5;
```

---

### 5. Granted privileges

```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db5 TO kodekloud_rin;
```

---

### 6. Exited PostgreSQL shell

```sql
\q
```

---

## Verification

### List users

```sql
\du
```

### List databases

```sql
\l
```

Expected:

* User `kodekloud_rin` exists
* Database `kodekloud_db5` exists
* User has privileges on the database

---

## Explanation

* PostgreSQL uses role-based access control
* A new role (user) was created with a secure password
* A dedicated database was created for the application
* Permissions were granted to allow full access to the database
* No service restart was required since changes are applied dynamically

---

## What I Learned

* how to create PostgreSQL users and databases
* how to manage database access permissions
* how PostgreSQL roles and privileges work
* importance of database-level security in application deployment

---

## Real-World Relevance

This reflects a common production scenario where applications require dedicated databases and controlled access for security and isolation.

---

## Improvement / Automation Idea

Database provisioning can be automated using tools like Ansible or Terraform to standardize deployments across environments.

