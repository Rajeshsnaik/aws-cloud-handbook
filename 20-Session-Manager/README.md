# 🔵 AWS Systems Manager Session Manager

![AWS](./session%20manager.png)

# 🔵 What is AWS Systems Manager Session Manager?

**AWS Systems Manager Session Manager (SSM Session Manager)** is a secure AWS service that allows you to connect to your EC2 instances **without using SSH, RDP, public IP addresses, or key pairs**.

Instead of opening inbound ports like **22 (SSH)** or **3389 (RDP)**, Session Manager creates a secure connection through the **AWS Systems Manager (SSM) service**.

> **Simple Definition**
>
> **Session Manager is a secure way to access and manage EC2 instances directly from the AWS Console or AWS CLI without exposing them to the internet.**

---

# 🔵 Main Uses of Session Manager

Session Manager is commonly used to:

- Securely connect to EC2 instances
- Eliminate the need for SSH keys
- Access private EC2 instances without a public IP
- Avoid opening inbound SSH (22) or RDP (3389) ports
- Record and audit user sessions (optional)
- Manage Linux and Windows instances
- Execute administrative tasks securely
- Improve security by reducing the attack surface

---

# 🔵 Why Use Session Manager?

Traditionally, connecting to an EC2 instance requires:

- Public IP address
- SSH key pair
- Port 22 open in the Security Group
- Bastion Host (for private instances)

With Session Manager, you **don't need any of these**.

Instead, the EC2 instance communicates **outbound** with the AWS Systems Manager service using the **SSM Agent**, and AWS securely establishes the session.

This makes your infrastructure more secure and easier to manage.

---

# 🔵 How Session Manager Works

```text
User
   │
   ▼
AWS Console / AWS CLI
   │
   ▼
AWS Systems Manager
   │
   ▼
SSM Agent (Running on EC2)
   │
   ▼
EC2 Instance
```

### Workflow

1. User starts a Session Manager session.
2. AWS verifies IAM permissions.
3. Systems Manager communicates with the SSM Agent running on the EC2 instance.
4. The SSM Agent establishes a secure outbound connection to AWS.
5. A secure shell session is created.

> **No SSH, no public IP, and no inbound security group rules are required.**

---

# 🔵 What is the SSM Agent?

The **Amazon SSM Agent** is software installed on an EC2 instance that enables communication with AWS Systems Manager.

It is responsible for:

- Receiving commands
- Starting Session Manager sessions
- Running automation tasks
- Sending inventory and patch information
- Reporting instance status

Without the SSM Agent, Session Manager cannot connect to the instance.

---

# 🔵 Is the SSM Agent Preinstalled?

For most modern AWS-managed AMIs, **yes**.

Common examples include:

- Amazon Linux 2
- Amazon Linux 2023
- Ubuntu AWS AMIs (most recent versions)
- Windows Server AWS AMIs

> **Note:** While many AWS-provided AMIs come with the SSM Agent preinstalled, some older or custom AMIs may not. Always verify that the agent is installed and running.

---

# 🔵 Check Whether the SSM Agent is Running

On Amazon Linux (or other Linux distributions using `systemd`), run:

```bash
sudo systemctl status amazon-ssm-agent
```

If the agent is running, you'll see:

```text
Active: active (running)
```

If it is stopped, start it:

```bash
sudo systemctl start amazon-ssm-agent
```

Enable it to start automatically on boot:

```bash
sudo systemctl enable amazon-ssm-agent
```

---

# 🔵 Demo: Connect to an EC2 Instance Using Session Manager

## Objective

Connect to an EC2 instance using **Session Manager** without:

- SSH
- Key Pair
- Port 22
- Public IP

---

# 🔵 Step 1: Launch an EC2 Instance

Create a new EC2 instance.

Configuration:

- **AMI:** Ubuntu
- **Instance Type:** t2.micro (or any supported instance type)
- **Key Pair:** **Optional** (not required for Session Manager)
- **Security Group:** No inbound rules are required for this demo.

Launch the instance.

---

# 🔵 Step 2: Attempt to Connect Using Session Manager

Go to:

```text
EC2
   ↓
Instances
   ↓
Select Instance
   ↓
Connect
   ↓
Session Manager
   ↓
Connect
```

At this point, you may see that the connection **fails**.

This is expected if the required configuration is missing.

---

# 🔵 Step 3: Verify the SSM Agent

If you can access the instance by another method (for example, SSH), verify that the SSM Agent is installed and running:

```bash
sudo systemctl status amazon-ssm-agent
```

If the service isn't running:

```bash
sudo systemctl start amazon-ssm-agent
```

Enable it at boot:

```bash
sudo systemctl enable amazon-ssm-agent
```

---

# 🔵 Step 4: Create an IAM Role for EC2

The EC2 instance must have permission to communicate with AWS Systems Manager.

Go to:

```text
IAM
   ↓
Roles
   ↓
Create Role
```

Choose:

```text
Trusted Entity

AWS Service
```

Use Case:

```text
EC2
```

Click **Next**.

Search for the AWS-managed policy:

```text
AmazonSSMManagedInstanceCore
```

Select the policy.

Click **Next**.

Provide a role name.

Example:

```text
EC2-SSM-Role
```

Click:

```text
Create Role
```

---

# 🔵 Step 5: Attach the IAM Role to the EC2 Instance

Go to:

```text
EC2
   ↓
Instances
   ↓
Select Instance
```

Choose:

```text
Actions
   ↓
Security
   ↓
Modify IAM Role
```

Select:

```text
EC2-SSM-Role
```

Click:

```text
Update IAM Role
```

---

# 🔵 Step 6: Wait a Few Minutes

After attaching the IAM role, it may take a few minutes for the permissions to propagate.

If the Session Manager connection still doesn't work immediately, wait briefly and try again.

---

# 🔵 Step 7: Connect Using Session Manager

Go to:

```text
EC2
   ↓
Instances
   ↓
Select Instance
   ↓
Connect
   ↓
Session Manager
   ↓
Connect
```

This time, the session should open successfully.

You now have shell access to the EC2 instance directly from your browser.

---

# 🔵 Security Group Comparison

### Traditional SSH Access

Requirements:

- Public IP
- Port 22 open
- SSH Key Pair

---

### Session Manager Access

Requirements:

- SSM Agent running
- IAM Role with `AmazonSSMManagedInstanceCore`
- Outbound internet access (or Systems Manager VPC endpoints for private environments)

No inbound SSH rule is required.

---

# 🔵 Advantages of Session Manager

- No SSH keys to manage
- No public IP required
- No inbound port 22 or 3389
- Secure browser-based terminal
- IAM-based access control
- Supports Linux and Windows
- Session logging and auditing (optional)
- Reduces the attack surface of EC2 instances
- Ideal for private subnet instances

---

# 🔵 Troubleshooting Checklist

If Session Manager doesn't connect:

- Verify the SSM Agent is installed and running.
- Ensure the EC2 instance has an IAM role with the `AmazonSSMManagedInstanceCore` policy.
- Wait a few minutes after attaching the IAM role.
- Confirm the instance can reach AWS Systems Manager endpoints (via internet, NAT Gateway, or VPC endpoints).
- Check that the instance status is **Running**.

---

# 🔵 Demo Flow Summary

```text
Launch EC2 (Ubuntu)
        │
        ▼
Try Session Manager
        │
        ▼
Connection Fails
        │
        ▼
Check SSM Agent
        │
        ▼
Create IAM Role
(AmazonSSMManagedInstanceCore)
        │
        ▼
Attach IAM Role to EC2
        │
        ▼
Wait a Few Minutes
        │
        ▼
Reconnect Using Session Manager
        │
        ▼
Secure Browser-Based Shell Access
```

---

# 🔵 In Short

**AWS Systems Manager Session Manager** provides a secure, IAM-controlled way to access EC2 instances **without SSH keys, public IP addresses, or inbound security group rules**. By using the **SSM Agent** and an IAM role with the **AmazonSSMManagedInstanceCore** policy, administrators can securely manage Linux and Windows instances through the AWS Console or CLI, making it a best practice for modern cloud and DevOps environments.
