# Amazon EC2 (Elastic Compute Cloud)

![AWS](./aws-ec2.png)

Amazon EC2 (Elastic Compute Cloud) is one of the core AWS compute services that provides **secure, scalable, and resizable virtual servers** in the cloud.

It enables you to launch servers within minutes without purchasing or maintaining physical hardware. You can choose the operating system, CPU, memory, storage, networking, and security settings based on your application requirements.

---

# 🔵 What is EC2?

**Amazon EC2 (Elastic Compute Cloud)** is an AWS service that provides virtual servers (called **Instances**) on demand.

### Purpose

EC2 is used to run:

- Web Applications
- Websites
- REST APIs
- Backend Services
- Databases
- Enterprise Applications
- Development & Testing Environments
- Machine Learning Workloads

Instead of purchasing physical servers, AWS allows you to create and manage servers in just a few minutes.

---

# 🔵 Benefits of EC2

- Launch servers within minutes
- Pay only for what you use
- Easily scale resources up or down
- High availability across multiple Availability Zones
- Secure networking using VPC and Security Groups
- Supports Linux and Windows operating systems
- Integrates with almost every AWS service

---

# 🔵 EC2 Components

Every EC2 instance consists of several important components.

## AMI (Amazon Machine Image)

An operating system template used to launch an EC2 instance.

Examples:

```text
Ubuntu
Amazon Linux
Red Hat Enterprise Linux (RHEL)
Windows Server
Debian
```

---

## Instance Type

Determines the CPU, memory, storage, and networking capacity of the instance.

Examples:

```text
t2.micro
t3.medium
m5.large
c7g.large
r6i.xlarge
```

---

## Storage

Provides persistent or temporary storage for the instance.

```text
EBS Volumes
Instance Store
```

---

## Network

Networking components used by EC2.

```text
VPC
Subnet
Route Table
Internet Gateway
Elastic IP
Security Group
Network ACL
```

---

## Key Pair

Used to securely connect to Linux EC2 instances using SSH.

AWS stores:

- Public Key
- Private Key (.pem)

Example:

```bash
ssh -i my-key.pem ubuntu@public-ip
```

---

## Security Group

Acts as a **virtual firewall** for an EC2 instance.

Controls:

- Incoming (Inbound) traffic
- Outgoing (Outbound) traffic

Common Ports:

| Port | Service       |
| ---- | ------------- |
| 22   | SSH           |
| 80   | HTTP          |
| 443  | HTTPS         |
| 3389 | RDP (Windows) |

---

# 🔵 EC2 Lifecycle

Every EC2 instance goes through different lifecycle states.

## Pending

The instance is being launched.

- Hardware is allocated
- Operating System starts booting

---

## Running

The instance is active and available.

- Applications can run
- Users can connect
- Billing for compute begins

---

## Stopping

AWS is shutting down the operating system.

---

## Stopped

The instance is powered off.

- Compute billing stops
- EBS storage charges continue
- Instance can be started again

---

## Rebooting

The operating system restarts.

- Public IP remains the same (unless using dynamic IP)
- Data on EBS remains unchanged

---

## Terminated

The instance is permanently deleted.

- Cannot be recovered
- Attached Instance Store data is lost
- Root EBS volume is deleted (unless configured otherwise)

---

# 🔵 What is AMI?

An **Amazon Machine Image (AMI)** is a pre-configured template used to launch EC2 instances.

It contains everything required to start a virtual server.

### AMI Contains

- Operating System
- Installed Applications
- Software Packages
- System Configuration
- Storage Configuration
- Startup Permissions

### Types of AMIs

- AWS Provided AMIs
- AWS Marketplace AMIs
- Community AMIs
- Custom AMIs

---

# 🔵 Amazon EBS (Elastic Block Store)

Amazon EBS is **persistent block storage** for EC2 instances.

Think of it as a virtual hard disk attached over the network.

It stores:

- Operating System
- Applications
- Databases
- User Files
- Configuration Data

### Features

- Persistent Storage
- Attach and Detach Volumes
- Resize Volumes
- Change Volume Types
- Encryption Support
- Snapshot Backup
- High Availability

### Common Volume Types

- gp3
- gp2
- io2
- io1
- st1
- sc1

---

# 🔵 EBS Snapshots

An **EBS Snapshot** is a backup of an EBS volume stored in Amazon S3.

Snapshots can be used to:

- Restore volumes
- Create new volumes
- Create AMIs
- Disaster Recovery
- Cross-Region Backup

Snapshots are **incremental**, meaning only changed blocks are stored after the first snapshot.

---

# 🔵 Instance Store

Instance Store (Ephemeral Storage) is temporary storage physically attached to the host machine.

It provides extremely fast read/write performance.

### Best Use Cases

