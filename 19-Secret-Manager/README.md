# Secret Management in AWS

![AWS](./secret-manager.png)

Modern applications require many sensitive credentials to function securely, such as database passwords, API keys, and access tokens. Instead of hardcoding these values in your application or storing them in configuration files, they should be stored in a secure secret management solution.

The three most common secret management solutions used in AWS environments are:

1. **AWS Systems Manager Parameter Store**
2. **AWS Secrets Manager**
3. **HashiCorp Vault** (Third-party, not managed by AWS)

---

# When Do We Use These?

| Service                             | Best Used For                                                                                                           |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Systems Manager Parameter Store** | Configuration values, application settings, environment variables, simple secrets that don't require automatic rotation |
| **AWS Secrets Manager**             | Database passwords, API keys, tokens, credentials that require automatic rotation and lifecycle management              |
| **HashiCorp Vault**                 | Enterprise environments, multi-cloud, hybrid cloud, Kubernetes, dynamic secrets, advanced access control                |

---

# AWS Secrets Manager

---

# What is AWS Secrets Manager?

**AWS Secrets Manager** is a fully managed AWS service that securely stores, manages, and retrieves sensitive information such as passwords, API keys, database credentials, OAuth tokens, and other application secrets.

Secrets are encrypted using **AWS Key Management Service (KMS)** and can be accessed securely by applications using IAM permissions.

> **Simple Definition**
>
> **AWS Secrets Manager is a secure vault for storing and managing sensitive credentials with built-in encryption, versioning, and automatic secret rotation.**

---

# Purpose of AWS Secrets Manager

Secrets Manager is designed to securely manage credentials throughout their lifecycle.

It helps you:

- Store application secrets securely
- Protect passwords and API keys
- Rotate credentials automatically
- Avoid hardcoding secrets in source code
- Control access using IAM
- Retrieve secrets programmatically
- Improve security and compliance

---

# Is Secrets Manager Regional or Global?

**AWS Secrets Manager is a Regional service.**

This means:

- Secrets are created within a specific AWS Region.
- Applications retrieve secrets from that Region.
- Secrets are **not automatically shared across Regions**.
- If needed, you can configure **multi-Region secret replication** to keep copies synchronized.

Example:

```text
ap-south-1
     │
     ├── DatabasePassword
     ├── StripeAPIKey
     └── JWTSecret

us-east-1
     │
     ├── Different Secret
     └── Or Replicated Secret
```

---

# Why Use Secrets Manager Instead of Parameter Store?

Both services can store encrypted values, but Secrets Manager is specifically built for managing **sensitive credentials**.

Secrets Manager provides features such as:

- Automatic secret rotation
- Secret version management
- Built-in integration with databases
- Secret replication across Regions
- Lifecycle management
- Native integration with Lambda rotation functions

Parameter Store focuses more on application configuration rather than credential lifecycle management.

---

# Parameter Store vs Secrets Manager

| Feature                      | Parameter Store                                                | Secrets Manager                                   |
| ---------------------------- | -------------------------------------------------------------- | ------------------------------------------------- |
| Primary Purpose              | Configuration & parameters                                     | Sensitive secrets & credentials                   |
| Secure Storage               | ✅                                                             | ✅                                                |
| KMS Encryption               | ✅                                                             | ✅                                                |
| Automatic Secret Rotation    | ❌                                                             | ✅                                                |
| Secret Versioning            | Basic parameter versions                                       | Full secret version lifecycle                     |
| Database Credential Rotation | ❌                                                             | ✅                                                |
| Multi-Region Replication     | ❌                                                             | ✅                                                |
| Cost                         | Standard tier available at no cost (advanced tier has charges) | Paid service                                      |
| Best For                     | Environment variables, application configs                     | Passwords, API keys, database credentials, tokens |

---

# Features of AWS Secrets Manager

---

## 1. Secure Storage

Secrets are securely stored instead of being hardcoded in source code or configuration files.

Examples:

- Database passwords
- API Keys
- JWT Secrets
- OAuth Tokens
- SMTP Credentials

---

## 2. Encryption with AWS KMS

Every secret is encrypted using **AWS Key Management Service (KMS)**.

Benefits:

- Encryption at rest
- Secure access
- Customer-managed or AWS-managed KMS keys
- Compliance with security standards

---

## 3. Automatic Secret Rotation

One of the biggest advantages of Secrets Manager.

Secrets can be rotated automatically without manual intervention.

Supported examples:

- Amazon RDS
- Amazon Aurora
- Other databases (using Lambda rotation functions)

Example:

```text
Old Password
      │
      ▼
Lambda Rotation
      │
      ▼
New Password
      │
      ▼
Application Continues Using New Secret
```

Applications continue working without exposing credentials.

---

## 4. Version Management

Whenever a secret changes, Secrets Manager creates a new version.

This allows you to:

- Track secret history
- Roll back if necessary
- Maintain previous versions during rotation

---

