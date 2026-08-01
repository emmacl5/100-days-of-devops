# Day 98 - Provisioning a Private AWS VPC with Terraform

## Overview

Today I provisioned a private AWS infrastructure using Terraform as part of my **#100DaysOfDevOps** challenge.

The lab involved creating a private VPC, a private subnet, a security group with restricted access, and an Amazon EC2 instance—all managed through Infrastructure as Code (IaC).

A key validation requirement was ensuring that **`terraform plan` returned "No changes. Your infrastructure matches the configuration."**, confirming that the deployed infrastructure fully matched the Terraform configuration.

---

# Objective

- Create a private AWS VPC.
- Create a private subnet with automatic public IP assignment disabled.
- Provision an EC2 instance inside the private subnet.
- Restrict inbound access to resources within the VPC CIDR block.
- Manage the complete infrastructure using Terraform.

---

# Environment

| Component | Details |
|-----------|---------|
| Cloud Provider | AWS |
| Region | us-east-1 |
| IaC Tool | Terraform |
| Compute Service | Amazon EC2 |
| Networking | Amazon VPC |
| Working Directory | `/home/bob/terraform` |

---

# Infrastructure Created

| Resource | Name |
|----------|------|
| VPC | `devops-priv-vpc` |
| Subnet | `devops-priv-subnet` |
| Security Group | `devops-priv-sg` |
| EC2 Instance | `devops-priv-ec2` |

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
                     ▼
        Private Amazon VPC
          10.0.0.0/16
                     │
                     ▼
       Private Subnet
         10.0.1.0/24
 (Auto Public IP Disabled)
                     │
                     ▼
      Security Group
 Allows Traffic Only From
      10.0.0.0/16
                     │
                     ▼
      Amazon EC2 Instance
      Name: devops-priv-ec2
      Type: t2.micro
```

---

# What I Learned

- Creating private VPCs using Terraform.
- Deploying private subnets.
- Disabling automatic public IP assignment.
- Restricting EC2 access using security groups.
- Using Terraform variables and outputs.
- Verifying Infrastructure as Code with `terraform plan`.

---

# Result

- ✅ Created a private VPC
- ✅ Created a private subnet
- ✅ Disabled automatic public IP assignment
- ✅ Configured a private security group
- ✅ Provisioned an EC2 instance
- ✅ `terraform plan` returned **No changes**
- ✅ Lab validation passed

---

# Skills Practiced

- Terraform
- AWS VPC
- AWS Subnets
- AWS Security Groups
- Amazon EC2
- Infrastructure as Code (IaC)
- Cloud Networking
- DevOps

---

**#100DaysOfDevOps #Day98 #Terraform #AWS #VPC #EC2 #InfrastructureAsCode #CloudNetworking #CloudEngineering #DevOps #LearningInPublic**


