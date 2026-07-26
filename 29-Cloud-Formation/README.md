# 🔵 AWS CloudFormation

![aws](./cloudformation.png)

**AWS CloudFormation (CFT)** is an **Infrastructure as Code (IaC)** service provided by AWS that allows you to create, update, and manage AWS infrastructure using code instead of creating resources manually through the AWS Console.

Instead of clicking through the AWS Console, you define your infrastructure in a template, and CloudFormation automatically provisions the required AWS resources.

---

# 🔵 What is Infrastructure as Code (IaC)?

**Infrastructure as Code (IaC)** is the process of managing and provisioning infrastructure using code instead of manually creating resources.

Instead of creating resources one by one:

- VPC
- EC2
- S3
- RDS
- IAM
- Load Balancer

You simply write code describing your infrastructure.

The IaC tool then communicates with the cloud provider and creates everything automatically.

---

# 🔵 How CloudFormation Works

Think of CloudFormation as a **middleman** between you and AWS.

```
User
   │
   ▼
CloudFormation Template (YAML / JSON)
   │
   ▼
AWS CloudFormation
   │
Converts Template into AWS API Calls
   │
   ▼
AWS Services
   │
   ├──► EC2
   ├──► VPC
   ├──► S3
   ├──► RDS
   ├──► IAM
   └──► Load Balancer
```

CloudFormation reads your template, converts it into AWS API calls, and creates the infrastructure automatically.

---

# 🔵 Simple Way to Understand

Imagine you want to build a house.

Without CloudFormation:

```
You

↓

Buy Bricks

↓

Hire Workers

↓

Build Room

↓

Build Kitchen

↓

Build Roof
```

Everything is done manually.

With CloudFormation:

```
You

↓

House Blueprint

↓

Construction Company

↓

Complete House
```

The blueprint tells the construction company exactly what to build.

Similarly,

CloudFormation reads your template and builds your AWS infrastructure.

---

# 🔵 Why CloudFormation?

CloudFormation provides several advantages.

- Infrastructure as Code (IaC)
- Automated resource creation
- Consistent infrastructure
- Version-controlled templates
- Easy updates
- Easy rollback
- Repeatable deployments
- Drift Detection
- AWS managed service

---

# 🔵 Why Use Infrastructure as Code?

Without IaC

```
Developer A

Creates VPC manually

↓

Developer B

Creates another VPC manually

↓

Different configurations
```

Results

- Human errors
- Different environments
- Difficult to reproduce

With IaC

```
Template

↓

Development

↓

Testing

↓

Production
```

All environments are created exactly the same.

---

# 🔵 Declarative Infrastructure

CloudFormation is a **Declarative IaC Tool**.

You only define:

> **What infrastructure you want.**

Example

```
I want:

1 VPC

2 Public Subnets

2 Private Subnets

1 Internet Gateway

2 EC2 Instances
```

CloudFormation decides:

- Which resource to create first
- Which resource depends on another
- Which AWS APIs to call

You don't write the creation sequence manually.

---

# 🔵 Version Control

CloudFormation templates are plain text files.

You can store them in Git.

Example

```
Git Repository

↓

cloudformation.yml

↓

Version 1

↓

Version 2

↓

Version 3
```

Benefits

- Track changes
- Rollback
- Team collaboration
- Code review

---

# 🔵 Supported Template Formats

CloudFormation supports two formats.

- YAML
- JSON

Example

```
CloudFormation

├── YAML ✅
└── JSON
```

Most engineers prefer **YAML**.

---

# 🔵 Why YAML?

YAML is preferred because it is:

- Easy to read
- Easy to write
- Supports comments
- Uses indentation
- Less verbose than JSON

Example

```yaml
Resources:
  MyEC2:
    Type: AWS::EC2::Instance
```

---

# 🔵 CloudFormation vs AWS CLI