## 5. Fine-Grained IAM Access Control

Access to secrets is controlled using IAM.

Examples:

- Developer can read only development secrets.
- Production applications can access only production secrets.
- Administrators manage all secrets.

---

## 6. Resource-Based Policies

Secrets can also use resource policies to control cross-account access when required.

---

## 7. Native AWS Service Integration

Secrets Manager integrates with many AWS services:

- Lambda
- ECS
- EKS
- EC2
- RDS
- Aurora
- CodeBuild
- CodePipeline

Applications can retrieve secrets securely at runtime.

---

## 8. CloudTrail Integration

Every API call related to Secrets Manager is logged in AWS CloudTrail.

This helps with:

- Auditing
- Compliance
- Security investigations

---

## 9. Multi-Region Secret Replication

Secrets can be replicated to other AWS Regions, making them available closer to applications and improving disaster recovery.

---

## 10. SDK & CLI Access

Secrets can be retrieved using:

- AWS SDK
- AWS CLI
- AWS CLI Profiles
- IAM Roles

No need to expose credentials in application code.

---

# Common Use Cases

Store:

- Database passwords
- API Keys
- Stripe Secret Keys
- GitHub Tokens
- JWT Secret Keys
- SMTP Credentials
- OAuth Client Secrets
- Third-party service credentials

---

# Demo: Create and Retrieve a Secret Using AWS Secrets Manager

## Demo Objective

In this demo, we will:

- Create a new secret
- Store a database password securely
- Retrieve the secret using the AWS Console
- Retrieve the same secret using the AWS CLI

---

# Step 1: Open AWS Secrets Manager

Navigate to:

```text
AWS Console
      ↓
Secrets Manager
```

Click:

```text
Store a new secret
```

---

# Step 2: Choose Secret Type

Select:

```text
Other type of secret
```

You can also choose predefined templates such as:

- Credentials for Amazon RDS
- Credentials for Amazon Redshift
- Credentials for Amazon DocumentDB

For this demo, use:

```text
Other type of secret
```

---

# Step 3: Add Secret Values

Example:

```text
Key               Value
-----------------------------
username          admin
password          MySecurePassword@123
```

Click **Next**.

---

# Step 4: Configure the Secret

Provide a secret name.

Example:

```text
prod/database/mysql
```

(Optional) Add a description:

```text
MySQL production database credentials
```

Click **Next**.

---

# Step 5: Configure Rotation

For this demo:

```text
Disable Automatic Rotation
```

(You can enable it later using AWS Lambda.)

Click **Next**.

---

# Step 6: Review and Create

Review the configuration.

Click:

```text
Store
```

The secret is now securely stored and encrypted with AWS KMS.

---

# Step 7: View the Secret

Open the newly created secret.

Initially, the secret value is hidden.

Click:

```text
Retrieve secret value
```

You'll see:

```json
{
  "username": "admin",
  "password": "MySecurePassword@123"
}
```

---

# Step 8: Retrieve the Secret Using AWS CLI

First, configure the AWS CLI if you haven't already.

Run:

```bash
aws configure
```

Retrieve the secret:

```bash
aws secretsmanager get-secret-value \
    --secret-id prod/database/mysql
```

To display only the secret string:

```bash
aws secretsmanager get-secret-value \
    --secret-id prod/database/mysql \
    --query SecretString \
    --output text
```

The CLI returns the stored JSON string containing the secret values.

---

# Step 9: Use the Secret in Applications

Applications should retrieve secrets at runtime instead of hardcoding them.

Typical flow:

```text
Application
      │
      ▼
AWS SDK / AWS CLI
      │
      ▼
AWS Secrets Manager
      │
      ▼
KMS Decryption
      │
      ▼
Secret Returned Securely
```

The application uses the secret in memory without storing it in source code.

---

# Best Practices

- Never hardcode secrets in your application.
- Use IAM roles instead of long-term access keys.
- Enable automatic rotation for supported credentials.
- Use customer-managed KMS keys when required.
- Grant least-privilege IAM permissions.
- Monitor access using CloudTrail.
- Delete unused secrets to reduce cost.

---

# Demo Flow Summary

```text
Open Secrets Manager
        │
        ▼
Store New Secret
        │
        ▼
Choose Secret Type
        │
        ▼
Enter Secret Values
        │
        ▼
Create Secret
        │
        ▼
Encrypted with AWS KMS
        │
        ▼
Retrieve Secret
(Console / AWS CLI / SDK)
        │
        ▼
Application Uses Secret Securely
```

---

# Key Takeaway

**AWS Secrets Manager** is the recommended AWS service for managing **passwords, API keys, tokens, and database credentials**. It provides **secure storage, KMS encryption, automatic secret rotation, version management, IAM-based access control, and seamless integration with AWS services**. Compared to **Systems Manager Parameter Store**, Secrets Manager is purpose-built for handling sensitive credentials and managing their lifecycle, making it the preferred choice for production applications that require strong security and automation.
