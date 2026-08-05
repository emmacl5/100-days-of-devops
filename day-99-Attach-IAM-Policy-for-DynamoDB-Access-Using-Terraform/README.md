# Day 99 - Securing Amazon DynamoDB with IAM Using Terraform

## Overview

Today I provisioned an Amazon DynamoDB table and implemented fine-grained access control using Terraform as part of my **#100DaysOfDevOps** challenge.

The lab focused on creating a DynamoDB table, an IAM role, and a custom IAM policy that grants read-only access (GetItem, Scan, Query) only to the specific table. This demonstrates how Infrastructure as Code can be used to automate both cloud resources and security configurations.

As a final validation step, Terraform confirmed that the deployed infrastructure matched the configuration exactly.

---

# Objective

- Create an Amazon DynamoDB table.
- Create an IAM role.
- Create an IAM policy with read-only permissions.
- Restrict access to the specific DynamoDB table.
- Attach the policy to the IAM role.
- Manage everything using Terraform.

---

# Environment

| Component | Details |
|-----------|---------|
| Cloud Provider | AWS |
| Region | us-east-1 |
| Database | Amazon DynamoDB |
| Identity Service | AWS IAM |
| IaC Tool | Terraform |
| Working Directory | `/home/bob/terraform` |

---

# Infrastructure Created

| Resource | Name |
|----------|------|
| DynamoDB Table | `xfusion-table` |
| IAM Role | `xfusion-role` |
| IAM Policy | `xfusion-readonly-policy` |

---

# Terraform Workflow

```bash
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
terraform plan
```

Final verification:

```text
No changes. Your infrastructure matches the configuration.
```

---

# Architecture

```text
                Developer
                    │
                    ▼
              Terraform CLI
                    │
                    ▼
                main.tf
                    │
                    ▼
              AWS Provider
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
 Amazon DynamoDB          AWS IAM Role
   xfusion-table          xfusion-role
        ▲                       │
        │                       ▼
        └──────── IAM Policy ────────┐
           xfusion-readonly-policy   │
                                     │
          Allowed Actions            │
          • GetItem                  │
          • Scan                     │
          • Query                    │
                                     ▼
          Access Restricted to xfusion-table
```

---

# What I Learned

- Creating DynamoDB tables with Terraform.
- Managing IAM roles as Infrastructure as Code.
- Writing custom IAM policies using JSON.
- Applying the Principle of Least Privilege.
- Attaching IAM policies to roles.
- Restricting permissions to specific AWS resources.

---

# Result

- ✅ Created an Amazon DynamoDB table
- ✅ Created an IAM role
- ✅ Created a custom IAM policy
- ✅ Granted only read permissions
- ✅ Restricted access to one DynamoDB table
- ✅ Attached the policy to the role
- ✅ `terraform plan` returned **No changes**
- ✅ Lab validation passed

---

# Skills Practiced

- Terraform
- Amazon DynamoDB
- AWS IAM
- IAM Policies
- IAM Roles
- Infrastructure as Code (IaC)
- Cloud Security
- DevOps

---

**#100DaysOfDevOps #Day99 #Terraform #AWS #DynamoDB #IAM #CloudSecurity #InfrastructureAsCode #CloudEngineering #DevOps #LearningInPublic**
