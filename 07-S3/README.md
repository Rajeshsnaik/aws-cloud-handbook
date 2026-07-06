# Amazon S3 (Simple Storage Service)

![AWS](./aws-s3.png)

Amazon S3 (Simple Storage Service) is a highly scalable, durable, and secure **object storage service** provided by AWS. It is designed to store and retrieve any amount of data from anywhere over the internet. Data in Amazon S3 is stored as **objects** inside **buckets**, making it ideal for storing application assets, backups, logs, media files, static websites, and much more.

---

# 🔵 What is an S3 Bucket?

An **S3 Bucket** is a logical container used to store objects in Amazon S3. Every object must be stored inside a bucket, and each bucket has a globally unique name across AWS.

**Common Uses**

- Store application files
- Host static websites
- Store backups
- Save logs and reports
- Store images, videos, and documents

---

# 🔵 What is an S3 Object?

An **S3 Object** is the actual file stored inside an S3 bucket. Every object consists of:

- Object Data (Actual File)
- Object Key (Unique Name)
- Metadata
- Version ID (If Versioning is Enabled)

Examples of S3 objects include:

- Images
- Videos
- PDFs
- ZIP Files
- Application Logs
- Backup Files

---

# 🔵 What is an Object Key?

An **Object Key** is the unique name or path that identifies an object inside an S3 bucket.

Example:

```text
Bucket Name:
my-company-data

Object Key:
images/profile.jpg
```

Here:

- Bucket = `my-company-data`
- Object Key = `images/profile.jpg`

Together they uniquely identify an object.

---

# 🔵 Durability

**Durability** refers to the probability that your data remains safe and is not lost over time.

Amazon S3 provides **99.999999999% (11 Nines) Durability**, making it one of the most reliable storage services available.

This high durability is achieved by automatically storing multiple copies of data across multiple Availability Zones.

---

# 🔵 Availability

**Availability** refers to the ability to access your data whenever it is needed.

Unlike durability, which focuses on preventing data loss, availability measures how often the service is accessible to users.

---

# 🔵 S3 Versioning

**Versioning** allows Amazon S3 to maintain multiple versions of the same object within a bucket.

When versioning is enabled:

- Deleted files can be recovered.
- Previous versions remain available.
- Accidental overwrites can be reversed.

Versioning is commonly used for backups and data protection.

---

# 🔵 S3 Lifecycle Policy

A **Lifecycle Policy** automatically manages objects throughout their lifecycle based on rules that you define.

Lifecycle policies help reduce storage costs by:

- Moving objects to cheaper storage classes.
- Deleting old or unused objects automatically.
- Archiving infrequently accessed data.

Example:

```text
S3 Standard
      ↓
Standard-IA
      ↓
Glacier
      ↓
Deep Archive
```

---

# 🔵 S3 Encryption

Amazon S3 supports multiple encryption options to protect stored data.

## SSE-S3

- Server-Side Encryption managed entirely by AWS.
- AWS automatically creates and manages encryption keys.
- Simplest encryption option.

---

## SSE-KMS

- Uses AWS Key Management Service (KMS).
- Provides greater security and audit capabilities.
- Supports key rotation and fine-grained access control.

---

## Client-Side Encryption

Data is encrypted before it is uploaded to Amazon S3.

In this approach:

- Encryption is performed by the client application.
- AWS never sees the unencrypted data.
- Customers manage their own encryption keys.

---

# 🔵 S3 Bucket Policy

An **S3 Bucket Policy** is a resource-based IAM policy attached directly to an S3 bucket.

It controls who can access the bucket and what actions they can perform.

Common permissions include:

- Read Objects
- Upload Objects
- Delete Objects
- List Bucket Contents

Bucket Policies are commonly used for cross-account access and public website hosting.

---

# 🔵 Pre-Signed URL

A **Pre-Signed URL** provides temporary access to an S3 object without making the bucket public.

Common use cases include:

- Temporary file downloads
- Secure file uploads
- Sharing private documents
- Limited-time access

The URL automatically expires after the configured duration.

---

# 🔵 S3 Event Notifications

Amazon S3 can automatically trigger actions whenever objects are created, deleted, or restored.

Supported destinations include:

- AWS Lambda
- Amazon SNS
- Amazon SQS
- Amazon EventBridge

Common use cases:

- Image processing
- File validation
- Notifications
- Workflow automation

---

# 🔵 Multipart Upload

Multipart Upload allows large files to be divided into multiple smaller parts and uploaded independently.

Instead of uploading a large file (for example, larger than 5 GB) as a single request, Amazon S3 uploads multiple parts simultaneously and combines them after completion.

Benefits include:

- Faster uploads
- Better reliability
- Easy retry of failed parts
- Improved performance

---

# 🔵 S3 Replication

Amazon S3 Replication automatically copies objects from one bucket to another.

## Cross-Region Replication (CRR)

Copies objects to a bucket in a **different AWS Region**.

Common use cases:

- Disaster Recovery
- Global Availability
- Regulatory Compliance

---

## Same-Region Replication (SRR)

Copies objects to another bucket within the **same AWS Region**.

Common use cases:

- Log Aggregation
- Testing Environments
- Data Synchronization

---

# 🔵 Difference Between Amazon EBS and Amazon S3

| Amazon EBS                               | Amazon S3                                     |
| ---------------------------------------- | --------------------------------------------- |
| Block Storage                            | Object Storage                                |
| Attached to EC2 Instances                | Independent Storage Service                   |
| One Volume per Instance (Typical)        | Accessible through APIs                       |
| Low Latency Storage                      | Massive Scalability                           |
| Used for Operating Systems and Databases | Used for Files, Images, Videos, Backups, Logs |

---

# 🔵 Amazon S3 Overview

Amazon S3 is one of the most widely used AWS services because of its scalability, durability, and integration with other AWS services.

Key features include:

- 99.999999999% (11 Nines) Durability
- Multiple Storage Classes
- Versioning
- Lifecycle Management
- Server-Side Encryption
- Replication
- Event Notifications
- Integration with IAM, Lambda, CloudFront, and many other AWS services

Common use cases include:

- File Storage
- Static Website Hosting
- Database Backups
- Application Assets
- Terraform Remote State
- Log Storage
- CI/CD Artifacts

---

# 🔵 S3 Storage Classes

Amazon S3 provides multiple storage classes to optimize **cost**, **availability**, and **retrieval speed** based on how frequently data is accessed.

## 1. S3 Standard

The default storage class for frequently accessed data.

**Best For**

- Application Files
- Websites
- Frequently Accessed Data

**Features**

- Highest Availability
- Lowest Latency
- Most Expensive Standard Storage

---

## 2. S3 Standard-Infrequent Access (Standard-IA)

Designed for data that is accessed occasionally but must be available immediately when needed.

**Features**

- Lower Storage Cost
- Retrieval Charges Apply
- Immediate Access

---

## 3. S3 One Zone-Infrequent Access (One Zone-IA)

Stores data in a single Availability Zone instead of multiple Availability Zones.

**Features**

- Lower Cost
- Immediate Retrieval
- Less Resilient

Suitable for data that can be recreated if lost.

---

## 4. S3 Glacier Instant Retrieval

Archive storage with immediate retrieval capability.

**Best For**

- Medical Records
- Archived Images
- Long-Term Documents

---

## 5. S3 Glacier Flexible Retrieval

Low-cost archival storage where data retrieval can take several minutes to hours.

Suitable when immediate access is not required.

---

## 6. S3 Glacier Deep Archive

The lowest-cost storage class designed for long-term data retention and compliance.

Typical use cases include:

- Legal Records
- Financial Archives
- Compliance Data

---

## 7. S3 Intelligent-Tiering

Automatically moves objects between storage tiers based on usage patterns.

Best suited for workloads where access frequency is unknown or unpredictable.

---

# 🔵 S3 Access Control List (ACL)

## Definition

An **Access Control List (ACL)** is the legacy permission mechanism used to control access to S3 buckets and objects.

## Interview Point

- Mostly replaced by IAM Policies and Bucket Policies.
- Generally disabled in modern AWS environments.
- AWS recommends using Bucket Policies and IAM Policies instead.

---

# 🔵 S3 Static Website Hosting

## Definition

Amazon S3 can host static websites directly from an S3 bucket.

### Can Host

```text
HTML
CSS
JavaScript
Images
```

### Cannot Host

```text
Node.js
Java
PHP
Python Backend
```

## Interview Point

Amazon S3 can host only **static websites**. Dynamic applications require services such as EC2, Elastic Beanstalk, ECS, or Lambda.

