# 🔵 Parameter Store

![AWS](./Parameter-store.png)

# 🔵 What is Parameter Store?

**AWS Parameter Store** is a service within **AWS Systems Manager (SSM)** that securely stores and manages configuration data and sensitive information such as API keys, database credentials, JWT secrets, environment variables, and application settings. Instead of hardcoding these values in your code, you store them in Parameter Store and retrieve them securely when needed, improving security and simplifying configuration management.

### Benefits of AWS Parameter Store

- **Centralized** – Store all configuration values and secrets in one place.
- **Secure** – Encrypt sensitive data using AWS KMS.
- **IAM Controlled** – Manage access with AWS IAM permissions.
- **Versioning** – Track and restore previous parameter versions.
- **Audit Logging** – Monitor access and changes using AWS CloudTrail.
- **Easy Rotation** – Update secrets and configuration values without changing application code.

---

# 🔵 Parameter Types in AWS Parameter Store

### 1. String

Stores a single text value.

Example:

```text
3000
```

### 2. StringList

Stores multiple values separated by commas.

Example:

```text
us-east-1, us-west-1, ap-south-1
```

### 3. SecureString

Stores sensitive data in encrypted form.

Examples:

```text
JWT_SECRET
DB_PASSWORD
API_KEY
```

Uses **AWS KMS** to encrypt data before storing it.

---

# 🔵 Creating Parameters

## 1. Using AWS Console

```text
Systems Manager
    → Parameter Store
        → Create Parameter
```

Example:

```text
Name:
/nextjs/prod/mongodb-uri

Type:
SecureString

Value:
mongodb+srv://...
```

---

## 2. Using CLI Commands

Before using AWS CLI, ensure the user has the required IAM permissions to perform Parameter Store actions.

### Create String

```bash
aws ssm put-parameter \
--name "/nextjs/dev/app-name" \
--value "NextJS-App" \
--type String
```

---

### Create Secure String

```bash
aws ssm put-parameter \
--name "/nextjs/prod/jwt-secret" \
--value "mysecret123" \
--type SecureString
```

---

# 🔵 Retrieve Parameter

### 1. Single Parameter

```bash
aws ssm get-parameter \
--name "/nextjs/prod/jwt-secret" \
--with-decryption
```

Output:

```json
{
  "Parameter": {
    "Name": "/nextjs/prod/jwt-secret",
    "Value": "mysecret123"
  }
}
```

---

### 2. Multiple Parameters

```bash
aws ssm get-parameters
```

---

### 3. By Path

```bash
aws ssm get-parameters-by-path \
--path "/nextjs/prod" \
--recursive
```

Very useful for applications.

---

# 🔵 Parameter Hierarchy

Parameter Store supports hierarchical naming using paths.

Example:

```text
/nextjs/dev/mongodb-uri
/nextjs/dev/jwt-secret

/nextjs/qa/mongodb-uri
/nextjs/qa/jwt-secret

/nextjs/prod/mongodb-uri
/nextjs/prod/jwt-secret
```

Benefits:

- Environment separation
- Easier management
- Fetch multiple parameters by path
- Better organization

---

# 🔵 Parameter Tiers

AWS Parameter Store supports three tiers.

## Standard Tier

- Default option
- Up to 10,000 parameters
- Lower cost
- Suitable for most applications

## Advanced Tier

- Larger parameter size
- More parameters
- Additional features
- Additional cost

## Intelligent-Tiering

- AWS automatically chooses Standard or Advanced based on usage.

---

# 🔵 Parameter Versioning

Every parameter update creates a new version.

Example:

```text
Version 1
JWT_SECRET=secret123

Version 2
JWT_SECRET=secret456

Version 3
JWT_SECRET=secret789
```

Benefits:

- Rollback support
- Audit tracking
- Secret rotation

Retrieve a specific version:

```bash
aws ssm get-parameter \
--name "/nextjs/prod/jwt-secret" \
--version 2
```

---

# 🔵 Parameter Store vs Secrets Manager

| Feature                      | Parameter Store      | Secrets Manager  |
| ---------------------------- | -------------------- | ---------------- |
| Cost                         | Lower                | Higher           |
| Secure Storage               | Yes                  | Yes              |
| KMS Encryption               | Yes                  | Yes              |
| Automatic Rotation           | No                   | Yes              |
| Database Credential Rotation | No                   | Yes              |
| Best For                     | Config & App Secrets | Critical Secrets |