| Feature                | AWS CLI                        | CloudFormation                 |
| ---------------------- | ------------------------------ | ------------------------------ |
| Purpose                | Execute AWS commands           | Create complete infrastructure |
| Resource Creation      | Usually one resource at a time | Multiple resources together    |
| Infrastructure as Code | ❌ No                          | ✅ Yes                         |
| Version Control        | ❌ No                          | ✅ Yes                         |
| Repeatable Deployment  | ❌ No                          | ✅ Yes                         |
| Drift Detection        | ❌ No                          | ✅ Yes                         |
| Best Use Case          | Quick tasks                    | Infrastructure deployment      |

---

# 🔵 When to Use AWS CLI?

Use AWS CLI when you want to perform quick operations.

Examples

- Launch one EC2 instance
- Upload files to S3
- List buckets
- Stop an EC2 instance
- Delete one resource

Example

```bash
aws s3 ls

aws ec2 describe-instances

aws s3 cp image.jpg s3://mybucket
```

CLI is best for quick commands and automation scripts.

---

# 🔵 When to Use CloudFormation?

Use CloudFormation when you need to create complete infrastructure.

Examples

- VPC
- EC2
- Security Groups
- RDS
- Load Balancer
- Auto Scaling
- IAM Roles

Everything is created together from one template.

---

# 🔵 CloudFormation vs Terraform

| Feature         | CloudFormation        | Terraform                               |
| --------------- | --------------------- | --------------------------------------- |
| Provider        | AWS                   | Multi-cloud                             |
| Developed By    | AWS                   | HashiCorp                               |
| Supports AWS    | ✅ Yes                | ✅ Yes                                  |
| Supports Azure  | ❌ No                 | ✅ Yes                                  |
| Supports GCP    | ❌ No                 | ✅ Yes                                  |
| Language        | YAML / JSON           | HCL                                     |
| Drift Detection | ✅ Built-in           | Supported through plan/state comparison |
| Best For        | AWS-only environments | Multi-cloud environments                |

---

# 🔵 When Should You Use CloudFormation?

Use CloudFormation when:

- Your infrastructure is only on AWS.
- You want deep integration with AWS services.
- You prefer an AWS-managed IaC solution.
- Your team mainly works within AWS.

---

# 🔵 When Should You Use Terraform?

Use Terraform when:

- You work with multiple cloud providers.
- You need a single tool for AWS, Azure, and GCP.
- Your infrastructure includes cloud and on-premises resources.
- You want one configuration language across different platforms.

---

# 🔵 CloudFormation Template Structure

A CloudFormation template contains several sections.

```
AWSTemplateFormatVersion

↓

Description

↓

Metadata

↓

Parameters

↓

Rules

↓

Resources

↓

Outputs
```

The **Resources** section is mandatory because it defines the AWS resources to create.

---

# 🔵 Template Sections

## AWSTemplateFormatVersion

Specifies the template version.

Example

```yaml
AWSTemplateFormatVersion: "2010-09-09"
```

---

## Description

Provides information about the template.

Example

```yaml
Description: Create an EC2 Instance
```

---

## Metadata

Stores additional information about the template.

---

## Parameters

Allows users to provide input values while creating the stack.

Examples

- EC2 Instance Type
- Key Pair
- VPC ID

---

## Rules

Validates parameter values before creating resources.

---

## Resources

The most important section.

Defines the AWS resources to create.

Example

- EC2
- S3
- IAM
- VPC
- RDS

---

## Outputs

Displays useful information after stack creation.

Examples

- EC2 Public IP
- Load Balancer DNS
- VPC ID

---

# 🔵 Example CloudFormation Template

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Description: Launch a Simple EC2 Instance

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance

    Properties:
      ImageId: ami-0123456789abcdef0

      InstanceType: t2.micro

      Tags:
        - Key: Name
          Value: My-EC2
