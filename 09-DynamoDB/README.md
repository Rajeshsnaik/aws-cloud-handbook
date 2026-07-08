# 🔵 Amazon DynamoDB

![AWS DynamoDB](./aws-dynamodb.png)

**Amazon DynamoDB** is a **fully managed, serverless NoSQL database service** provided by AWS. It is designed to deliver **single-digit millisecond latency** at any scale while automatically handling infrastructure management, scaling, backups, and replication.

Unlike traditional relational databases, DynamoDB stores data as **key-value** and **document** data, making it ideal for modern web, mobile, gaming, IoT, and real-time applications.

---

# 🔵 Key Features

Amazon DynamoDB offers several powerful features that make it one of the most popular NoSQL databases.

- Fully Managed - AWS manages servers, operating systems, maintenance, and updates.
- Serverless - No infrastructure provisioning or management is required.
- High Performance - Provides **single-digit millisecond response times**, even with millions of requests.
- Automatic Scaling - Automatically adjusts read and write capacity based on traffic.
- High Availability - Data is automatically replicated across multiple Availability Zones.
- Flexible Data Model - Supports key-value and document-based data.
- Backup & Recovery - Supports:
  - On-demand backups
  - Point-in-Time Recovery (PITR)
- Global Tables - Replicates data across multiple AWS Regions automatically.

---

# 🔵 Use Cases

DynamoDB is best suited for applications requiring **high performance, scalability, and flexible schemas.**

---

# 🔵 NoSQL Concepts

**NoSQL** stands for **Not Only SQL**.

Unlike relational databases, NoSQL databases do not store data in rows and columns with fixed schemas.

Instead, DynamoDB stores data as:

- Key-Value
- Document (JSON-like)

## Characteristics

- No fixed schema
- Horizontally scalable
- Fast read/write operations
- Handles massive amounts of data
- Supports flexible attributes

## SQL vs NoSQL

| SQL Database             | DynamoDB (NoSQL)            |
| ------------------------ | --------------------------- |
| Tables with fixed schema | Flexible schema             |
| Joins supported          | No joins                    |
| Vertical scaling         | Horizontal scaling          |
| Complex relationships    | Simple key-based access     |
| Slower at massive scale  | Optimized for massive scale |

---

# 🔵 Tables, Items & Attributes

DynamoDB stores data in a hierarchy.

```text
Table
   │
   ├── Item
   │      ├── Attribute
   │      ├── Attribute
   │
   ├── Item
   │      ├── Attribute
   │      ├── Attribute
```

## Table

A **Table** is a collection of related data.

Example: Users, Products, Orders..etc

---

## Item

An **Item** is a single record inside a table.

Example:

```json
{
  "UserId": 101,
  "Name": "Rajesh",
  "Age": 23
}
```

Each item is uniquely identified using its **Primary Key**.

---

## Attribute

Attributes are the individual fields inside an item.

Example: UserId, Name, Age..etc

---

# 🔵 Primary Keys

A **Primary Key** uniquely identifies every item in a DynamoDB table.

DynamoDB supports **two types** of primary keys.

- Partition Key
- Composite Key (Partition Key + Sort Key)

---

# 🔵 Partition Key

A **Partition Key** consists of a **single attribute**.

AWS uses the partition key value to determine **which physical partition** stores the data.

Example

```text
Users Table

UserId (Partition Key)

1001
1002
```

Each UserId is unique.

### Advantages

- Fast lookups
- Simple design
- Even data distribution (if key values are well distributed)

---

# 🔵 Composite Primary Key (Partition Key + Sort Key)

A composite key consists of:

- Partition Key
- Sort Key

The **Partition Key** groups related items together, while the **Sort Key** uniquely identifies and orders items within that group.

Example

```text
Orders

CustomerId     OrderId
-----------    --------
C101           O001
C101           O002
C101           O003
```

Here -

Partition Key → CustomerId

Sort Key → OrderId

This allows one customer to have multiple orders while keeping them organized.

---

# 🔵 Secondary Indexes

A **Secondary Index** allows you to query data using attributes other than the table's primary key.

Without an index, you can only efficiently query using the primary key.

DynamoDB supports two types of secondary indexes:

- Global Secondary Index (GSI)
- Local Secondary Index (LSI)

---

# 1. Global Secondary Index (GSI)

A **Global Secondary Index (GSI)** lets you create an alternative partition key and optional sort key that are different from the table's primary key.

This allows querying data in new ways without changing the original table.

### Example

**Users Table**

