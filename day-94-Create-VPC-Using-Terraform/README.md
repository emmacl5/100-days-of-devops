# Day 94 - Provisioning an AWS VPC with Terraform

## Overview

Today I took my first step toward automating AWS infrastructure using **Terraform**.

In this lab, I created an Amazon Virtual Private Cloud (VPC) named **xfusion-vpc** in the **us-east-1** region. The infrastructure was defined as code using Terraform and deployed through the Terraform workflow (`init`, `validate`, `plan`, and `apply`).

This exercise demonstrates the core principles of **Infrastructure as Code (IaC)** by provisioning cloud resources in a repeatable and version-controlled manner.

---

# Objective

- Create an AWS VPC using Terraform.
- Deploy the infrastructure in the **us-east-1** region.
- Assign the VPC the name **xfusion-vpc**.
- Define infrastructure using a Terraform configuration.
- Provision the resource through Terraform.

---

# Environment

| Component | Details |
|-----------|---------|
| Cloud Provider | AWS |
| Region | us-east-1 |
| IaC Tool | Terraform |
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
resource "aws_vpc" "xfusion_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "xfusion-vpc"
  }
}
```

---

# Terraform Workflow

### Initialize Terraform

```bash
terraform init
```

### Validate the Configuration

```bash
terraform validate
```

### Preview Infrastructure Changes

```bash
terraform plan
```

### Provision the Infrastructure

```bash
terraform apply -auto-approve
```

---

# Verification

Verify the Terraform state:

```bash
terraform state list
```

Expected output:

```text
aws_vpc.xfusion_vpc
```

Display the deployed infrastructure:

```bash
terraform show
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
          Amazon Virtual Private Cloud
              Name: xfusion-vpc
          CIDR: 10.0.0.0/16
          Region: us-east-1
```

---

# Key Concepts

- Infrastructure as Code (IaC)
- Terraform Resources
- AWS Provider
- Amazon VPC
- Terraform State
- Resource Tagging
- Cloud Networking Fundamentals

---

# What I Learned

- How Terraform provisions AWS infrastructure.
- Defining cloud resources using HCL (HashiCorp Configuration Language).
- The Terraform workflow:
  - `terraform init`
  - `terraform validate`
  - `terraform plan`
  - `terraform apply`
- Managing cloud infrastructure through code.
- Creating and tagging an Amazon VPC.

---

# Result

- ✅ Created an AWS VPC using Terraform
- ✅ Named the VPC **xfusion-vpc**
- ✅ Provisioned infrastructure in **us-east-1**
- ✅ Successfully applied the Terraform configuration
- ✅ Lab validation passed

---

# Skills Practiced

- Terraform
- Infrastructure as Code (IaC)
- AWS VPC
- Amazon Web Services
- Cloud Networking
- HashiCorp Configuration Language (HCL)
- DevOps Automation

---

**#100DaysOfDevOps #Day94 #Terraform #AWS #VPC #InfrastructureAsCode #CloudComputing #DevOps #HashiCorp #Automation**
