# Amazon Elastic Block Store (EBS)

![EBS](./ebs.png)

## 🔵 What is Amazon EBS?

**Amazon Elastic Block Store (EBS)** is a block storage service provided by AWS for **Amazon EC2 instances**.

It works like a **virtual hard disk (SSD/HDD)** attached to an EC2 server. The operating system, applications, and data are stored on EBS volumes.

Think of it as the internal hard drive of a physical computer, but in the cloud.

---

## 🔵 Why Do We Use EBS?

Amazon EBS is used to:

- Store the operating system of an EC2 instance
- Store application files
- Store databases
- Store user data
- Store logs and backups
- Persist data even after an EC2 reboot

Without EBS, your EC2 instance would not have persistent storage for your applications and files.

---

## 🔵 How Data is Stored in EBS

```text
Application

↓

Operating System

↓

EBS Volume

↓

AWS Storage Infrastructure
```

Whenever an application writes data, it is stored inside the attached EBS volume.

Unlike RAM, the data remains available even after restarting the EC2 instance.

---

## 🔵 Features

- Persistent block storage
- Works like a virtual hard disk
- Supports SSD and HDD volume types
- Resizable storage volumes
- Snapshot support for backups
- Built-in encryption
- High durability
- Low latency
- High performance
- Supports automatic backups
- Can be attached and detached from EC2 instances
- Integrated with AWS KMS for encryption
- Supports lifecycle management

---

## 🔵 EBS During EC2 Creation

While launching an EC2 instance, navigate to:

```text
Configure Storage
```

Here you can configure:

- Volume Size
- Volume Type
- IOPS
- Throughput
- Encryption
- KMS Key
- Delete on Termination

Example:

```text
Volume Type:
gp3

Size:
20 GB

Encrypted:
Yes

Delete on Termination:
No
```

---

## 🔵 Advanced Storage Options

Some commonly used advanced options include:

- Delete on Termination
- Encryption
- AWS KMS Key
- Throughput
- Provisioned IOPS
- Tags

---

## 🔵 Key Points

- Region specific
- Availability Zone (AZ) specific
- Built-in redundancy within the same AZ
- Supports snapshots for backup
- Supports encryption
- Supports resizing without recreating the volume
- Multiple volume types available
- Can be attached and detached
- Suitable for databases and production workloads

---

## 🔵 EBS Volume Types

| Volume Type | Description              | Best For                      |
| ----------- | ------------------------ | ----------------------------- |
| gp3         | General Purpose SSD      | Most workloads                |
| gp2         | Previous Generation SSD  | General workloads             |
| io1         | Provisioned IOPS SSD     | High-performance databases    |
| io2         | High durability SSD      | Mission-critical applications |
| st1         | Throughput Optimized HDD | Big data, log processing      |
| sc1         | Cold HDD                 | Infrequent access             |
| Magnetic    | Previous generation HDD  | Legacy workloads              |

---

# Practical Demo 1 - Create an EC2 with an EBS Volume

## 🔵 Step 1: Launch an EC2 Instance

While launching the EC2 instance:

Go to:

```text
Configure Storage
```

Configure:

```text
Volume Type:
gp3

Size:
20 GB

Encryption:
Enabled

Delete on Termination:
No
```

Launch the EC2 instance.

---

## 🔵 Step 2: View the Default Volume

Go to:

```text
EC2

↓

Elastic Block Store

↓

Volumes
```

You will see the default EBS volume automatically created for the EC2 instance.

You can also check:

```text
EC2

↓

Instance

↓

Storage
```

The attached volume is listed there.

---

## 🔵 Important - Delete on Termination

Go to:

```text
EC2

↓

Instance

↓

Storage

↓

Delete on Termination
```

By default:

```text
Yes
```

This means deleting the EC2 instance also deletes the EBS volume.

For important production data, it is recommended to change it to:

```text
No
```

This keeps the EBS volume even after the EC2 instance is terminated.

---

