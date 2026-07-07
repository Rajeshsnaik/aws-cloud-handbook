# AWS Database Services Overview

![AWS](./aws-databases.png)

AWS provides multiple database services designed for different use cases. Choosing the right database depends on the type of data, scalability requirements, performance expectations, and application architecture.

---

# 🔵 Amazon RDS (Relational Database Service)

## What is RDS?

Amazon RDS is a fully managed relational database service that allows you to run SQL databases without managing the underlying infrastructure.

### Supported Database Engines

- MySQL
- PostgreSQL
- MariaDB
- Oracle
- Microsoft SQL Server

Instead of installing and maintaining database servers manually, AWS manages:

- Server Provisioning
- Automated Backups
- Software Patching
- Monitoring
- High Availability

---

## Why RDS?

Without RDS:

```text
Install Database
Configure Database
Patch Database
Backup Database
Monitor Database
Recover Database
```

Everything must be managed manually.

With RDS:

```text
AWS Manages Infrastructure
          ↓
You Manage Data and Queries
```

This allows developers and database administrators to focus on application development rather than infrastructure management.

---

## RDS Architecture

```text
Application
      │
      ▼
RDS Endpoint
      │
      ▼
Database Instance
      │
      ▼
Storage
```

Applications connect to RDS using a database endpoint.

---

## Key Features

### Automated Backups

AWS automatically creates backups and allows point-in-time recovery.

### Multi-AZ Deployment

Creates a standby database instance in another Availability Zone.

Benefits:

- High Availability
- Automatic Failover
- Disaster Recovery

### Read Replicas

Creates read-only copies of the primary database.

```text
Application
      │
      ▼
Primary Database
      │
      ▼
Read Replicas
```

Used to reduce load on the primary database during heavy read traffic.

---

## Common Use Cases

- E-Commerce Applications
- ERP Systems
- Banking Applications
- CRM Platforms
- Inventory Management Systems

---

# 🔵 Amazon Aurora

## What is Aurora?

Amazon Aurora is AWS's cloud-native relational database service.

It is compatible with:

- MySQL
- PostgreSQL

Aurora is not simply a managed MySQL database. It is a completely redesigned database engine built by AWS for better performance, scalability, and availability.

---

## Why Aurora?

Traditional databases tightly couple compute and storage.

Aurora separates:

```text
Compute
Storage
```

Benefits:

- Better Scalability
- Faster Failover
- Higher Availability
- Improved Performance

---

## Aurora Architecture

```text
Aurora Writer
      │
      ▼
Shared Storage Layer
      ▲
      │
Aurora Readers
```

Storage is automatically replicated across multiple Availability Zones.

```text
2 Copies per AZ
× 3 Availability Zones
─────────────────────
6 Copies Total
```

---

## Key Features

### High Availability

Automatic failover within seconds.

### Read Replicas

Supports up to 15 Aurora Replicas.

### Auto Scaling Storage

Storage automatically grows as data increases.

---

## Common Use Cases

- SaaS Platforms
- High-Traffic Websites
- Enterprise Applications
- Financial Systems
- Mission-Critical Workloads

---

# 🔵 Amazon DynamoDB

## What is DynamoDB?

Amazon DynamoDB is AWS's fully managed NoSQL database service.

Data is stored as:

```text
Table
 └── Items
       └── Attributes
```

Unlike relational databases:

```text
No Joins
No Fixed Schema
No Foreign Keys
```

---

## Why DynamoDB?

Traditional databases often struggle with massive scale and unpredictable workloads.

DynamoDB provides:

- Single-Digit Millisecond Latency
- Automatic Scaling
- Serverless Architecture
- High Availability

---

## DynamoDB Architecture

```text
Application
      │
      ▼
DynamoDB Table
      │
      ▼
Partitions
```

AWS automatically distributes data across partitions.

---

## Key Concepts

### Partition Key

Determines where data is stored.

### Sort Key

Organizes data within a partition.

### Global Secondary Index (GSI)

Provides alternative query patterns.

### DynamoDB Streams

Captures table changes for downstream processing.

---

## Common Use Cases

- Shopping Carts
- User Sessions
- Gaming Applications
- IoT Platforms
- Real-Time Applications

---

# 🔵 Amazon DocumentDB

## What is DocumentDB?

Amazon DocumentDB is a fully managed document database service.

It is compatible with MongoDB APIs and stores data as JSON-like documents.

Example:

```json
{
  "name": "John",
  "city": "Bangalore"
}
```

---

## Why DocumentDB?

Relational databases require predefined schemas.

DocumentDB supports flexible schemas, allowing different documents to contain different fields.

This flexibility makes it ideal for rapidly evolving applications.

---

## DocumentDB Architecture

```text
Application
      │
      ▼
DocumentDB Cluster
      │
      ▼
Storage Layer
```

Storage automatically scales as data grows.

---

## Key Features

### JSON Documents

Ideal for modern web and mobile applications.

### Managed Service

AWS handles maintenance and infrastructure.

### High Availability

Data is replicated across multiple Availability Zones.

---