---

# 🔵 S3 Transfer Acceleration

## Definition

S3 Transfer Acceleration uses AWS Edge Locations to speed up uploads and downloads for users located around the world.

## Example

```text
India User
      │
      ▼
AWS Edge Location
      │
      ▼
S3 Bucket (US Region)
```

## Interview Point

Useful when users upload large files from different countries.

---

# 🔵 S3 Consistency

## Definition

Consistency determines how quickly changes become visible after write operations.

## Current Behavior

Amazon S3 provides:

```text
Strong Read-After-Write Consistency
```

This means that immediately after uploading or updating an object, it can be read without waiting.

---

# 🔵 Difference Between Bucket Policy and IAM Policy

This is one of the most common Amazon S3 interview questions.

| IAM Policy                       | Bucket Policy                      |
| -------------------------------- | ---------------------------------- |
| Attached to User, Role, or Group | Attached to an S3 Bucket           |
| Identity-Based Policy            | Resource-Based Policy              |
| Controls what a user can do      | Controls who can access the bucket |
| Used across AWS services         | Specific to Amazon S3 resources    |

---

# 🔵 S3 Object Metadata

**Object Metadata** is additional information stored with an object in Amazon S3. It provides details about the object but is **not part of the actual file content**.

Metadata can be automatically generated by S3 or added by users during upload.

## Common Examples

```text
Content-Type
File Size
Last Modified
ETag
Custom Metadata
```

### Common Use Cases

- Identify file type
- Store custom application information
- Track upload details
- Improve file management

> **Note:** Metadata describes the object but does not contain the object's actual data.

---

# 🔵 S3 Object Lock

**S3 Object Lock** prevents objects from being modified or deleted for a specified period. It is mainly used to protect important data from accidental deletion or malicious changes.

Object Lock provides **WORM (Write Once, Read Many)** capability.

## Object Lock Modes

### Governance Mode

- Prevents most users from modifying or deleting objects.
- Authorized users with special permissions can override the lock.

### Compliance Mode

- Prevents everyone from modifying or deleting objects.
- Even AWS account administrators cannot remove the lock before the retention period ends.

## Common Use Cases

- Financial records
- Legal documents
- Regulatory compliance
- Audit logs
- Backup protection

---

# 🔵 S3 Request Types

Amazon S3 supports different request types to interact with objects stored in a bucket.

## GET

Downloads an object from an S3 bucket.

## PUT

Uploads a new object or replaces an existing object.

## DELETE

Deletes an object from a bucket.

## LIST

Lists the objects stored inside a bucket.

### Interview Point

Amazon S3 pricing includes both **storage charges** and **request charges**. Operations such as GET, PUT, DELETE, and LIST contribute to request costs.

---

# 🔵 S3 VPC Endpoint

An **S3 VPC Endpoint** allows resources inside a VPC to access Amazon S3 **without using the public internet**.

Instead of sending traffic through an Internet Gateway or NAT Gateway, traffic stays within the AWS network, making communication more secure.

## Flow

```text
EC2 Instance
      │
      ▼
S3 VPC Endpoint
      │
      ▼
Amazon S3
```

Instead of:

```text
EC2 Instance
      │
      ▼
Internet Gateway
      │
      ▼
Amazon S3
```

### Benefits

- Private communication with S3
- Improved security
- Reduced internet dependency
- Better network performance

---

# 🔵 S3 Notification Destinations

Amazon S3 can automatically trigger other AWS services whenever events occur in a bucket, such as object uploads, deletions, or restores.

Supported notification destinations include:

```text
AWS Lambda
Amazon SNS
Amazon SQS
Amazon EventBridge
```

### Common Use Cases

- Image processing after upload
- Sending notifications
- Background file processing
- Triggering automated workflows
- Event-driven architectures

---

# 🔵 DevOps-Specific S3 Use Cases

Amazon S3 is widely used in DevOps workflows because of its durability, scalability, and integration with AWS services.

## Terraform Remote State

Store Terraform state files securely so multiple team members can collaborate safely.

## Application Logs

Store application, web server, and system logs for long-term retention and analysis.

## CI/CD Artifacts

Store build artifacts, deployment packages, Docker images, and release files used in CI/CD pipelines.

## Database Backups

Store database backups and snapshots securely for disaster recovery and long-term retention.