# Practical Demo 2 - Create and Attach a New EBS Volume

## 🔵 Step 1: Check Existing Volumes

Connect to the EC2 instance.

Run:

```bash
lsblk
```

You will see the default root volume.

---

## 🔵 Step 2: Create a New Volume

Go to:

```text
EC2

↓

Elastic Block Store

↓

Volumes

↓

Create Volume
```

Configure:

```text
Volume Type:
gp3

Size:
10 GB

Availability Zone:
Same as EC2 Instance
```

Click:

```text
Create Volume
```

> **Important:** The EBS Volume and EC2 instance **must be in the same Availability Zone (AZ)**. Otherwise, the volume cannot be attached.

---

## 🔵 Step 3: Attach the Volume

Select the volume.

Choose:

```text
Actions

↓

Attach Volume
```

Configure:

```text
Instance:
Select EC2

Device Name:
/dev/xvdf
```

Click:

```text
Attach Volume
```

---

## 🔵 Step 4: Verify the Attachment

Check from the AWS Console:

```text
EC2

↓

Instance

↓

Storage
```

Or connect to EC2 and run:

```bash
lsblk
```

The newly attached volume will appear.

---

# Practical Demo 3 - Modify the EBS Volume Size

Go to:

```text
EBS

↓

Volumes

↓

Select Volume

↓

Actions

↓

Modify Volume
```

Increase:

```text
Size:
10 GB

↓

20 GB
```

Click:

```text
Modify
```

> **Note:** AWS allows you to **increase** the volume size. You **cannot decrease** the size of an existing EBS volume.

---

# Practical Demo 4 - Detach and Delete an EBS Volume

Select the EBS volume.

Choose:

```text
Actions

↓

Detach Volume
```

After detaching:

```text
Actions

↓

Delete Volume
```

This permanently removes the EBS volume and all data stored on it.

---

# Practical Demo 5 - Create an EBS Snapshot

## 🔵 Why Use Snapshots?

Snapshots are backups of EBS volumes.

They are commonly used to:

- Backup important data
- Restore deleted volumes
- Create new volumes
- Copy data to another Availability Zone
- Copy data to another AWS Region

---

## 🔵 Create a Snapshot

Go to:

```text
EBS

↓

Volumes

↓

Select Volume

↓

Actions

↓

Create Snapshot
```

Provide:

```text
Description:
Production Backup
```

Click:

```text
Create Snapshot
```

Check it under:

```text
Snapshots
```

---

# Practical Demo 6 - Create a Volume from a Snapshot

Go to:

```text
Snapshots

↓

Select Snapshot

↓

Actions

↓

Create Volume from Snapshot
```

Choose:

```text
Volume Type

Size

Availability Zone
```

Click:

```text
Create Volume
```

---

## 🔵 Attach Snapshot-Based Volume

> **Important:** The EC2 instance and the new volume created from the snapshot **must be in the same Availability Zone**.

Attach the new volume using:

```text
Actions

↓

Attach Volume
```

---

## 🔵 Mount the New Volume

Check the filesystem:

```bash
sudo file -s /dev/device_name
```

Create a mount directory:

```bash
sudo mkdir /mnt/mybackup
```

Mount the volume:

```bash
sudo mount -o nouuid /dev/device_name /mnt/mybackup
```

Verify:

```bash
df -h
```

The snapshot data is now accessible.

---

## 🔵 Unmount the Volume

Run:

```bash
sudo umount -l /mnt/mybackup
```

---

# Practical Demo 7 - Copy Snapshot to Another Region

Go to:

```text
Snapshots

↓

Select Snapshot

↓

Actions

↓

Copy Snapshot
```

Choose:

```text
Destination Region
```

Click:

```text
Copy Snapshot
```

After the copy completes:

- Create a new EBS volume from the copied snapshot.
- Launch or use an EC2 instance in the destination Region.
- Attach the new volume.
- Mount it using the same Linux commands shown earlier.

