# 🔵 Amazon RDS (Relational Database Service)

![AWS](./aws-rds.png)

# 🔵 Amazon RDS

![AWS RDS](./aws-rds.png)

**Amazon RDS (Relational Database Service)** is a **fully managed relational database service** provided by AWS. It simplifies database administration by automating tasks such as provisioning, backups, patching, monitoring, and scaling.

Instead of managing database servers manually, you can focus on building your applications.

---

# 🔵 Key Features

Amazon RDS provides several built-in features to simplify database management.

### Features

- Fully Managed Service
- Automated Backups
- Multi-AZ High Availability
- Read Replicas
- Automatic Software Patching
- Monitoring with CloudWatch
- Encryption using AWS KMS
- Storage Auto Scaling
- Easy Scaling
- Point-in-Time Recovery (PITR)

---

# 🔵 Supported Database Engines

Amazon RDS supports multiple popular relational database engines.

## 1. MySQL

- Open-source relational database
- Popular for web applications
- Easy to use and cost-effective

## 2. PostgreSQL

- Advanced open-source relational database
- Supports complex queries and JSON data
- Ideal for enterprise applications

## 3. MariaDB

- Community-developed fork of MySQL
- Compatible with MySQL
- Improved performance and additional features

## 4. Oracle Database

- Enterprise-grade commercial database
- Suitable for large business applications
- Advanced security and high performance

## 5. Microsoft SQL Server

- Microsoft's relational database
- Commonly used with .NET applications
- Supports business intelligence and reporting

## 6. Amazon Aurora

- AWS-developed relational database
- Compatible with MySQL and PostgreSQL
- Higher performance and availability than standard MySQL/PostgreSQL
- Automatically scales storage

---

# 🔵 RDS Architecture

Amazon RDS consists of a **DB Instance**, storage, backups, and networking managed by AWS.

```text
Application
      │
      ▼
Amazon RDS
      │
      ▼
Database Engine
      │
      ▼
Storage
```

AWS manages the underlying infrastructure, while your application connects to the DB Instance.

---

# 🔵 DB Instance

A **DB Instance** is the primary database server in Amazon RDS.

It contains:

- Database Engine
- CPU
- Memory (RAM)
- Storage
- Network Configuration

Applications connect directly to the DB Instance using its **endpoint**.

---

# 🔵 DB Instance Classes

A **DB Instance Class** defines the computing resources allocated to your database.

It determines:

- CPU
- Memory (RAM)
- Network Performance

### Examples

- **db.t3.micro** – Development and Free Tier
- **db.t3.small** – Small applications
- **db.m7g.large** – Production workloads
- **db.r7g.large** – Memory-intensive databases

Choose the instance class based on your application's performance requirements.

---

# 🔵 Storage Types

Amazon RDS offers different storage options based on performance and cost requirements.

## 1. General Purpose SSD (gp3/gp2)

- Balanced price and performance
- Suitable for most applications
- Recommended for general workloads

## 2. Provisioned IOPS SSD (io1/io2)

- High-performance SSD storage
- Designed for I/O-intensive workloads
- Best for enterprise and mission-critical databases

## 3. Magnetic (Legacy)

- Older generation storage
- Lowest cost
- Lower performance
- Recommended only for legacy applications; SSD storage is preferred for new deployments.

---

# 🔵 Multi-AZ Deployment

**Multi-AZ (Availability Zone)** deployment improves **high availability** by creating a **primary database** and a **standby replica** in another Availability Zone.

- Data is synchronously replicated.
- Standby database cannot be used for reading.
- Automatic failover during failures.
- Best for production applications.

```text
Application
      │
      ▼
Primary DB (AZ-1)
      │
Synchronous Replication
      ▼
Standby DB (AZ-2)
```

---

# 🔵 Read Replicas

A **Read Replica** is a read-only copy of your database used to improve **read performance**.

- Asynchronous replication
- Handles read-only queries
- Multiple read replicas can be created
- Best for read-heavy applications

---

# 🔵 Single-AZ vs Multi-AZ vs Read Replica

| Feature      | Single-AZ        | Multi-AZ          | Read Replica             |
| ------------ | ---------------- | ----------------- | ------------------------ |
| Purpose      | Basic deployment | High Availability | Improve Read Performance |
| Replica      | No               | Standby           | Read-only Copy           |
| Read Traffic | Primary DB       | Primary DB        | Replica DB               |
| Failover     | No               | Automatic         | No                       |
| Replication  | —                | Synchronous       | Asynchronous             |
| Best For     | Development      | Production        | Read-heavy applications  |

