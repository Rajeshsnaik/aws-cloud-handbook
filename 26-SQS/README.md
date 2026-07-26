# 🔵 Amazon SQS (Simple Queue Service)

![aws](./aws-sqs.png)

Amazon **Simple Queue Service (SQS)** is a **fully managed message queuing service** provided by AWS that enables applications and microservices to communicate **asynchronously**.

Instead of applications communicating directly, they communicate through a **queue**, making systems more scalable, reliable, and fault tolerant.

In simple words:

> **Amazon SQS = A message queue that stores messages until another application is ready to process them.**

---

# 🔵 Why Amazon SQS?

Imagine an e-commerce application.

When a customer places an order, many tasks need to happen:

- Process payment
- Update inventory
- Send confirmation email
- Generate invoice
- Notify the shipping service

If the application performs all these tasks immediately, users may experience delays.

Instead, the application places the order into an SQS queue, and background services process the tasks independently.

### Benefits

- Decouples applications
- Reliable message delivery
- Handles traffic spikes
- Improves scalability
- Improves fault tolerance
- Prevents application failures
- Supports asynchronous processing

---

# 🔵 How Amazon SQS Works

Amazon SQS follows a simple Producer-Consumer model.

```text
Producer
    │
Send Message
    ▼
Amazon SQS Queue
    │
Receive Message
    ▼
Consumer
```

The producer sends messages to the queue.

The consumer retrieves the messages, processes them, and deletes them from the queue.

---

# 🔵 Producer

A **Producer** is any application or AWS service that sends messages to an SQS queue.

Examples:

- EC2
- Lambda
- Amazon SNS
- Web Applications
- Mobile Applications

Example:

```
Customer Places Order

↓

Web Application

↓

SQS Queue
```

---

# 🔵 Queue

A **Queue** temporarily stores messages until a consumer processes them.

Think of it like waiting in a ticket counter queue.

Messages wait until someone is available to process them.

---

# 🔵 Consumer

A **Consumer** is an application or service that retrieves messages from the queue.

Examples:

- EC2
- AWS Lambda
- Container Applications
- Background Worker

Example:

```
SQS Queue

↓

Order Service

↓

Process Order
```

---

# 🔵 Queue Types

Amazon SQS provides two queue types.

## Standard Queue

Standard Queue provides maximum throughput and high scalability.

### Features

- Unlimited throughput
- Best-effort message ordering
- At-least-once message delivery
- Duplicate messages are possible
- Suitable for most applications

---

## FIFO Queue

FIFO stands for **First In, First Out**.

Messages are processed in the exact order they are received.

### Features

- Guaranteed message ordering
- Exactly-once processing
- No duplicate messages
- Lower throughput than Standard Queue
- Suitable for financial and banking applications

---

# 🔵 Standard Queue vs FIFO Queue

| Feature            | Standard Queue       | FIFO Queue                |
| ------------------ | -------------------- | ------------------------- |
| Message Order      | Not Guaranteed       | Guaranteed                |
| Throughput         | Very High            | Lower                     |
| Duplicate Messages | Possible             | Not Allowed               |
| Delivery           | At Least Once        | Exactly Once              |
| Best For           | General Applications | Banking, Payments, Orders |

---

# 🔵 Message Lifecycle

Every message follows this lifecycle.

```text
Producer
     │
Send Message
     ▼
Amazon SQS Queue
     │
Receive Message
     ▼
Consumer
     │
Delete Message
```

If the consumer successfully processes the message, it deletes it from the queue.

---

# 🔵 Visibility Timeout

When a consumer receives a message, SQS hides it from other consumers for a specified period.

```text
Queue

↓

Consumer Receives Message

↓

Message Hidden

↓

Processed Successfully

↓

Delete Message
```

If processing succeeds, the message is deleted.

If processing fails, the message becomes visible again after the visibility timeout expires.

### Benefits

- Prevents multiple consumers from processing the same message simultaneously.
- Allows failed messages to be retried automatically.

---

# 🔵 Long Polling

Normally, SQS immediately responds when there are no messages.