| UserId (PK) | Email             | City      |
| ----------- | ----------------- | --------- |
| 101         | raj@example.com   | Bengaluru |
| 102         | anita@example.com | Mumbai    |
| 103         | amit@example.com  | Bengaluru |

Primary key is `UserId`, but if you frequently search by `Email`, create a GSI using `Email` as the partition key. You can then efficiently retrieve a user by email instead of scanning the entire table.

### Benefits

- Query using different attributes
- Supports different partition and sort keys
- Ideal for multiple access patterns

---

# 2. Local Secondary Index (LSI)

A **Local Secondary Index (LSI)** uses the **same partition key** as the base table but a **different sort key**.

LSIs must be created when the table is created.

### Example

**Orders Table**

Primary Key:

- Partition Key: `CustomerId`
- Sort Key: `OrderId`

You create an LSI with:

- Partition Key: `CustomerId`
- Sort Key: `OrderDate`

Now you can retrieve a customer's orders sorted by date instead of order ID.

### Benefits

- Same partition key as the base table
- Different sorting options
- Strongly consistent reads supported

---

# 🔵 Read & Write Capacity

DynamoDB automatically handles read and write throughput using two capacity modes.

- On-Demand Mode
- Provisioned Mode

---

# 1. On-Demand Mode

In **On-Demand Mode**, DynamoDB automatically scales capacity based on application traffic.

You don't need to estimate or configure read/write capacity in advance.

### Best For

- Unpredictable traffic
- Startups
- Development and testing
- New applications

### Advantages

- No capacity planning
- Automatic scaling
- Pay only for actual read and write requests

---

# 2. Provisioned Mode

In **Provisioned Mode**, you specify the number of **Read Capacity Units (RCUs)** and **Write Capacity Units (WCUs)** your application requires.

You can enable **Auto Scaling** to adjust these values automatically within defined limits.

### Best For

- Predictable workloads
- Stable production applications
- Cost optimization for consistent traffic

### Advantages

- Lower cost for steady workloads
- Greater control over throughput
- Auto Scaling support

---

# 🔵 Read Consistency

Read consistency determines **how up-to-date the data is** when you perform a read operation.

DynamoDB supports two types of read consistency:

- Eventually Consistent Reads
- Strongly Consistent Reads

---

# 1. Eventually Consistent Reads

An **Eventually Consistent Read** may return stale data immediately after a write because changes take a short time to propagate to all replicas.

Within a brief period, all replicas become consistent.

### Characteristics

- Default read option
- Lower cost (uses fewer read capacity units)
- Higher throughput
- Suitable for applications where slight delays are acceptable

### Example

1. Update a user's profile.
2. Immediately read the profile.
3. The first read may still show the old data.
4. A subsequent read shortly after returns the updated data.

---

# 2. Strongly Consistent Reads

A **Strongly Consistent Read** always returns the most recent successful write, ensuring you see the latest data.

### Characteristics

- Always returns the latest data
- Higher read cost than eventually consistent reads
- Slightly lower throughput
- Available only within the same AWS Region (not for Global Tables)

### Example

1. Update an account balance.
2. Immediately read the balance.
3. The latest updated balance is returned every time.

### When to Use

- Banking transactions
- Payment processing
- Inventory management
- Order confirmation systems
- Any application where stale data is unacceptable

---

# 🔵 CRUD Operations

CRUD represents the four basic operations performed on data in DynamoDB.

- **Create** → Add a new item
- **Read** → Retrieve an existing item
- **Update** → Modify an existing item
- **Delete** → Remove an item

## Example

Suppose we have a **Users** table.

### Create

```json
{
  "UserId": 101,
  "Name": "Rajesh",
  "City": "Bengaluru"
}
```

Adds a new user.

---

### Read

Retrieve user whose UserId = 101.

```text
GetItem(UserId = 101)
```

Returns:

```json
{
  "UserId": 101,
  "Name": "Rajesh",
  "City": "Bengaluru"
}
```

---

### Update

Update user's city.

```json
Update:
City = "Mysuru"
```

---

### Delete

```text
DeleteItem(UserId = 101)
```

Removes the record permanently.

---

# 🔵 Query vs Scan

Both are used to retrieve data, but they work very differently.

| Query                        | Scan                                   |
| ---------------------------- | -------------------------------------- |
| Searches using Partition Key | Reads entire table                     |
| Very fast                    | Slow                                   |
| Low cost                     | Higher cost                            |
| Best for production          | Mostly used for testing or admin tasks |

---

## Query

A **Query** retrieves data using the **Partition Key** (and optionally the Sort Key).

Example

```text
CustomerId = C101
```

Returns only that customer's orders.

### Advantages