## Common Use Cases

- Product Catalogs
- Content Management Systems
- User Profiles
- Mobile Applications

---

# 🔵 Amazon Neptune

## What is Neptune?

Amazon Neptune is a fully managed graph database service.

It is specifically designed to store and query relationships between data.

Example:

```text
User
 │
Friend
 │
User
```

---

## Why Neptune?

Relational databases are not optimized for highly connected data.

Neptune is designed for:

- Relationship Queries
- Graph Traversals
- Connected Data Analysis

---

## Neptune Architecture

```text
Nodes
  │
Relationships
  │
Graph Queries
```

---

## Examples

### Social Network

```text
John
 │
Friend
 │
David
```

### Recommendation Engine

```text
User
 │
Purchased
 │
Product
```

---

## Common Use Cases

- Social Networks
- Fraud Detection
- Recommendation Systems
- Knowledge Graphs

---

# 🔵 Amazon Timestream

## What is Timestream?

Amazon Timestream is a fully managed serverless time-series database.

It is specifically designed for storing and analyzing timestamp-based data.

---

## Example Data

```text
Timestamp      CPU Usage
──────────     ─────────
10:00 AM          40%
10:01 AM          45%
10:02 AM          50%
```

---

## Why Timestream?

Traditional databases are not optimized for storing billions of timestamped records.

Timestream is built specifically for:

- Monitoring
- Analytics
- Time-Series Queries

---

## Common Use Cases

- IoT Sensor Data
- Server Monitoring
- Application Metrics
- Financial Market Data

---

# 🔵 Amazon Keyspaces

## What is Keyspaces?

Amazon Keyspaces is AWS's managed Apache Cassandra service.

Apache Cassandra is a distributed NoSQL database designed for large-scale workloads.

AWS manages the infrastructure behind Cassandra.

---

## Why Keyspaces?

Managing Cassandra clusters can be complex.

Keyspaces eliminates the need to manage:

```text
Servers
Scaling
Patching
Replication
Maintenance
```

---

## Keyspaces Architecture

```text
Application
      │
      ▼
Amazon Keyspaces
      │
      ▼
Distributed Storage
```

---

## Key Features

### Cassandra Compatible

Uses Cassandra APIs and tools.

### Serverless

No infrastructure management.

### Multi-AZ Replication

Built-in fault tolerance.

### High Scalability

Designed for massive workloads.

---

## Common Use Cases

- Telecom Systems
- IoT Platforms
- Event Tracking Systems
- Large-Scale Applications

---

# 🔵 Amazon MemoryDB

## What is MemoryDB?

Amazon MemoryDB is a fully managed in-memory database compatible with Redis.

Most data is stored directly in memory (RAM) for extremely fast performance.

---

## Why MemoryDB?

Applications requiring ultra-low latency benefit from in-memory storage.

MemoryDB provides:

```text
Microsecond Response Time
```

---

## MemoryDB Architecture

```text
Application
      │
      ▼
MemoryDB
      │
      ▼
RAM
```

Most reads are served directly from memory.

---

## Key Features

### Redis Compatible

Supports Redis commands and data structures.

### Durable Storage

Unlike traditional caches, data is durable.

### Ultra-Fast Performance

Microsecond latency.

---

## Common Use Cases

- Gaming Leaderboards
- Trading Platforms
- Session Stores
- Real-Time Analytics

---

# 🔵 Amazon ElastiCache

## What is ElastiCache?

Amazon ElastiCache is a fully managed caching service.

Supported Engines:

- Redis
- Memcached

---

## Why ElastiCache?

Databases become slower under heavy traffic.

A cache stores frequently accessed data in memory, reducing database load and improving performance.

---

## ElastiCache Architecture

```text
Application
      │
      ▼
ElastiCache
      │
      ▼
Database
```

Without Cache:

```text
Every Request
      ↓
Database
```

With Cache:

```text
Frequent Requests
      ↓
Cache
      ↓
Database (Less Load)
```

---

## Key Features

### Redis

Supports:

- Caching
- Pub/Sub Messaging
- Session Storage

### Memcached

Provides simple distributed caching.

### Fast Performance

Delivers millisecond latency.

---

## Common Use Cases

- API Caching
- Session Management
- Website Acceleration
- Database Offloading
- Real-Time Applications

---

# 🔵 Quick Comparison

| Service     | Type                             | Best For                       |
| ----------- | -------------------------------- | ------------------------------ |
| RDS         | Relational Database              | Traditional SQL Applications   |
| Aurora      | Cloud-Native Relational Database | High Performance SQL Workloads |
| DynamoDB    | NoSQL Database                   | Massive Scale Applications     |
| DocumentDB  | Document Database                | JSON-Based Applications        |
| Neptune     | Graph Database                   | Relationship Data              |
| Timestream  | Time-Series Database             | Monitoring and Metrics         |
| Keyspaces   | Cassandra Database               | Distributed NoSQL Workloads    |
| MemoryDB    | In-Memory Database               | Ultra-Low Latency Applications |
| ElastiCache | Caching Service                  | Performance Optimization       |
