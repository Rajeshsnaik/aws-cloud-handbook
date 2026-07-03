# AWS Command Line Interface (CLI)

![AWS](./aws-cli.png)

# 🔵 What is AWS CLI?

**AWS Command Line Interface (CLI)** is an open-source tool provided by AWS that allows you to interact with AWS services directly from your terminal or command-line shell. Instead of using the AWS Management Console (GUI), you can execute commands to manage your cloud resources.

Using AWS CLI, you can:

- Create AWS resources
- Update existing resources
- Delete AWS resources
- Automate repetitive administrative tasks
- Manage large-scale cloud infrastructure
- Integrate AWS operations into Shell Scripts
- Integrate AWS with CI/CD pipelines
- Manage multiple AWS accounts efficiently

AWS CLI communicates with AWS services by sending API requests on your behalf.

---

# 🔵 Why Use AWS CLI?

Although the **AWS Management Console** provides an easy-to-use graphical interface, AWS CLI offers several advantages, especially for developers, DevOps engineers, and cloud administrators.

Benefits include:

- Faster than navigating through the Console
- Automates repetitive tasks
- Reduces manual configuration errors
- Easy scripting using Bash, PowerShell, or Python
- Suitable for Infrastructure Automation
- Works well with CI/CD pipelines
- Can manage thousands of resources quickly
- Makes cloud operations repeatable and consistent

---

# 🔵 AWS CLI Architecture

AWS CLI acts as a bridge between your computer and AWS services.

```
User / Terminal
       │
       ▼
    AWS CLI
       │
       ▼
    AWS API
       │
       ▼
  AWS Service
```

### Flow Explanation

1. The user enters a command in the terminal.
2. AWS CLI validates the command.
3. AWS CLI converts the command into an AWS API request.
4. The API request is securely sent to AWS.
5. AWS performs the requested operation.
6. AWS returns the response.
7. AWS CLI displays the result in the terminal.

---

# 🔵 Installation Guide

## Linux (Generic x86_64)

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install
```

---

## Ubuntu / Debian

```bash
sudo apt update

sudo apt install awscli -y
```

---

## Windows

1. Download the official 64-bit MSI installer.
2. Run the installer.
3. Follow the installation wizard.
4. Finish the installation.

---

## macOS

```bash
brew install awscli
```

---

## Verify Installation

Check whether AWS CLI is installed correctly.

```bash
aws --version
```

Example output:

```text
aws-cli/2.x.x Python/3.x Linux/Windows/macOS
```

---

# 🔵 Configure AWS CLI

Before configuring AWS CLI, you must:

- Create an IAM User
- Assign required permissions
- Generate an Access Key ID
- Generate a Secret Access Key

Run:

```bash
aws configure
```

AWS CLI asks four questions.

```text
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]:
Default output format [None]:
```

Example:

```text
AWS Access Key ID [None]: ABC123EXAMPLEKEY
AWS Secret Access Key [None]: XYZ123EXAMPLESECRET
Default region name [None]: ap-south-1
Default output format [None]: json
```

After configuration, AWS CLI stores these values locally.

---

# 🔵 Configuration Files

AWS CLI automatically creates a hidden folder named:

```text
~/.aws/
```

Inside this folder are two important files.

---

## 1. Credentials File

Location:

```text
~/.aws/credentials
```

Purpose:

Stores authentication credentials.

Example:

```ini
[default]
aws_access_key_id=ABC123EXAMPLEKEY
aws_secret_access_key=XYZ123EXAMPLESECRET
```

---

## 2. Config File

Location:

```text
~/.aws/config
```

Purpose:

Stores operational settings.

Example:

```ini
[default]
region=ap-south-1
output=json
```

---

# 🔵 AWS Profiles

AWS Profiles allow you to configure multiple AWS accounts or environments on the same computer.

Examples:

- Development
- Testing
- Staging
- Production

---

## Create a Named Profile

```bash
aws configure --profile dev
```

AWS CLI stores another profile.

Example:

```ini
[dev]
aws_access_key_id=ABC789DEVKEY
aws_secret_access_key=XYZ789DEVSECRET
```

---

## Use a Named Profile

```bash
aws s3 ls --profile dev
```

AWS CLI will use the credentials stored under the **dev** profile.

---

# 🔵 Verify Identity

AWS provides **Security Token Service (STS)** to verify which IAM identity is currently authenticated.

Command:

```bash
aws sts get-caller-identity
```

Example Output:

```json
{
  "UserId": "AIDAXXXXXXXXXXXXX",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/devops-user"
}
```

This command is useful for:

- Confirming login identity
- Troubleshooting permission issues
- Verifying the active AWS account
- Checking which profile is currently being used

---

# 🔵 Core Command Syntax

Every AWS CLI command follows the same structure.

```text
aws <service> <operation> <parameters>
```

Examples:

```bash
aws s3 ls

aws s3 help