- Fast
- Cost-effective
- Reads only matching items

---

## Scan

A **Scan** checks **every item** in the table.

Example

```text
Find all users whose City = Bengaluru
```

If City isn't indexed, DynamoDB scans the entire table.

### Disadvantages

- Slow
- Expensive
- Consumes more read capacity

> **Best Practice:** Always prefer **Query** over **Scan** whenever possible.

---

# 🔵 Conditional Writes

Conditional writes allow DynamoDB to perform an operation **only if a specified condition is true**.

This helps prevent accidental overwrites and ensures data consistency.

### Common Uses

- Prevent duplicate usernames
- Check inventory before purchase
- Optimistic locking
- Update only if a value exists

---

### Example

Update account balance only if balance is greater than ₹1000.

```text
Condition:
Balance > 1000
```

If the condition is true, the update succeeds.

Otherwise, DynamoDB returns an error.

---

# 🔵 Batch Operations

Batch operations allow multiple items to be processed in a **single API request**, reducing network calls and improving performance.

DynamoDB supports:

- BatchGetItem
- BatchWriteItem

---

# 🔵 BatchGetItem

Retrieves multiple items from one or more tables in a single request.

### Example

Instead of making 100 GetItem requests,

Use one:

```text
BatchGetItem
```

### Benefits

- Faster
- Fewer API calls
- Lower latency

---

# 🔵 BatchWriteItem

Allows inserting or deleting multiple items in one request.

Supports:

- PutItem
- DeleteItem

Example

Insert 25 products together.

Instead of:

```text
25 PutItem requests
```

Use:

```text
1 BatchWriteItem request
```

### Limitations

- Maximum **25 write operations**
- Maximum **16 MB** request size
- Does **not support UpdateItem**

---

# 🔵 Time To Live (TTL)

**Time To Live (TTL)** automatically deletes expired items from a DynamoDB table after a specified timestamp.

AWS removes expired items in the background without requiring manual deletion.

### Example

A session expires after 24 hours.

```json
{
  "SessionId": "ABC123",
  "ExpireAt": 1754500000
}
```

When the expiration time is reached, DynamoDB automatically deletes the item.

### Common Use Cases

- Login sessions
- OTP records
- Cache data
- Temporary tokens
- Shopping cart expiration

---

# 🔵 DynamoDB Streams

**DynamoDB Streams** capture every change made to a table.

Events include:

- Item Created
- Item Updated
- Item Deleted

These events can trigger other AWS services such as Lambda.

### Example

```text
User updates profile
        │
        ▼
DynamoDB Table
        │
        ▼
DynamoDB Stream
        │
        ▼
AWS Lambda
        │
        ▼
Send Email / Update Search Index / Audit Log
```

### Common Use Cases

- Event-driven applications
- Audit logging
- Data synchronization
- Real-time analytics
- Notifications

---

# 🔵 Transactions

Transactions ensure that **multiple operations either all succeed or all fail** (Atomicity).

This maintains data consistency across related items.

### Example

Bank Transfer

```text
Account A
- ₹1000

↓

Account B
+ ₹1000
```

If one operation fails, DynamoDB rolls back both operations.

### Transaction APIs

- TransactWriteItems
- TransactGetItems

### Benefits

- ACID transactions
- Data consistency
- Atomic operations

---

# 🔵 Global Tables (Multi-Region Replication)

**Global Tables** automatically replicate DynamoDB tables across multiple AWS Regions.

Users access the nearest region, while data remains synchronized globally.

### Example

```text
Mumbai
     │
     ▼
Global Table
 ▲         ▲
 │         │
London   Virginia
```

If data is updated in Mumbai, it is automatically replicated to London and Virginia.

### Benefits

- Low latency worldwide
- Disaster recovery
- High availability
- Multi-region applications

---

# 🔵 Backup & Restore

DynamoDB provides built-in backup and recovery features to protect data from accidental deletion or corruption.

# 1. On-Demand Backup

Creates a **manual snapshot** of the entire table.

The backup remains available until you delete it.

### Best For

- Before major deployments
- Before schema changes
- Compliance requirements
- Manual backups

---

# 2. Point-in-Time Recovery (PITR)

PITR continuously backs up your table and allows you to restore it to **any second within the last 35 days**.

### Example

```text
10:00 AM
Table deleted accidentally

↓

Restore table to

9:59:55 AM
```

### Benefits

- Continuous backup
- Recovery from accidental deletion
- Minimal data loss
- Restore to any point within the retention window

---

# 🔵 Security

DynamoDB provides multiple layers of security to protect your data.

Important security features include:

- IAM Permissions
- Encryption at Rest

