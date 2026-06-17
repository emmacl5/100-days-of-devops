# Day 76 - Configuring Job-Level Permissions in Jenkins

## Background

The xFusionCorp development team onboarded new developers who required controlled access to an existing Jenkins job.

Instead of granting broad Jenkins permissions, access was restricted at the job level using Project-Based Matrix Authorization.

This approach follows the Principle of Least Privilege by granting users only the permissions they need.

---

## Objective

Configure permissions for the existing Jenkins job:

```text
Packages
```

Users:

```text
sam
rohan
```

Requirements:

### Inheritance Strategy

Use:

```text
Inherit permissions from parent ACL
```

### User Permissions

#### sam

Grant:

```text
Read
Build
Configure
```

#### rohan

Grant:

```text
Read
Build
Cancel
Configure
Update
Tag
```

---

## Solution

### Step 1 - Login to Jenkins

Access Jenkins using:

```text
Username: admin
Password: Adm!n321
```

---

### Step 2 - Verify Authorization Strategy

Navigate:

```text
Manage Jenkins
→ Security
```

Confirmed:

```text
Project-based Matrix Authorization Strategy
```

was enabled.

---

### Step 3 - Open Existing Job

Opened:

```text
Packages
→ Configure
```

---

### Step 4 - Configure Inheritance

Selected:

```text
Inherit permissions from parent ACL
```

This ensures job permissions inherit global permissions while allowing job-specific customization.

---

### Step 5 - Configure sam Permissions

Added user:

```text
sam
```

Granted:

```text
Job → Read
Job → Build
Job → Configure
```

---

### Step 6 - Configure rohan Permissions

Added user:

```text
rohan
```

Granted:

```text
Job → Read
Job → Build
Job → Cancel
Job → Configure
Job → Update
Job → Tag
```

---

## Permission Matrix

| Permission | sam | rohan |
| ---------- | --- | ----- |
| Read       | ✅   | ✅     |
| Build      | ✅   | ✅     |
| Cancel     | ❌   | ✅     |
| Configure  | ✅   | ✅     |
| Update     | ❌   | ✅     |
| Tag        | ❌   | ✅     |

---

## Architecture

```text
                 Jenkins

                     │
        ┌────────────┴────────────┐
        │                         │
        ▼                         ▼

      sam                      rohan

 Read                        Read
 Build                       Build
 Configure                   Configure
                              Cancel
                              Update
                              Tag
```

---

## Key Learnings

* Jenkins Job Security
* Project-Based Matrix Authorization
* ACL Inheritance
* Least Privilege Principle
* Job-Level Access Control
* Jenkins Administration

---

## Real-World Relevance

Enterprise Jenkins deployments commonly implement:

* Role-Based Access Control (RBAC)
* Project-Based Authorization
* Team-Specific Permissions
* Separation of Duties

Benefits:

* Improved Security
* Reduced Risk
* Better Governance
* Controlled Access to CI/CD Pipelines

---

## Result

✅ Project-Based Authorization Verified

✅ ACL Inheritance Enabled

✅ sam Permissions Configured

✅ rohan Permissions Configured

✅ Existing Job Protected

✅ Least Privilege Applied

✅ Lab Validation Passed