```

---

# 🔵 Explanation of Each Line

```yaml
AWSTemplateFormatVersion: "2010-09-09"
```

Defines the CloudFormation template version.

---

```yaml
Description:
```

Describes the purpose of the template.

---

```yaml
Resources:
```

Starts the section where AWS resources are defined.

---

```yaml
MyEC2Instance:
```

Logical name of the EC2 instance inside the template.

---

```yaml
Type: AWS::EC2::Instance
```

Specifies the AWS resource type to create.

---

```yaml
Properties:
```

Defines the configuration for the EC2 instance.

---

```yaml
ImageId:
```

Specifies the Amazon Machine Image (AMI) used to launch the instance.

---

```yaml
InstanceType:
```

Defines the EC2 instance size.

Example

```
t2.micro
```

---

```yaml
Tags:
```

Adds metadata to the resource.

---

```yaml
Key: Name
Value: My-EC2
```

Assigns the Name tag shown in the AWS Console.

---

# 🔵 What is a Stack?

A **Stack** is a collection of AWS resources created and managed together using a single CloudFormation template.

Example

```
CloudFormation Stack

├── VPC
├── EC2
├── S3
├── IAM
├── Security Group
└── RDS
```

All resources are managed as one unit.

---

# 🔵 Create a Stack

## Step 1 – Create Stack

Open:

```
AWS Console

↓

CloudFormation

↓

Create Stack

↓

With New Resources
```

Upload your YAML template or specify an Amazon S3 URL containing the template.

---

## Step 2 – Specify Stack Details

Provide:

- Stack Name
- Parameter values (if any)

Example

```
Stack Name

↓

My-EC2-Stack
```

---

## Step 3 – Configure Stack Options

Configure optional settings such as:

- Tags
- IAM permissions
- Stack failure options
- Notifications
- Rollback behaviour

Then click **Next**.

---

## Step 4 – Review and Create

Review all settings.

Click:

```
Submit
```

CloudFormation starts creating the resources.

You can monitor progress in the **Events** tab.

---

# 🔵 What is Drift Detection?

**Drift Detection** checks whether the actual AWS resources match the CloudFormation template.

If someone manually changes a resource outside CloudFormation, the resource is considered **drifted**.

---

# 🔵 How Drift Happens

Suppose your CloudFormation template contains:

```yaml
VersioningConfiguration:
  Status: Enabled
```

CloudFormation creates the S3 bucket with versioning enabled.

Everything matches.

```
Template

↓

Versioning Enabled

↓

AWS S3

↓

Versioning Enabled
```

Now someone opens the AWS Console and manually disables versioning.

```
Template

↓

Versioning Enabled

↓

AWS Console

↓

Versioning Disabled
```

Now the actual resource no longer matches the template.

This is called **Configuration Drift**.

---

# 🔵 Detecting Drift

Open:

```
CloudFormation

↓

Select Stack

↓

Actions

↓

Detect Drift
```

CloudFormation compares the template with the actual AWS resources.

Example result

```
Template

↓

Versioning Enabled

≠

Actual Resource

↓

Versioning Disabled

↓

Status

DRIFTED
```

CloudFormation identifies which resources have changed and displays the differences.

---

# 🔵 Correcting Drift

To restore the infrastructure to the desired state:

- Update the stack using the correct template.
- Or manually modify the resource to match the template.

This ensures the infrastructure matches the Infrastructure as Code definition.

---

# 🔵 Uploading Templates from VS Code

Most engineers write CloudFormation templates locally using **Visual Studio Code**.

Workflow

```
VS Code

↓

cloudformation.yaml

↓

Git Repository (Optional)

↓

AWS CloudFormation

↓

Create Stack

↓

Upload YAML File

↓

AWS Infrastructure
```

You write and maintain the template in VS Code, store it in Git for version control, and then upload it to CloudFormation through the AWS Console or deploy it using the AWS CLI.

---

# 🔵 Best Practices

- Use YAML instead of JSON.
- Store templates in Git.
- Use Parameters instead of hardcoding values.
- Add Tags to resources.
- Keep templates modular and reusable.
- Run Drift Detection regularly.
- Review the AWS documentation before using new resources or properties.

> **Note:** Always refer to the official AWS documentation for the latest CloudFormation resource types, properties, syntax, and best practices, as AWS frequently introduces new features and updates.
