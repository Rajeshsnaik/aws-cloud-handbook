# 🔵 AWS Config

![AWS](./config.png)

# 🔵 What is AWS Config?

**AWS Config** is a fully managed AWS service that continuously monitors, records, and evaluates the configuration of your AWS resources. It helps you understand how your resources are configured, track configuration changes over time, and determine whether those resources comply with your organization's security, governance, and operational policies.

Instead of manually checking every AWS resource, AWS Config automatically records configuration changes and evaluates them against predefined or custom compliance rules.

> **Simple Definition**
>
> **AWS Config continuously records the configuration of AWS resources and checks whether they comply with your organization's rules and best practices.**

---

# 🔵 Why Do We Need AWS Config?

As cloud environments grow, it becomes difficult to manually verify whether resources are configured correctly.

AWS Config helps you:

- Monitor AWS resource configurations
- Track every configuration change
- Detect configuration drift
- Ensure compliance with company policies
- Improve security and governance
- Generate compliance reports
- Audit infrastructure changes
- Troubleshoot configuration issues

---

# 🔵 How AWS Config Works

```text
AWS Resources
      │
      ▼
AWS Config Records Configuration
      │
      ▼
Stores Configuration History
      │
      ▼
Evaluates Config Rules
      │
      ▼
Compliant / Non-Compliant
      │
      ▼
Notifications & Reports
```

---

# 🔵 Is AWS Config Regional or Global?

**AWS Config is primarily a Regional service.**

This means:

- Configuration data is recorded separately for each AWS Region.
- Rules are evaluated within that Region.
- Some global resources (such as IAM) can also be recorded depending on your recorder settings.

---

# 🔵 Main Components of AWS Config

AWS Config consists of several important components.

### Configuration Recorder

Continuously records the configuration of supported AWS resources.

Examples:

- EC2
- VPC
- Security Groups
- IAM
- S3
- RDS

---

### Configuration History

Stores every configuration change made to AWS resources.

Example:

```text
EC2 Instance

Created
      │
      ▼
Security Group Changed
      │
      ▼
Instance Type Updated
      │
      ▼
Monitoring Enabled
```

---

### Configuration Snapshot

A point-in-time snapshot of all supported AWS resources in your account.

Useful for:

- Auditing
- Compliance
- Disaster recovery
- Inventory reporting

---

### AWS Config Rules

Rules evaluate whether AWS resources comply with your organization's policies.

Rules can be:

- AWS Managed Rules
- Custom Lambda Rules

Example:

- EC2 must have Detailed Monitoring enabled.
- S3 buckets should not allow public access.
- Security Groups should not allow SSH from `0.0.0.0/0`.

---

### Compliance Dashboard

Provides an overview of:

- Compliant resources
- Non-compliant resources
- Rule evaluation results

---

# 🔵 AWS Managed Rules vs Custom Rules

| Feature         | AWS Managed Rules          | Custom Rules                   |
| --------------- | -------------------------- | ------------------------------ |
| Created By      | AWS                        | User                           |
| Custom Logic    | ❌                         | ✅                             |
| Lambda Required | ❌                         | ✅                             |
| Best For        | Standard Compliance Checks | Organization-Specific Policies |

---

# 🔵 Features of AWS Config

---

## 1. Configuration Tracking

Automatically records configuration changes made to AWS resources.

---

## 2. Configuration History

Maintains historical records of resource configurations.

---

## 3. Compliance Monitoring

Evaluates resources against AWS Config Rules.

---

## 4. Resource Relationships

Shows how AWS resources are connected.

Example:

```text
EC2
 │
 ├── Security Group
 │
 ├── IAM Role
 │
 ├── VPC
 │
 └── EBS Volume
```

---

## 5. Change Notifications

Detects configuration changes automatically.

---

## 6. Audit & Governance

Helps meet compliance standards such as:

- ISO
- PCI-DSS
- HIPAA
- SOC

---

## 7. AWS CloudTrail Integration

AWS Config works alongside CloudTrail to determine:

- What changed
- When it changed
- Who changed it

---

# 🔵 Common Use Cases

AWS Config is commonly used to:

- Monitor EC2 configurations
- Detect public S3 buckets
- Audit Security Groups
- Ensure EBS encryption
- Verify IAM policies
- Track infrastructure changes
- Generate compliance reports

---

# 🔵 Demo: Create a Custom AWS Config Rule Using AWS Lambda

## 🎯 Demo Objective

In this demo, we will:

- Create a Lambda function
- Add the custom compliance code
- Create an AWS Config Custom Lambda Rule
- Evaluate whether EC2 instances have **Detailed Monitoring** enabled
- View compliance results in AWS Config

---

# 🔵 Demo Architecture

```text
EC2 Instance
      │
      ▼
AWS Config Detects Change
      │
      ▼
Custom Config Rule
      │
      ▼
AWS Lambda
      │
      ▼
Checks EC2 Detailed Monitoring
      │
      ▼
COMPLIANT / NON_COMPLIANT
```

---

# 🔵 Step 1: Create a Lambda Function

Navigate to:

```text
AWS Console
      ↓
Lambda
      ↓
Create Function
```

Choose:

```text
Author from scratch
```

Example:

```text
Function Name

EC2-Detailed-Monitoring-Rule
```

Runtime:

```text
Python 3.x
```

Create the function.

---

# 🔵 Step 2: Add the Lambda Code

Replace the default Lambda code with the following:

```python
import json
import boto3

# AWS Clients
ec2_client = boto3.client("ec2")
config_client = boto3.client("config")

def lambda_handler(event, context):
    try:
        # Parse the AWS Config event
        invoking_event = json.loads(event["invokingEvent"])
        configuration_item = invoking_event["configurationItem"]

        # Get EC2 Instance ID
        instance_id = configuration_item["configuration"]["instanceId"]

        # Get EC2 instance details
        response = ec2_client.describe_instances(
            InstanceIds=[instance_id]
        )

        instance = response["Reservations"][0]["Instances"][0]

        # Check if Detailed Monitoring is enabled
        monitoring_state = instance["Monitoring"]["State"]

        if monitoring_state == "enabled":
            compliance_status = "COMPLIANT"
            annotation = "EC2 Detailed Monitoring is enabled."
        else:
            compliance_status = "NON_COMPLIANT"
            annotation = "EC2 Detailed Monitoring is not enabled."

        # Report compliance back to AWS Config
        evaluation = {
            "ComplianceResourceType": "AWS::EC2::Instance",
            "ComplianceResourceId": instance_id,
            "ComplianceType": compliance_status,
            "Annotation": annotation,
            "OrderingTimestamp": invoking_event["notificationCreationTime"]
        }

        response = config_client.put_evaluations(
            Evaluations=[evaluation],
            ResultToken=event["resultToken"]
        )

        return response

    except Exception as e:
        print(f"Error: {str(e)}")
        raise
```

Deploy the Lambda function.

---

# 🔵 Step 3: Copy the Lambda ARN

After deployment:

Navigate to:

```text
Lambda
     ↓
Configuration
     ↓
General Configuration
```

Copy the **Lambda ARN**.

Example:

```text
arn:aws:lambda:ap-south-1:123456789012:function:EC2-Detailed-Monitoring-Rule
```

You will use this ARN while creating the AWS Config Rule.

---

# 🔵 Step 4: Create a Custom AWS Config Rule

Navigate to:

```text
AWS Config
      ↓
Rules
      ↓
Add Rule
```

Choose:

```text
Create Custom Lambda Rule
```

Provide the following:

Rule Name

```text
ec2-detailed-monitoring-check
```

Description

```text
Checks whether EC2 Detailed Monitoring is enabled.
```

Trigger Type

```text
Configuration Changes
```

Resource Type

```text
AWS::EC2::Instance
```

Lambda Function ARN

Paste the Lambda ARN copied earlier.

Click:

```text
Save
```

AWS Config will now invoke your Lambda function whenever an EC2 instance configuration changes.

---

# 🔵 Step 5: Test the Rule

Create a new EC2 instance.

Initially:

```text
Detailed Monitoring

Disabled
```

AWS Config evaluates the instance.

Result:

```text
NON_COMPLIANT
```

Now enable Detailed Monitoring.

Navigate to:

```text
EC2
    ↓
Instances
    ↓
Actions
    ↓
Monitor and Troubleshoot
    ↓
Manage Detailed Monitoring
```

Enable:

```text
Detailed Monitoring
```

After AWS Config evaluates again:

```text
COMPLIANT
```

---

# 🔵 Understanding the Lambda Code

The Lambda function performs the following steps:

1. Receives an event from AWS Config.
2. Extracts the EC2 Instance ID.
3. Retrieves the EC2 instance details using the EC2 API.
4. Checks whether **Detailed Monitoring** is enabled.
5. Determines the compliance status:
   - **COMPLIANT** if monitoring is enabled.
   - **NON_COMPLIANT** if monitoring is disabled.
6. Reports the evaluation result back to AWS Config using `put_evaluations()`.

This allows AWS Config to automatically evaluate EC2 instances whenever their configuration changes.

---

# 🔵 Demo Flow Summary

```text
Create Lambda Function
        │
        ▼
Deploy Lambda Code
        │
        ▼
Copy Lambda ARN
        │
        ▼
Create Custom AWS Config Rule
        │
        ▼
Select EC2 Resource Type
        │
        ▼
Paste Lambda ARN
        │
        ▼
AWS Config Monitors EC2
        │
        ▼
Lambda Checks Detailed Monitoring
        │
        ▼
COMPLIANT / NON_COMPLIANT
```

---

# 🔵 Best Practices

- Enable AWS Config in all production Regions.
- Record all supported resources whenever possible.
- Use AWS Managed Rules for standard compliance checks.
- Create Custom Lambda Rules for organization-specific policies.
- Store configuration history in an encrypted S3 bucket.
- Use SNS notifications for compliance alerts.
- Monitor rule evaluations regularly.
- Combine AWS Config with CloudTrail for complete auditing.

---

# 🔵 Key Takeaway

**AWS Config** is a powerful governance and compliance service that continuously records AWS resource configurations, tracks configuration changes, and evaluates resources against predefined or custom rules. By combining **AWS Config** with **AWS Lambda**, you can automate compliance checks, enforce organization-specific policies, and maintain a secure, well-governed AWS environment.