---

# 🔵 High Availability & Failover

**High Availability (HA)** ensures the database remains available even if the primary instance fails.

During a failure:

1. Primary DB becomes unavailable.
2. AWS automatically switches to the standby DB.
3. Applications reconnect using the same endpoint.

This process is called **Automatic Failover**.

### Benefits

- Minimal downtime
- Automatic recovery
- No manual intervention
- Increased reliability

---

# 🔵 Automated Backups

Amazon RDS automatically creates daily backups and transaction logs.

### Features

- Enabled during DB creation
- Backup retention: **1–35 days**
- Supports Point-in-Time Recovery
- No manual backup required

Best for regular data protection.

---

# 🔵 Manual Snapshots

A **Manual Snapshot** is a user-created backup of an RDS database.

### Features

- Created manually
- Never expires until deleted
- Can be restored anytime
- Useful before upgrades or major changes

---

# 🔵 Point-in-Time Recovery (PITR)

**Point-in-Time Recovery (PITR)** allows you to restore your database to **any specific second** within the backup retention period.

### Example

```text
10:30 AM
Database accidentally deleted

↓

Restore database to

10:29:45 AM
```

This minimizes data loss after accidental changes or failures.

---

# 🔵 Database Security

Amazon RDS provides multiple security features to protect your database and data.

Main security mechanisms include:

- IAM Authentication
- Security Groups
- Encryption at Rest
- Encryption in Transit (SSL/TLS)

---

# 1. IAM Authentication

IAM Authentication allows users or applications to connect to an RDS database using **AWS IAM credentials** instead of a database password.

### Benefits

- Improved security
- Temporary authentication tokens
- No need to store database passwords
- Centralized access management using IAM

---

# 2. Security Groups

A **Security Group** acts as a virtual firewall for your RDS instance.

It controls:

- Who can connect
- Allowed IP addresses
- Allowed ports (e.g., **3306** for MySQL, **5432** for PostgreSQL)

### Example

```text
Allow:

EC2 Instance
Developer Laptop
Application Server
```

Only approved sources can access the database.

---

# 3. Encryption at Rest

Encryption at Rest protects stored database data using **AWS Key Management Service (KMS)**.

It encrypts:

- Database storage
- Automated backups
- Read Replicas
- Snapshots

This ensures data remains secure even if the storage is accessed directly.

---

# 4. Encryption in Transit (SSL/TLS)

Encryption in Transit protects data while it travels between the application and the database.

It uses **SSL/TLS** to encrypt network communication.

### Benefits

- Prevents data interception
- Secure client-to-database communication
- Recommended for all production environments

---

# 🔵 Monitoring

Amazon RDS provides built-in monitoring tools to track database health, performance, and resource usage.

It integrates with:

- Amazon CloudWatch
- Enhanced Monitoring
- Performance Insights

---

# 1. Amazon CloudWatch

**Amazon CloudWatch** collects standard metrics for your RDS instance.

### Common Metrics

- CPU Utilization
- Memory Usage
- Storage Usage
- Read/Write IOPS
- Database Connections
- Network Traffic

You can also create alarms to notify you when a metric exceeds a defined threshold.

---

# 2. Enhanced Monitoring

**Enhanced Monitoring** provides real-time operating system (OS) level metrics for your RDS instance.

It displays:

- CPU usage
- Memory usage
- Disk activity
- Running processes
- File system statistics

Useful for troubleshooting performance issues.

---

# 3. Performance Insights

**Performance Insights** helps identify database performance bottlenecks.

It shows:

- Slow SQL queries
- Database load
- Active sessions
- Wait events

This helps optimize query performance and improve application responsiveness.

---

# 🔵 Maintenance Windows

A **Maintenance Window** is a scheduled time when AWS performs maintenance tasks on your RDS instance.

These tasks may include:

- Software updates
- Security patches
- Minor version upgrades

You can choose a preferred maintenance window to minimize application impact.

---

# 🔵 Auto Minor Version Upgrades

RDS can automatically install **minor database version updates** during the maintenance window.

### Benefits

- Latest bug fixes
- Security patches
- Improved stability
- Reduced manual maintenance

Example:

```text
MySQL 8.0.38 → MySQL 8.0.39
```

---

# 🔵 Scaling

Amazon RDS supports scaling to handle increasing workloads.

There are two main types:

- Vertical Scaling
- Storage Auto Scaling

---

# 1. Vertical Scaling

Vertical Scaling means increasing or decreasing the **DB Instance Class**.

Example:

```text
db.t3.micro
        ↓
db.t3.medium
        ↓
db.m7g.large
```

This provides:

- More CPU
- More Memory (RAM)
- Better Network Performance

---

# 2. Storage Auto Scaling

Storage Auto Scaling automatically increases database storage when existing storage is running low.

### Benefits

- Prevents storage shortages
- No manual intervention
- No application downtime
- Pay only for additional storage used

---

# 🔵 Connectivity

Applications can connect to Amazon RDS in multiple ways depending on the network configuration.

---

# 1. Public vs Private Access

### Public Access

- Accessible over the internet
- Requires a public IP and proper Security Group rules
- Mainly used for development or testing

### Private Access

- Accessible only within the VPC
- More secure
- Recommended for production environments

---

# 2. Connecting from EC2

The recommended approach is to connect your application running on an **EC2 instance** to an RDS instance within the same VPC.

### Benefits

- Private communication
- Better security
- Lower network latency
- No internet exposure

---

# 3. Connecting from Local Machine

You can also connect to RDS from your local computer using tools such as:

- MySQL Workbench
- pgAdmin
- DBeaver
- SQL Server Management Studio (SSMS)

Ensure your IP address is allowed in the RDS Security Group.

---

# 🔵 Parameter Groups

A **Parameter Group** is a collection of database configuration settings.

Examples include:

- Maximum Connections
- Query Cache
- Time Zone
- Character Set

Instead of changing settings individually, you modify the Parameter Group and apply it to the DB instance.

---

# 🔵 Option Groups

An **Option Group** enables additional database features that are not enabled by default.

Examples:

- Oracle TDE (Transparent Data Encryption)
- SQL Server Backup & Restore
- Oracle OEM

Different database engines support different options.

---

# 🔵 Subnet Groups

A **DB Subnet Group** is a collection of subnets within a VPC where Amazon RDS can create database instances.

### Benefits

- High Availability
- Multi-AZ support
- Better network isolation

A subnet group should include subnets in at least two Availability Zones.

---

# 🔵 RDS Proxy

**Amazon RDS Proxy** is a fully managed database proxy that sits between your application and the RDS database.

```text
Application
      │
      ▼
RDS Proxy
      │
      ▼
Amazon RDS
```

### Benefits

- Connection pooling
- Improved scalability
- Faster failover
- Better performance for applications with many database connections

---

# 🔵 Amazon Aurora Overview

**Amazon Aurora** is AWS's high-performance relational database, compatible with **MySQL** and **PostgreSQL**.

### Features

- Up to 5× faster than MySQL
- Up to 3× faster than PostgreSQL
- Automatic storage scaling
- High availability
- Fault tolerant
- Multi-AZ by default
- Read Replicas supported

Ideal for enterprise and high-performance applications.

---

# 🔵 Pricing Model

Amazon RDS pricing depends on several factors:

- DB Instance Class
- Database Engine
- Storage Used
- Backup Storage
- Read Replicas
- Data Transfer
- Multi-AZ Deployment

You pay only for the resources you use.

---

# 🔵 Free Tier

AWS Free Tier includes eligible Amazon RDS usage for new AWS accounts.

Typically includes:

- **750 hours/month** of a **db.t3.micro** (or eligible micro instance, depending on the current Free Tier offer)
- **20 GB** of General Purpose SSD storage
- **20 GB** of backup storage

Suitable for learning, development, and testing.

---

# 🔵 Best Practices

- Use **Multi-AZ** for production databases.
- Enable **Automated Backups** and **Point-in-Time Recovery (PITR)**.
- Use **Read Replicas** for read-heavy workloads.
- Keep databases **private** inside a VPC whenever possible.
- Enable **encryption** for data at rest and in transit.
- Monitor database health using **CloudWatch** and **Performance Insights**.
- Choose the appropriate **DB Instance Class** and storage type based on workload.
- Regularly update database engines with minor version upgrades.
- Follow the **Principle of Least Privilege** using IAM and Security Groups.
- Use **Amazon Aurora** for applications requiring high performance and scalability.