Long Polling waits for messages before returning a response.

```text
Consumer

↓

Wait

↓

Message Arrives

↓

Return Message
```

### Benefits

- Reduces empty responses
- Lowers API costs
- Improves performance
- Retrieves messages faster

---

# 🔵 Dead Letter Queue (DLQ)

A **Dead Letter Queue (DLQ)** stores messages that repeatedly fail processing.

```text
Main Queue

↓

Retry

↓

Retry

↓

Retry

↓

Dead Letter Queue
```

Instead of retrying forever, failed messages are moved to the DLQ.

### Benefits

- Prevents endless retries
- Makes debugging easier
- Keeps the main queue healthy

---

# 🔵 Message Retention Period

Message Retention determines how long SQS stores a message before deleting it automatically.

The retention period can be configured from **1 minute to 14 days**.

Example:

```
Retention Period

↓

4 Days

↓

Message Automatically Deleted
```

---

# 🔵 Delay Queue

Delay Queue postpones message delivery.

Example:

```
Producer

↓

Send Message

↓

Wait 30 Seconds

↓

Consumer Receives Message
```

Useful when messages should not be processed immediately.

---

# 🔵 Security

Amazon SQS provides multiple security features.

## IAM Permissions

Use IAM policies to control who can:

- Create Queues
- Delete Queues
- Send Messages
- Receive Messages
- Delete Messages

---

## Encryption using AWS KMS

Amazon SQS supports server-side encryption using **AWS Key Management Service (KMS)**.

Messages remain encrypted while stored inside the queue.

---

## Queue Access Policies

Queue policies allow you to control which AWS accounts or services can access the queue.

---

# 🔵 Monitoring using Amazon CloudWatch

Amazon SQS integrates with Amazon CloudWatch.

Common metrics include:

- Messages Sent
- Messages Received
- Messages Deleted
- Approximate Queue Size
- Oldest Message Age
- Empty Receives

CloudWatch helps monitor queue health and application performance.

---

# 🔵 Amazon SQS vs Amazon SNS

| Amazon SQS                       | Amazon SNS                               |
| -------------------------------- | ---------------------------------------- |
| Message Queue                    | Notification Service                     |
| Pull-based                       | Push-based                               |
| One Consumer processes a message | One Message sent to Multiple Subscribers |
| Stores Messages                  | Doesn't store messages long-term         |
| Used for Background Processing   | Used for Notifications                   |

---

# 🔵 Pricing

Amazon SQS follows a **pay-as-you-go** pricing model.

You pay based on:

- Number of requests
- Data transfer (if applicable)

There are no upfront costs.

---

# 🔵 AWS Free Tier

AWS Free Tier includes:

- **1 Million Amazon SQS requests per month**

This is sufficient for learning, testing, and small projects.

---

# 🔵 Common Use Cases

Amazon SQS is commonly used for:

- Microservices Communication
- Order Processing
- Background Job Processing
- Image Processing
- Video Processing
- Payment Processing
- Email Queues
- Event-Driven Applications
- Decoupling Distributed Systems
- Batch Processing

---

# 🔵 Hands-on Demo – Amazon SQS

In this hands-on demo, you will learn how to:

- Create an SQS Queue
- Send Messages
- Receive Messages
- Delete Messages
- Configure Visibility Timeout
- Configure a Dead Letter Queue
- Integrate SQS with SNS
- Integrate SQS with Lambda
- Monitor SQS using CloudWatch

---

# 🔵 Step 1 – Create an SQS Queue

An SQS Queue stores messages until a consumer processes them.

### Navigation

```text
AWS Console → Amazon SQS → Create Queue
```

### Configuration

Queue Type

- Standard Queue
- FIFO Queue

Queue Name

```text
My-Order-Queue
```

Click **Create Queue**.

Your queue is now ready to receive messages.

---

# 🔵 Step 2 – Configure Queue Settings

While creating the queue, you can configure:

- Visibility Timeout
- Message Retention Period
- Delivery Delay
- Maximum Message Size
- Receive Message Wait Time (Long Polling)
- Server-side Encryption (KMS)

