# 🔐 AWS IAM (Identity and Access Management)

![AWS](./aws-iam.png)

> **A complete beginner-to-advanced guide to AWS IAM covering authentication, authorization, users, groups, roles, policies, STS, permission evaluation, and CLI commands.**

---

# 🔵 What is IAM?

**IAM (Identity and Access Management)** is an AWS service used to securely control who can access AWS resources and what actions they can perform.

---

# 🔵 Why IAM is Required?

IAM is required to provide secure access control within an AWS account.

---

# 🔵 Authentication vs Authorization

## Authentication

Authentication verifies **who you are**.

## Authorization

Authorization determines **what you can do**.

> Authentication verifies identity, while authorization determines what actions that identity is allowed to perform.

---

# 🔵 IAM Architecture

IAM works between users and AWS services.

```text
User/Application
        │
        ▼
Authentication
        │
        ▼
IAM
        │
        ▼
Authorization
        │
        ▼
AWS Service
```

> IAM authenticates the requester and then authorizes access based on attached policies before allowing access to AWS services.

---

# 🔵 STS (Security Token Service)

STS provides temporary security credentials for users and applications.

## Benefits

- Temporary Access
- More Secure
- Supports Cross-Account Access

---

# 🔵 Root User vs IAM User

## Root User

The root user is the AWS account owner created during AWS account creation.

### Characteristics

- Has full access to all AWS services.
- Cannot be restricted by IAM policies.
- Should be used only for critical account-level tasks.

---

## IAM User

An IAM User is an identity created within an AWS account.

### Characteristics

- Permissions can be controlled.
- Used for daily operations.
- Can belong to groups.
- Can have MFA enabled.

> Root user has unrestricted access to the AWS account and should rarely be used, whereas IAM users are created for day-to-day access with controlled permissions.

---

# 🔵 IAM Global Service

IAM is a **Global AWS Service**.

> IAM is a global service because users, groups, roles, and policies are available across all AWS regions within the same AWS account.

---

# 🔵 IAM Users

An IAM User is an identity created in AWS for a person or application that needs access to AWS resources. Permissions are assigned using IAM policies.

---

# 🔵 IAM Groups

An IAM Group is a collection of IAM users. Permissions are assigned to the group, and all users in the group inherit those permissions.

---

# 🔵 IAM Roles

An IAM Role is an AWS identity that provides temporary permissions to users, applications, or AWS services without using long-term credentials.

---

# 🔵 IAM Identity Lifecycle

The IAM Identity Lifecycle is the process of creating, managing, modifying, monitoring, and eventually removing IAM identities and their permissions throughout their usage period.

---

# 🔵 Access Keys

Access Keys consist of an **Access Key ID** and **Secret Access Key** used to authenticate AWS CLI, SDKs, APIs, and automation tools.

---

# 🔵 Console Access

Console Access allows users to sign in to the AWS Management Console using a username, password, and optionally MFA.

---

# 🔵 Programmatic Access

Programmatic Access allows applications, scripts, AWS CLI, and SDKs to access AWS services using Access Keys or temporary credentials.

---

# 🔵 What is a Policy?

An IAM Policy is a JSON document that defines permissions by specifying what actions are allowed or denied on AWS resources.

---

# 🔵 Policy JSON Structure

An IAM Policy is written in JSON format and contains elements such as **Version**, **Statement**, **Effect**, **Action**, **Resource**, and **Condition**.

## Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "*"
    }
  ]
}
```

---

# 🔵 Effect

The **Effect** element determines whether an action is allowed or denied.

### Values

- Allow
- Deny

---

# 🔵 Action

The **Action** element specifies the AWS operations that are allowed or denied.

### Examples

```text
s3:GetObject
ec2:StartInstances
iam:CreateUser
```

---

# 🔵 Resource

The **Resource** element specifies the AWS resource on which the action can be performed.

### Example

```text
arn:aws:s3:::my-bucket/*
```

---

# 🔵 Condition

The **Condition** element applies additional rules that must be satisfied before permissions are granted.

### Examples

- IP Address Restriction
- MFA Required
- Time-Based Access
- Tag-Based Access

---

# 🔵 Version

The **Version** element defines the policy language version being used.

### Current Version

```text
2012-10-17
```

---

# 🔵 Statement

A **Statement** is the main permission block inside a policy that contains **Effect**, **Action**, **Resource**, and optional **Conditions**.

A policy can have one or multiple statements.

---

# 🔵 Wildcards

Wildcards are special characters used to match multiple resources or actions.

### Examples

```text
*  → Matches everything

?  → Matches a single character
```

---

# 🔵 Variables

Variables are dynamic placeholders used in policies to reference information about the current user or request.

### Example

```text
${aws:username}
```

This automatically uses the current IAM user's name during policy evaluation.

---

# 🔵 AWS Managed Policies

AWS Managed Policies are pre-created and maintained by AWS.

AWS automatically updates these policies when new services or features are introduced.

### Examples

```text
AmazonS3ReadOnlyAccess
AdministratorAccess
AmazonEC2FullAccess
```

---

# 🔵 Customer Managed Policies

Customer Managed Policies are custom policies created and managed by customers to meet specific permission requirements.

They provide more flexibility and control than AWS Managed Policies.

---

# 🔵 Inline Policies

Inline Policies are policies directly attached to a single IAM User, Group, or Role and cannot be shared with other identities.

They are deleted automatically when the associated identity is deleted.

---

# 🔵 Resource-Based Policies

Resource-Based Policies are attached directly to AWS resources and define who can access that resource and what actions they can perform.

### Common Examples

```text
S3 Bucket Policy
SQS Queue Policy
SNS Topic Policy
KMS Key Policy
```

---

# 🔵 Permission Boundaries

Permission Boundaries define the maximum permissions an IAM User or Role can receive.

Even if a policy grants permissions, actions outside the boundary are denied.

> Think of it as a permissions limit or guardrail.

---

# 🔵 Session Policies

Session Policies are temporary policies applied during a role session created through AWS STS.

They further restrict permissions for that specific session without modifying the role's original permissions.

---

# 🔵 Service Control Policies (SCP)

Service Control Policies (SCPs) are policies used in AWS Organizations to define the maximum permissions available for AWS accounts within the organization.

SCPs do **not** grant permissions; they only limit what permissions IAM users and roles can have.

### Examples

```text
Deny EC2 Termination
Deny RDS Deletion
Restrict Specific AWS Regions
```

---

# 🔵 Implicit Deny

By default, all AWS requests are denied unless a policy explicitly allows them.

If no permission is granted, access is automatically denied.

---

# 🔵 Explicit Allow

An Explicit Allow occurs when an IAM policy specifically grants permission to perform an action on a resource.

---

# 🔵 Explicit Deny

An Explicit Deny occurs when a policy specifically denies an action.

> **Explicit Deny always overrides any Allow permissions.**

---

# 🔵 Policy Evaluation Logic

AWS evaluates permissions in the following order:

```text
Default = Implicit Deny
        │
        ▼
Check for Explicit Allow
        │
        ▼
Check for Explicit Deny
        │
        ▼
Final Decision
```

---

# 🔵 Multiple Policy Evaluation

When multiple policies are attached to a User, Group, or Role, AWS combines all permissions and evaluates them together.

---

# 🔵 Easy Memory Rule

```text
No Policy       = Implicit Deny

Allow Policy    = Explicit Allow

Deny Policy     = Explicit Deny

Explicit Deny Always Wins
```

---

# 🔵 What is a Role?

An IAM Role is an AWS identity that provides temporary permissions to users, applications, or AWS services without requiring long-term credentials.

Unlike IAM Users, roles do not have a username, password, or access keys.

---

# 🔵 Cross-Account Roles

Cross-Account Roles allow users or services in one AWS account to access resources in another AWS account without sharing credentials.

This is commonly used in multi-account AWS environments.

---

# 🔵 Service Roles

A Service Role is an IAM Role assumed by an AWS service to perform actions on your behalf.

### Examples

```text
EC2 Role
Lambda Execution Role
ECS Task Role
CodeBuild Role
```

---

# 🔵 Service-Linked Roles

A Service-Linked Role is a special IAM Role that is automatically created and managed by AWS for a specific service.

The role contains predefined permissions required for that service to operate correctly.

### Examples

```text
AWSServiceRoleForAutoScaling
AWSServiceRoleForECS
AWSServiceRoleForElasticLoadBalancing
```

AWS manages the permissions and lifecycle of service-linked roles automatically.

---

# 🔵 What is STS?

**AWS STS (Security Token Service)** is a service that provides temporary security credentials for accessing AWS resources.

These credentials automatically expire after a specified period, making them more secure than long-term access keys.

---

# 🔵 AWS CLI Commands

## User Commands

```bash
aws iam create-user
aws iam list-users
aws iam delete-user
```

---

## Group Commands

```bash
aws iam create-group
aws iam list-groups
aws iam delete-group
```

---

## Role Commands

```bash
aws iam create-role
aws iam list-roles
aws iam delete-role
```

---

## Policy Commands

```bash
aws iam create-policy
aws iam list-policies
aws iam delete-policy
```

---

## STS Commands

```bash
aws sts get-caller-identity
aws sts assume-role
aws sts get-session-token
```

---

# ⭐ Conclusion

AWS IAM is the foundation of security in AWS. By properly managing users, groups, roles, policies, and temporary credentials, you can implement the principle of **least privilege**, improve security, and control access to every AWS resource efficiently.