---

# 🔵 IAM Permissions

AWS **IAM (Identity and Access Management)** controls who can access DynamoDB resources and what actions they can perform.

### Best Practices

- Follow the Principle of Least Privilege
- Use IAM Roles instead of long-term access keys
- Restrict permissions to required resources only

---

# 🔵 Encryption at Rest

DynamoDB automatically encrypts data stored on disk using **AWS Key Management Service (KMS)**.

This protects stored data from unauthorized access.

---

# 🔵 Monitoring

Monitoring helps track the health, performance, and usage of your DynamoDB tables.

## 1. CloudWatch Metrics

**Amazon CloudWatch** collects real-time performance metrics for DynamoDB.

### Benefits

- Performance monitoring
- Set alarms
- Capacity planning
- Troubleshooting

---

## 2. CloudTrail Logging

**AWS CloudTrail** records every API call made to DynamoDB.

### Benefits

- Security auditing
- Compliance
- User activity tracking
- Troubleshooting
- Incident investigation

---

# 🔵 DynamoDB Free Tier

AWS DynamoDB offers a generous **Free Tier** for new users.

### Free Tier Includes

- **25 GB** of storage
- Supports approximately **200 million requests per month** (depending on request type and item size)
- Ideal for learning, development, and small applications

---

# 🔵 Creating a DynamoDB Table

To create a table:

1. Open **AWS Console → DynamoDB**
2. Click **Create Table**
3. Enter the **Table Name**
4. Specify the **Partition Key** (required)
5. (Optional) Add a **Sort Key**
6. Choose **On-Demand** or **Provisioned** capacity mode
7. Click **Create Table**

After creation, wait until the table status changes to:

```text
Active
```

Only after the table becomes **Active** can you start performing operations.

---

# 🔵 Exploring Table Items

Open your table and navigate to **Explore Table Items**.

Initially, the table is empty.

For testing purposes:

- Click **Create Item**
- Add attributes manually
- Save the item

Since DynamoDB is a **NoSQL database**, attributes are **schema-less**.

Example:

```json
{
  "UserId": 101,
  "Name": "Rajesh",
  "City": "Bengaluru"
}
```

Another item in the same table can contain completely different attributes.

```json
{
  "UserId": 102,
  "Email": "abc@gmail.com",
  "Phone": "9999999999"
}
```

---

# 🔵 Capacity Mode Settings

After creating the table, open the **Capacity** section.

Here you can view or modify the table's capacity mode.

### On-Demand Mode

- Automatically handles traffic
- No manual configuration required
- Pay only for actual requests

### Provisioned Mode

You can configure:

- Read Capacity Units (RCUs)
- Write Capacity Units (WCUs)
- Minimum Capacity
- Maximum Capacity
- Auto Scaling settings

These values can be updated later as application traffic changes.

---

# 🔵 Monitoring

The **Monitoring** tab provides insights into your table's health and performance.

You can monitor:

- Read requests
- Write requests
- Request latency
- Throttled requests
- Consumed capacity
- Error rates

These metrics help identify bottlenecks and optimize performance.

---

# 🔵 Additional Table Features

From the table dashboard, you can also configure several advanced features, including:

- Secondary Indexes (GSI & LSI)
- Backup & Restore
- Point-in-Time Recovery (PITR)
- Time To Live (TTL)
- Deletion Protection
- Cost Explorer
- Streams
- Encryption
- Tags
- Export to Amazon S3
- Import from Amazon S3

---

# 🔵 DynamoDB Accelerator (DAX)

**DynamoDB Accelerator (DAX)** is a fully managed, in-memory cache for DynamoDB.

Instead of reading data directly from the database every time, frequently accessed data is served from memory.

### Benefits

- Microsecond read latency
- Reduces load on DynamoDB
- Improves performance for read-heavy applications
- No major application code changes required

---

# 🔵 Global Tables Demo

To create a Global Table:

1. Open your DynamoDB table.
2. Go to the **Global Tables** section.
3. Select your primary AWS Region.
4. Add one or more replica Regions.
5. AWS automatically replicates data between all selected Regions.

This provides:

- Low-latency access for global users
- Multi-region availability
- Built-in disaster recovery
- Automatic cross-region data replication

---

# 🔵 Local Application Demo

In this demo, the application stores data in **Amazon DynamoDB** instead of **MongoDB**.

To keep the setup simple and minimize AWS costs:

- The application runs locally.
- No Amazon EC2 instance is required.
- No Docker container is used.
- The application connects directly to DynamoDB over the internet using AWS credentials.

This approach is ideal for development and demonstrations.
