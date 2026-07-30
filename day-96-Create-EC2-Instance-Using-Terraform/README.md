# Day 96 - Provisioning an AWS EC2 Instance with Terraform

## Overview

Today I provisioned an Amazon EC2 instance on AWS using Terraform as part of my #100DaysOfDevOps challenge.

Instead of manually launching a virtual machine through the AWS Management Console, I defined the infrastructure as code using Terraform. The deployment included creating an RSA key pair, attaching the default security group, and launching an Amazon Linux instance.

---

# Objective

- Create an EC2 instance using Terraform.
- Launch the instance using an Amazon Linux AMI.
- Create and attach an RSA key pair.
- Attach the default security group.
- Manage the infrastructure using Infrastructure as Code (IaC).

---

# Environment

| Component | Details |
|-----------|---------|
| Cloud Provider | AWS |
| Region | us-east-1 |
| IaC Tool | Terraform |
| Compute Service | Amazon EC2 |
| Instance Type | t2.micro |
| Operating System | Amazon Linux |
| Working Directory | `/home/bob/terraform` |

---

# Project Structure

```text
terraform/
├── provider.tf
└── main.tf
```

---

# Terraform Resources

- TLS Private Key
- AWS Key Pair
- Default Security Group (Data Source)
- Amazon EC2 Instance

---

# Terraform Workflow

```bash
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
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
      ┌──────────────┴──────────────┐
      ▼                             ▼
 RSA Key Pair                Default Security Group
      │                             │
      └──────────────┬──────────────┘
                     ▼
               Amazon EC2 Instance
                 Name: nautilus-ec2
```

---

# What I Learned

- Creating EC2 instances with Terraform.
- Generating RSA key pairs automatically.
- Importing existing AWS resources using Terraform data sources.
- Attaching security groups to EC2 instances.
- Automating cloud infrastructure deployments with Infrastructure as Code.

---

# Result

- ✅ Created an RSA key pair
- ✅ Attached the default security group
- ✅ Provisioned an Amazon Linux EC2 instance
- ✅ Successfully deployed using Terraform
- ✅ Lab validation passed

---

# Skills Practiced

- Terraform
- AWS EC2
- AWS Security Groups
- SSH Key Pairs
- Infrastructure as Code (IaC)
- Cloud Automation
- DevOps

---

**#100DaysOfDevOps #Day96 #Terraform #AWS #EC2 #InfrastructureAsCode #CloudComputing #DevOps #CloudEngineering #HashiCorp #LearningInPublic**