This is useful for disaster recovery and cross-region backups.

---

## 🔵 EBS Encryption

EBS supports encryption using **AWS Key Management Service (AWS KMS)**.

When encryption is enabled:

```text
Application

↓

Operating System

↓

Encrypted EBS Volume

↓

AWS KMS Encryption Key

↓

AWS Storage
```

Encryption protects:

- Data at rest
- Data in transit between EC2 and EBS
- Snapshots
- Volumes created from encrypted snapshots

You can:

- Use the default AWS-managed KMS key
- Use your own Customer Managed Key (CMK)

---

# Practical Demo 8 - EBS Lifecycle Manager

## 🔵 What is EBS Lifecycle Manager?

Amazon **EBS Lifecycle Manager** automates the creation, retention, and deletion of EBS snapshots and AMIs based on schedules.

It helps you maintain regular backups without manual intervention.

> Creating Lifecycle Manager policies does not incur an additional charge, but the **snapshots and cross-region snapshot copies** created by the policy are billed according to AWS pricing.

---

## 🔵 Create a Lifecycle Policy

Go to:

```text
EC2

↓

Lifecycle Manager

↓

Create Lifecycle Policy
```

Configure:

```text
Policy Type:
EBS Snapshot Policy

Target Resource:
Volumes

Description:
Daily Backup Policy

Tags:
Add tags to identify volumes
```

Click:

```text
Next
```

Configure the schedule:

- Backup frequency
- Backup time
- Retention period
- Archive options (optional)

Optionally enable:

```text
Cross-Region Snapshot Copy
```

> Cross-region snapshot copies incur additional charges.

Review the policy and click:

```text
Create Policy
```

AWS now automatically creates snapshots based on your schedule.

---

# Practical Demo 9 - Snapshot Recycle Bin

## 🔵 What is Snapshot Recycle Bin?

The **Recycle Bin** protects EBS Snapshots and Amazon Machine Images (AMIs) from accidental deletion.

Instead of being permanently deleted immediately, they remain recoverable for a specified retention period.

> Recycle Bin retention incurs additional AWS charges while deleted snapshots or AMIs are retained.

---

## 🔵 Create a Recycle Bin Rule

Go to:

```text
EC2

↓

Recycle Bin

↓

Create Retention Rule
```

Configure:

```text
Resource Type:
EBS Snapshot

Retention Period:
Example:
30 Days
```

Create the rule.

If a protected snapshot is accidentally deleted, it remains in the Recycle Bin until the retention period expires, allowing it to be restored.

---

## 🔵 Best Practices

- Use **gp3** volumes for most workloads.
- Set **Delete on Termination = No** for important production data.
- Encrypt production EBS volumes using AWS KMS.
- Take regular snapshots of critical volumes.
- Use Lifecycle Manager to automate backups.
- Use tags to organize EBS volumes and snapshots.
- Store cross-region copies for disaster recovery.
- Delete unused volumes and snapshots to reduce costs.

---

## 🔵 Interview Questions

### Q1. What is Amazon EBS?

**Answer:** Amazon EBS is a persistent block storage service for Amazon EC2. It works like a virtual hard disk where the operating system, applications, and data are stored.

### Q2. Can an EBS volume be attached to an EC2 instance in another Availability Zone?

**Answer:** No. An EBS volume can only be attached to an EC2 instance in the **same Availability Zone (AZ)**.

### Q3. Can we reduce the size of an EBS volume?

**Answer:** No. AWS allows you to increase the size of an EBS volume, but you cannot directly reduce its size.

### Q4. What is an EBS Snapshot?

**Answer:** An EBS Snapshot is a point-in-time backup of an EBS volume stored in Amazon S3. It can be used to restore data or create new EBS volumes.

### Q5. What is the purpose of EBS Lifecycle Manager?

**Answer:** EBS Lifecycle Manager automates the creation, retention, and deletion of EBS snapshots and AMIs based on schedules, reducing manual backup management.