Most settings can also be modified later.

---

# 🔵 Step 3 – Send Messages

Open your queue.

Click:

```text
Send and Receive Messages
```

Example Message

```text
Order ID : 1001

Product : Laptop

Quantity : 1
```

Click **Send Message**.

The message is now stored in the queue.

---

# 🔵 Step 4 – Receive Messages

Click:

```text
Poll for Messages
```

Amazon SQS retrieves available messages.

Applications such as EC2, Lambda, or containers can also retrieve messages using the AWS SDK.

---

# 🔵 Step 5 – Delete Messages

After successfully processing a message, delete it.

```text
Receive Message

↓

Process Message

↓

Delete Message
```

Deleting the message prevents it from being processed again.

---

# 🔵 Step 6 – Configure Visibility Timeout

### Navigation

```text
Queue → Edit → Visibility Timeout
```

Example

```text
30 Seconds
```

During this period, no other consumer can process the same message.

If processing fails, the message becomes visible again after the timeout expires.

---

# 🔵 Step 7 – Configure Dead Letter Queue (DLQ)

Create another queue to act as the Dead Letter Queue.

### Workflow

```text
Main Queue

↓

Edit

↓

Dead Letter Queue

↓

Select DLQ
```

Set the **Maximum Receive Count**.

Example:

```text
5
```

After five failed processing attempts, the message is automatically moved to the Dead Letter Queue.

---

# 🔵 Step 8 – Amazon SNS to Amazon SQS Integration

Amazon SNS can automatically publish messages to an SQS queue.

```text
Application
      │
Publish Message
      ▼
Amazon SNS Topic
      │
      ▼
Amazon SQS Queue
      │
      ▼
Consumer
```

This enables asynchronous communication between applications.

---

# 🔵 Step 9 – AWS Lambda to Amazon SQS Integration

AWS Lambda can automatically process messages from an SQS queue.

```text
Producer
      │
      ▼
Amazon SQS Queue
      │
Trigger
      ▼
AWS Lambda
      │
      ▼
Process Message
```

Whenever a new message arrives, Lambda automatically executes.

---

# 🔵 Step 10 – Monitor Amazon SQS

Amazon SQS integrates with Amazon CloudWatch.

### Navigation

```text
AWS Console → CloudWatch → Metrics → SQS
```

Monitor important metrics such as:

- Messages Sent
- Messages Received
- Messages Deleted
- Approximate Queue Size
- Approximate Age of Oldest Message
- Empty Receives

These metrics help monitor queue health and troubleshoot performance issues.

---

# 🔵 Real-World Project Example

## Order Processing System

```text
Customer
      │
Place Order
      ▼
Web Application
      │
Send Message
      ▼
Amazon SQS Queue
      │
Receive Message
      ▼
Order Service
      │
Process Order
      ▼
Database
      │
      ▼
Email Notification
```

### Workflow

1. A customer places an order.
2. The web application sends the order details to Amazon SQS.
3. The Order Service retrieves the message from the queue.
4. The order is processed.
5. The message is deleted after successful processing.
6. If processing repeatedly fails, the message is automatically moved to the Dead Letter Queue.

This architecture improves scalability, reliability, fault tolerance, and allows producers and consumers to work independently.

---

# 🔵 Summary

- Amazon SQS is a fully managed message queuing service.
- It enables asynchronous communication between applications.
- Producers send messages to a queue, and consumers process them independently.
- Standard Queues provide high throughput, while FIFO Queues guarantee ordering and exactly-once processing.
- Visibility Timeout prevents multiple consumers from processing the same message simultaneously.
- Long Polling reduces empty responses and lowers API costs.
- Dead Letter Queues store failed messages for troubleshooting.
- Amazon SQS integrates with SNS, Lambda, EC2, ECS, EKS, and many other AWS services.
- CloudWatch provides monitoring and metrics for queue performance.
- Amazon SQS is widely used for microservices, background processing, event-driven architectures, and scalable distributed applications.