- Cache
- Temporary Files
- Scratch Data
- Buffers
- High-Speed Processing

### Important Notes

- Data is **NOT Persistent**
- Data is lost if the instance stops or terminates
- Cannot be detached
- No snapshots
- No backup support

---

# 🔵 EBS vs Instance Store

| Feature     | EBS                  | Instance Store |
| ----------- | -------------------- | -------------- |
| Persistent  | Yes                  | No             |
| Backup      | Snapshots            | No             |
| Resize      | Yes                  | No             |
| Detach      | Yes                  | No             |
| Performance | High                 | Very High      |
| Best For    | Production Workloads | Temporary Data |

---

# 🔵 User Data & Bootstrapping

**User Data** is a script that automatically runs when an EC2 instance launches for the first time.

AWS uses **Cloud-Init** (Linux AMIs) to execute the script during startup.

This automatic server configuration process is called **Bootstrapping**.

### Common Uses

- Install Nginx
- Install Apache
- Install Docker
- Install Node.js
- Install Java
- Clone Git Repository
- Configure Services
- Create Users

### Example

```bash
#!/bin/bash

sudo apt update -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

When the instance launches, Nginx is automatically installed and started without manual login.

---

# 🔵 Auto Scaling

Auto Scaling automatically adjusts the number of EC2 instances based on application demand.

It ensures applications remain available while reducing infrastructure costs.

Scaling decisions are typically based on:

- CPU Utilization
- Memory Usage
- Network Traffic
- Request Count
- CloudWatch Metrics

---

# 🔵 Auto Scaling Group (ASG)

An **Auto Scaling Group (ASG)** manages a collection of EC2 instances.

Responsibilities include:

- Launch New Instances
- Replace Failed Instances
- Remove Unnecessary Instances
- Maintain Desired Capacity
- Improve High Availability

---

# 🔵 Launch Template

A **Launch Template** defines how EC2 instances should be created.

It contains:

- AMI
- Instance Type
- Key Pair
- Security Groups
- Storage
- User Data
- IAM Role

Auto Scaling Groups use Launch Templates to create new instances automatically.

---

# 🔵 Elastic IP (EIP)

An **Elastic IP** is a static public IPv4 address provided by AWS.

Unlike a normal public IP, it does not change when an instance is stopped and started.

### Benefits

- Static Public IP
- Easy Failover
- Can be remapped to another instance

---

# 🔵 Placement Groups

Placement Groups help control how EC2 instances are physically placed within AWS data centers.

Types:

- Cluster
- Spread
- Partition

They improve performance, fault tolerance, or both depending on the workload.

---

# 🔵 EC2 Hibernation

EC2 Hibernation allows an instance to pause while preserving the contents of RAM.

AWS saves the RAM contents to the root EBS volume.

When restarted, the instance resumes from exactly where it left off.

### Workflow

```text
Running Instance
        │
        ▼
   Hibernate
        │
        ▼
 RAM Saved to EBS
        │
        ▼
 Start Instance
        │
        ▼
 Resume Previous State
```

### Benefits

- Faster startup
- Preserves running applications
- Maintains memory state
- Reduces initialization time

---

# 🔵 EC2 Pricing Models

AWS offers multiple pricing options depending on your workload.

| Pricing Model      | Best For                                  |
| ------------------ | ----------------------------------------- |
| On-Demand          | Short-term workloads                      |
| Reserved Instances | Long-term predictable workloads           |
| Savings Plans      | Flexible long-term savings                |
| Spot Instances     | Fault-tolerant workloads at very low cost |
| Dedicated Hosts    | Compliance and licensing requirements     |

---

# 🔵 Common EC2 Use Cases

- Hosting Websites
- Running REST APIs
- Microservices
- Backend Applications
- CI/CD Servers
- Docker Containers
- Kubernetes Worker Nodes
- Database Servers
- Development & Testing
- Big Data Processing
- Machine Learning
- Batch Processing

---

# 🔵 Best Practices

- Use IAM Roles instead of storing AWS credentials.
- Enable Security Groups with least privilege.
- Regularly create EBS Snapshots.
- Monitor instances using Amazon CloudWatch.
- Use Auto Scaling for high availability.
- Choose the correct instance type.
- Keep AMIs updated.
- Enable detailed monitoring when required.
- Use Elastic IP only when necessary.
- Tag resources for easier management and billing.

---

# 🔵 EC2 Summary

Amazon EC2 is the foundational compute service of AWS that provides scalable virtual servers in the cloud. By combining AMIs, Instance Types, EBS, Security Groups, Auto Scaling, Load Balancers, IAM Roles, and monitoring services like CloudWatch, EC2 enables organizations to deploy secure, highly available, and cost-effective applications ranging from simple websites to large enterprise systems.
