# Day 95 - Creating an AWS Security Group with Terraform

## Overview

Today I continued my Infrastructure as Code (IaC) journey by provisioning an **AWS Security Group** using Terraform.

The security group was created inside the **default VPC** in the **us-east-1** region. I configured inbound rules to allow HTTP and SSH traffic while managing the entire infrastructure through Terraform.

This lab reinforced how Terraform can automate cloud networking resources in a repeatable and version-controlled way.

---

# Objective

- Create an AWS Security Group using Terraform.
- Deploy the security group inside the default VPC.
- Configure HTTP and SSH inbound rules.
- Provision the infrastructure in the **us-east-1** region.
- Manage the infrastructure using Infrastructure as Code (IaC).

---

# Environment

| Component | Details |
|-----------|---------|
| Cloud Provider | AWS |
| Region | us-east-1 |
| IaC Tool | Terraform |
| Resource | AWS Security Group |
| Working Directory | `/home/bob/terraform` |

---

# Project Structure

```text
terraform/
├── provider.tf
└── main.tf
```

---

# Terraform Configuration

## main.tf

```hcl
data "aws_vpc" "default" {
  default = true
}

resource "aws_security_group" "nautilus_sg" {
  name        = "nautilus-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    description = "Allow HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "nautilus-sg"
  }
}
```

---

# Terraform Workflow

### Initialize Terraform

```bash
terraform init
```

### Validate Configuration

```bash
terraform validate
```

### Preview Changes

```bash
terraform plan
```

### Apply Configuration

```bash
terraform apply -auto-approve
```

---

# Verification

List Terraform-managed resources:

```bash
terraform state list
```

Expected output:

```text
data.aws_vpc.default
aws_security_group.nautilus_sg
```

Inspect the deployed infrastructure:

```bash
terraform show
```

Verify:

- Security Group Name: `nautilus-sg`
- Description: `Security group for Nautilus App Servers`
- VPC: Default VPC
- HTTP (TCP 80) open to `0.0.0.0/0`
- SSH (TCP 22) open to `0.0.0.0/0`

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
             AWS Provider (Terraform)
                     │
                     ▼
            Default Amazon VPC
                     │
                     ▼
          AWS Security Group
             nautilus-sg
            ├──────────────┐
            │              │
         HTTP:80        SSH:22
      0.0.0.0/0      0.0.0.0/0
```

---

# Key Concepts

- Infrastructure as Code (IaC)
- AWS Security Groups
- Terraform Data Sources
- Terraform Resources
- AWS Networking
- Ingress Rules
- Cloud Security

---

# What I Learned

- Using Terraform data sources to retrieve existing infrastructure.
- Creating AWS Security Groups with Terraform.
- Configuring multiple ingress rules.
- Managing cloud networking resources through code.
- Automating AWS security configurations.

---

# Result

- ✅ Retrieved the default VPC
- ✅ Created the `nautilus-sg` security group
- ✅ Configured HTTP (80) inbound access
- ✅ Configured SSH (22) inbound access
- ✅ Successfully provisioned the infrastructure using Terraform
- ✅ Lab validation passed

---

# Skills Practiced

- Terraform
- AWS
- AWS Security Groups
- VPC
- Infrastructure as Code (IaC)
- Cloud Networking
- HCL
- DevOps Automation

---

**#100DaysOfDevOps #Day95 #Terraform #AWS #SecurityGroup #VPC #InfrastructureAsCode #CloudSecurity #CloudComputing #DevOps**