aws iam list-users
```

---

# 🔵 Global Options

Global options can be used with almost every AWS CLI command.

| Option           | Description                  |
| ---------------- | ---------------------------- |
| `--profile`      | Use a specific AWS profile   |
| `--region`       | Override the default region  |
| `--output`       | Change output format         |
| `--query`        | Filter output using JMESPath |
| `--debug`        | Show detailed request logs   |
| `--no-cli-pager` | Disable output pager         |
| `--version`      | Show AWS CLI version         |
| `help`           | Display command help         |

Example:

```bash
aws ec2 describe-instances --profile dev --region us-east-1
```

---

# 🔵 Advanced Output Formats

AWS CLI supports multiple output formats.

---

## JSON (Default)

```bash
aws s3 ls --output json
```

---

## Table

```bash
aws s3 ls --output table
```

---

## Text

```bash
aws s3 ls --output text
```

Choose the format that best suits your scripting or readability needs.

---

# 🔵 Region Selection

AWS resources exist inside Regions.

Your default Region is configured during `aws configure`.

Sometimes you need to run commands in another Region.

---

## Temporary Region Override

```bash
aws ec2 describe-instances --region us-east-1
```

This affects only the current command.

---

## Permanent Region Change

Run:

```bash
aws configure
```

Then enter a new default Region.

---

# 🔵 Environment Variables

Instead of storing credentials in files, AWS CLI can read credentials from environment variables.

This approach is commonly used in:

- CI/CD pipelines
- Temporary sessions
- Docker containers
- Kubernetes
- GitHub Actions

---

## Set Access Key

```bash
export AWS_ACCESS_KEY_ID=XXXX
```

---

## Set Secret Key

```bash
export AWS_SECRET_ACCESS_KEY=XXXX
```

---

## Set Region

```bash
export AWS_DEFAULT_REGION=ap-south-1
```

---

## Verify Environment Variables

```bash
env | grep AWS
```

Environment variables only exist for the current shell session unless permanently configured.

---

# 🔵 AWS CLI Autocomplete

AWS CLI supports tab completion to make command writing faster.

Enable autocomplete:

```bash
complete -C aws_completer aws
```

Example:

Type:

```bash
aws ec2
```

Press the **TAB** key twice to display all available EC2 commands.

Benefits:

- Faster typing
- Fewer spelling mistakes
- Discover available commands
- Improved productivity

---

# 🔵 Useful EC2 Commands

## List EC2 Instances

```bash
aws ec2 describe-instances
```

---

## Start an EC2 Instance

```bash
aws ec2 start-instances --instance-ids i-12345
```

---

## Stop an EC2 Instance

```bash
aws ec2 stop-instances --instance-ids i-12345
```

---

## Reboot an EC2 Instance

```bash
aws ec2 reboot-instances --instance-ids i-12345
```

---

## Terminate an EC2 Instance

```bash
aws ec2 terminate-instances --instance-ids i-12345
```

---

# 🔵 Useful S3 Commands

## List Buckets

```bash
aws s3 ls
```

---

## Create Bucket

```bash
aws s3 mb s3://mybucket-unique-name
```

---

## Upload File

```bash
aws s3 cp file.txt s3://mybucket-unique-name
```

---

## Download File

```bash
aws s3 cp s3://mybucket-unique-name/file.txt .
```

---

## Sync Local Folder

```bash
aws s3 sync ./website s3://mybucket-unique-name
```

---

# 🔵 Useful IAM Commands

## List Users

```bash
aws iam list-users
```

---

## Create User

```bash
aws iam create-user --user-name devops-user
```

---

## Delete User

```bash
aws iam delete-user --user-name devops-user
```

---

# 🔵 Useful CloudWatch Commands

## List Metrics

```bash
aws cloudwatch list-metrics
```

---

## Get Metric Statistics

```bash
aws cloudwatch get-metric-statistics
```

---

# 🔵 Useful ECS Commands

## List Clusters

```bash
aws ecs list-clusters
```

---

## List Services

```bash
aws ecs list-services --cluster mycluster
```

---

# 🔵 Useful EKS Commands

## Configure kubeconfig

```bash
aws eks update-kubeconfig \
--region ap-south-1 \
--name production-cluster
```

---

## Check Kubernetes Nodes

```bash
kubectl get nodes
```

---

# 🔵 Troubleshooting & Diagnostics

## Debug Mode

To inspect every API request, response, headers, payload, SSL handshake, retries, and execution details, use the `--debug` option.

```bash
aws s3 ls --debug
```

This is extremely useful for troubleshooting authentication, networking, and permission-related issues.

---

## Exit Codes

AWS CLI returns standard exit codes after each command execution.

| Exit Code | Meaning                                                                 |
| --------- | ----------------------------------------------------------------------- |
| **0**     | Command completed successfully                                          |
| **1**     | Operational failure (Access Denied, invalid parameters, service errors) |
| **255**   | Command parsing error, missing dependencies, or CLI configuration issue |

Check the exit status of the last executed command:

```bash
aws s3 ls

echo $?
```

---

# 🔵 Best Practices

- Never share your Access Key ID or Secret Access Key.
- Use IAM Roles whenever possible instead of long-term access keys.
- Follow the Principle of Least Privilege by granting only the permissions required.
- Enable Multi-Factor Authentication (MFA) for IAM users.
- Rotate Access Keys regularly.
- Use named profiles to manage multiple AWS accounts.
- Store sensitive credentials securely and avoid hardcoding them in scripts.
- Prefer environment variables or IAM Roles for temporary credentials.
- Use the `--debug` option only for troubleshooting, as it may expose sensitive request details.
- Verify your identity with `aws sts get-caller-identity` before performing critical operations.
