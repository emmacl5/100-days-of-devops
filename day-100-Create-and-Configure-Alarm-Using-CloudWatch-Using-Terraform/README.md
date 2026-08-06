# Day 100 - AWS EC2 Monitoring with Terraform

## Overview

In this lab, Terraform was used to provision an **Amazon EC2 instance** and configure **Amazon CloudWatch** monitoring to track the instance's CPU utilization.

A **CloudWatch Alarm** was created to automatically monitor CPU usage and send a notification to an existing **Amazon SNS Topic** whenever the CPU utilization reaches or exceeds **90%** for one consecutive **5-minute** period.

This lab demonstrates how Infrastructure as Code (IaC) can be used to automate both infrastructure provisioning and operational monitoring.

---

## Objectives

- Launch an Amazon EC2 instance using Terraform.
- Tag the instance with the name **nautilus-ec2**.
- Create a CloudWatch Alarm named **nautilus-alarm**.
- Monitor the EC2 **CPUUtilization** metric.
- Trigger the alarm when CPU usage is **≥ 90%**.
- Configure the alarm to evaluate one **5-minute** period.
- Send notifications to an existing SNS topic.
- Output the EC2 instance name and CloudWatch alarm name.

---

## Architecture

```text
                    Terraform
                        │
                        ▼
                    main.tf
                        │
          ┌─────────────┴─────────────┐
          ▼                           ▼
   EC2 Instance                CloudWatch Alarm
   nautilus-ec2                nautilus-alarm
          │                           │
          └──────── InstanceId ───────┘
                                      │
                              Monitor CPUUtilization
                                      │
                           Average >= 90% for 5 min
                                      │
                                      ▼
                              Existing SNS Topic
                              nautilus-sns-topic
                                      │
                                      ▼
                              Email / Notification
```

---

## AWS Services Used

- Amazon EC2
- Amazon CloudWatch
- Amazon SNS
- Terraform

---

## Project Structure

```text
.
├── provider.tf
├── main.tf
├── outputs.tf
└── README.md
```

---

## Terraform Configuration

### EC2 Instance

Creates a **t2.micro** Ubuntu EC2 instance.

```hcl
resource "aws_instance" "nautilus_ec2" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"

  tags = {
    Name = "nautilus-ec2"
  }
}
```

---

### CloudWatch Alarm

Creates a CloudWatch alarm that monitors the EC2 instance.

```hcl
resource "aws_cloudwatch_metric_alarm" "nautilus_alarm" {
  alarm_name          = "nautilus-alarm"
  alarm_description   = "Alarm when CPU utilization is greater than or equal to 90 percent"

  comparison_operator = "GreaterThanOrEqualToThreshold"

  namespace   = "AWS/EC2"
  metric_name = "CPUUtilization"
  statistic   = "Average"

  threshold           = 90
  period              = 300
  evaluation_periods  = 1
  datapoints_to_alarm = 1

  dimensions = {
    InstanceId = aws_instance.nautilus_ec2.id
  }

  alarm_actions = [
    "arn:aws:sns:us-east-1:000000000000:nautilus-sns-topic"
  ]
}
```

---

### Outputs

```hcl
output "KKE_instance_name" {
  value = aws_instance.nautilus_ec2.tags["Name"]
}

output "KKE_alarm_name" {
  value = aws_cloudwatch_metric_alarm.nautilus_alarm.alarm_name
}
```

---

## Deployment Steps

### 1. Navigate to the project directory

```bash
cd /home/bob/terraform
```

### 2. Format the configuration

```bash
terraform fmt
```

### 3. Initialize Terraform

```bash
terraform init
```

### 4. Validate the configuration

```bash
terraform validate
```

### 5. Review the execution plan

```bash
terraform plan
```

### 6. Deploy the infrastructure

```bash
terraform apply -auto-approve
```

### 7. Display the outputs

```bash
terraform output
```

### 8. Verify the Terraform state

```bash
terraform state list
```

### 9. Ensure the infrastructure matches the configuration

```bash
terraform plan
```

Expected output:

```text
No changes. Your infrastructure matches the configuration.
```

---

## Resources Created

| Resource | Name |
|----------|------|
| EC2 Instance | `nautilus-ec2` |
| CloudWatch Alarm | `nautilus-alarm` |

---

## Existing Resource Used

| Resource | Name |
|----------|------|
| SNS Topic | `nautilus-sns-topic` |

---

## Alarm Configuration

| Property | Value |
|----------|-------|
| Namespace | AWS/EC2 |
| Metric | CPUUtilization |
| Statistic | Average |
| Threshold | 90% |
| Comparison Operator | GreaterThanOrEqualToThreshold |
| Evaluation Period | 5 minutes (300 seconds) |
| Consecutive Periods | 1 |
| Notification | Amazon SNS |

---

## Verification

Verify the deployed resources:

```bash
terraform output
```

Expected output:

```text
KKE_instance_name = "nautilus-ec2"
KKE_alarm_name    = "nautilus-alarm"
```

List the Terraform-managed resources:

```bash
terraform state list
```

Inspect the deployed infrastructure:

```bash
terraform show
```

Finally, confirm there are no pending changes:

```bash
terraform plan
```

---

## Troubleshooting

### Error

```text
Error: reading SNS Topic (nautilus-sns-topic): empty result
```

### Cause

The LocalStack environment used in the lab could not retrieve the existing SNS topic through the Terraform `aws_sns_topic` data source.

### Solution

Instead of using a data source, reference the existing SNS topic directly by its ARN:

```hcl
alarm_actions = [
  "arn:aws:sns:us-east-1:000000000000:nautilus-sns-topic"
]
```

This allowed the CloudWatch alarm to use the existing SNS topic without attempting to manage or recreate it.

---

## Key Learning Outcomes

- Provisioning EC2 instances with Terraform.
- Monitoring EC2 instances using Amazon CloudWatch.
- Creating CloudWatch alarms for operational monitoring.
- Configuring alarm thresholds and evaluation periods.
- Sending CloudWatch notifications through Amazon SNS.
- Creating reusable Terraform outputs.
- Understanding implicit resource dependencies in Terraform.
- Troubleshooting Terraform data source limitations in LocalStack.
- Validating infrastructure using `terraform plan`.

---

## Result

- ✅ Launched an EC2 instance named **nautilus-ec2**.
- ✅ Created a CloudWatch alarm named **nautilus-alarm**.
- ✅ Configured CPU monitoring using the **Average CPUUtilization** metric.
- ✅ Triggered the alarm when CPU utilization reaches **90%**.
- ✅ Configured one consecutive **5-minute** evaluation period.
- ✅ Connected the alarm to the existing SNS topic.
- ✅ Exported the required Terraform outputs.
- ✅ Verified the deployed infrastructure.
- ✅ Confirmed that `terraform plan` returned **No changes**.
- ✅ Successfully completed the lab.
