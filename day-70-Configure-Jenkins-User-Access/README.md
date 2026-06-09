# Day 70 - Configuring User Access and Project-Based Authorization in Jenkins

## Background

The Nautilus DevOps team needed to onboard developers into Jenkins while ensuring proper access control and security.

To achieve this, a new Jenkins user was created and granted limited permissions using the Project-based Matrix Authorization Strategy.

The goal was to allow developers to view jobs without granting administrative or build permissions.

---

## Objective

Configure Jenkins access control with the following requirements:

### User Creation

Create a Jenkins user:

```text
Username: james
Password: GyQkFRVNr3
Full Name: James
```

### Global Permissions

Grant:

```text
Overall → Read
```

to the user `james`.

### Anonymous Access

Remove all permissions from:

```text
Anonymous
```

### Administrator Access

Ensure:

```text
admin
```

retains:

```text
Overall → Administer
```

### Job-Level Permissions

For the existing Jenkins job:

```text
Helloworld
```

Grant:

```text
Job → Read
```

only to the `james` user.

---

## Solution

### Step 1 - Login to Jenkins

Login using:

```text
Username: admin
Password: Adm!n321
```

---

### Step 2 - Create User

Navigate to:

```text
Manage Jenkins
→ Users
→ Create User
```

Create:

```text
Username: james
Password: GyQkFRVNr3
Full Name: James
```

---

### Step 3 - Configure Authorization Strategy

Navigate to:

```text
Manage Jenkins
→ Security
```

Select:

```text
Project-based Matrix Authorization Strategy
```

---

### Step 4 - Configure Global Permissions

User:

```text
james
```

Granted:

```text
Overall → Read
```

Only.

---

### Step 5 - Remove Anonymous Permissions

Ensure:

```text
Anonymous
```

has no permissions assigned.

---

### Step 6 - Verify Administrator Permissions

Ensure:

```text
admin
```

retains:

```text
Overall → Administer
```

---

### Step 7 - Configure Job-Level Permissions

Open existing job:

```text
Helloworld
```

Configure project security.

Add:

```text
james
```

Grant only:

```text
Job → Read
```

Do not grant:

```text
Build
Configure
Delete
Workspace
SCM
Agent
Credentials
```

---

## Verification

Login as:

```text
Username: james
Password: GyQkFRVNr3
```

Verify:

✅ Can view Jenkins

✅ Can see Helloworld job

✅ Can open job

❌ Cannot build jobs

❌ Cannot configure jobs

❌ Cannot administer Jenkins

---

## Architecture

```text
                     Jenkins
                         │
        ┌────────────────┴───────────────┐
        │                                │
        ▼                                ▼
     admin                           james

Overall: Administer           Overall: Read

Job:
  Full Control                Job: Read Only
```

---

## Key Learnings

* Jenkins User Management
* Authentication vs Authorization
* Matrix Authorization Strategy
* Principle of Least Privilege
* Job-Level Permissions
* Secure CI/CD Platform Administration

---

## Real-World Relevance

In enterprise environments:

* Developers typically receive read/build access.
* DevOps Engineers manage Jenkins configuration.
* Administrators maintain platform security.
* Anonymous access is usually disabled.

Implementing least-privilege access reduces security risks and prevents accidental changes.

---

## Result

✅ User Created Successfully

✅ Project-Based Matrix Authorization Configured

✅ Anonymous Permissions Removed

✅ Admin Permissions Preserved

✅ Job-Level Access Configured

✅ Security Validation Passed

✅ Lab Validation Passed

