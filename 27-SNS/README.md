# 🔵 Amazon SNS (Simple Notification Service)

![aws](./aws-sns.png)

Amazon **Simple Notification Service (SNS)** is a **fully managed messaging and notification service** provided by AWS that enables applications to send messages to multiple subscribers simultaneously.

It follows the **Publish-Subscribe (Pub/Sub)** messaging model, where a single message can be delivered to multiple subscribers at the same time.

In simple words:

> **Amazon SNS = Send one message to many subscribers instantly.**

---

# 🔵 Why Amazon SNS?

In modern applications, multiple systems often need to be notified when an event occurs.

For example:

- A new order is placed.
- A payment is successful.
- A server goes down.
- CPU usage becomes high.
- A new file is uploaded to Amazon S3.

Instead of sending separate notifications to every service, the application publishes one message to an SNS Topic, and SNS delivers it to all subscribers.

### Benefits

- Real-time notifications
- One-to-many messaging
- Fully managed service
- Highly scalable
- Highly available
- Easy integration with AWS services
- Supports event-driven architectures

---

# 🔵 How Amazon SNS Works

Amazon SNS follows the **Publish-Subscribe (Pub/Sub)** model.

```
Publisher
     │
Publish Message
     ▼
SNS Topic
     │
     ├──► Email
     ├──► SMS
     ├──► Amazon SQS
     ├──► AWS Lambda
     ├──► HTTP/HTTPS
     └──► Mobile Push Notification
```

A publisher sends **one message** to an SNS Topic.

SNS automatically delivers that message to **every subscribed endpoint**.

---

# 🔵 What is a Topic?

A **Topic** is a communication channel in Amazon SNS.

Publishers send messages to a Topic, and all subscribers attached to that Topic receive the message.

Think of a Topic as a WhatsApp group.

- SNS Topic = WhatsApp Group
- Publisher = Person sending the message
- Subscribers = Group members

One message reaches everyone in the group.

---

# 🔵 What is a Publisher?

A **Publisher** is any AWS service, application, or user that sends messages to an SNS Topic.

Common publishers include:

- Amazon CloudWatch
- Amazon EC2
- AWS Lambda
- Amazon S3
- Custom Applications
- AWS CodePipeline
- AWS CodeBuild

Example:

CloudWatch detects high CPU usage and publishes an alert to an SNS Topic.

---

# 🔵 What is a Subscriber?

A **Subscriber** is any endpoint that receives messages from an SNS Topic.

Common subscribers include:

- Email
- SMS
- Amazon SQS
- AWS Lambda
- HTTP/HTTPS Endpoints
- Mobile Applications

Every subscriber attached to the Topic receives the published message.

---

# 🔵 Publish-Subscribe (Pub/Sub) Model

The Pub/Sub model separates the sender from the receivers.

Instead of sending messages directly to each receiver, the publisher sends one message to the SNS Topic.

SNS then distributes the message to all subscribers.

```
Application

        │

Publish

        ▼

SNS Topic

   │     │     │

   ▼     ▼     ▼

Email   SQS   Lambda
```

### Advantages

- Loose coupling between applications
- Easy to scale
- Faster communication
- One message reaches multiple services
- Supports microservices architecture

---

# 🔵 Supported Subscription Protocols

Amazon SNS supports multiple subscription types.

## Email

Sends notification emails to subscribers.

Example:

```
Server Down

↓

Email Notification
```

---

## SMS

Sends text messages directly to mobile phones.

Example:

```
OTP

↓

SMS
```

---

## HTTP/HTTPS

Delivers notifications to web applications through HTTP or HTTPS endpoints.

Example:

```
SNS

↓

REST API

↓

Application
```

---

## Amazon SQS

Sends messages to Amazon SQS for asynchronous processing.

Example:

```
SNS

↓

SQS Queue

↓

Worker Application
```

---

## AWS Lambda

Triggers a Lambda function whenever a message is published.

Example:

```
SNS

↓

Lambda Function

↓

Process Message
```

---

## Mobile Push Notifications

Sends push notifications to mobile devices.

Supported platforms include:

- Android (FCM)
- Apple Push Notification Service (APNs)

---

# 🔵 SNS Fan-Out Pattern

One of the most popular SNS architectures is the **Fan-Out Pattern**.

A single message is copied and delivered to multiple SQS queues.

```
Application
      │
      ▼
 SNS Topic
  │   │   │
  ▼   ▼   ▼
SQS1 SQS2 SQS3
```

Each queue processes the message independently.

This is commonly used in microservices.

---

# 🔵 Message Filtering

Sometimes, not every subscriber needs every message.

SNS allows subscribers to receive only relevant messages using **Filter Policies**.

Example:

```
Published Messages

Order Created

Order Cancelled

Payment Success

Payment Failed
```

Subscriber A only wants:

```
Payment Success
```

Subscriber B only wants:

```
Order Created
```

SNS automatically filters messages before delivery.

### Benefits

- Reduces unnecessary notifications
- Improves performance
- Saves processing time
- Supports targeted message delivery

---

# 🔵 Message Attributes

SNS supports **Message Attributes**.

These are additional key-value pairs attached to a message.

Example:

```
Message

Order Placed

Attributes

Type = Order

Region = India

Priority = High
```

These attributes are commonly used with Filter Policies.

---

# 🔵 Security

Amazon SNS provides multiple security features.

## IAM Permissions

IAM policies control who can:

- Create Topics
- Delete Topics
- Publish Messages
- Subscribe Endpoints
- Unsubscribe Endpoints

---

## Encryption using AWS KMS

SNS supports server-side encryption using **AWS Key Management Service (KMS)**.

Encryption protects messages while stored inside Amazon SNS.

---

## Access Policies

SNS Topics support resource-based policies.

These policies allow you to specify which AWS accounts or services can publish or subscribe.

---

# 🔵 Monitoring using Amazon CloudWatch

Amazon SNS integrates with Amazon CloudWatch.

CloudWatch provides monitoring and metrics for SNS Topics.

Common metrics include:

- Number of Messages Published
- Number of Notifications Delivered
- Number of Failed Deliveries
- Number of Notifications Filtered Out
- Topic Activity

CloudWatch Alarms can also send notifications through SNS.

---

# 🔵 SNS with CloudWatch

A common monitoring workflow looks like this.

```
EC2 Instance

      │

CPU > 80%

      ▼

CloudWatch Alarm

      ▼

SNS Topic

      ▼

Email / SMS
```

Whenever the alarm enters the **Alarm** state, CloudWatch publishes a message to SNS.

SNS then sends notifications to all subscribers.

---

# 🔵 SNS vs SQS

| Amazon SNS                        | Amazon SQS                                    |
| --------------------------------- | --------------------------------------------- |
| Push-based messaging              | Pull-based messaging                          |
| One-to-many communication         | One-to-one communication                      |
| Sends notifications instantly     | Stores messages until consumers retrieve them |
| No message persistence by default | Messages remain in the queue until processed  |
| Used for notifications            | Used for message processing                   |

---

# 🔵 Pricing

Amazon SNS follows a **pay-as-you-go** pricing model.

You pay based on:

- Number of publish requests
- Number of notification deliveries
- SMS usage
- Data transfer (if applicable)

There are no upfront costs.

---

# 🔵 AWS Free Tier

The AWS Free Tier includes:

- 1 million SNS publish requests per month

This is sufficient for learning, testing, and small projects.

**Note:**

SMS messaging is **not included** in the free tier and is billed separately.

---

# 🔵 Common Use Cases

Amazon SNS is widely used for:

- Email Notifications
- SMS Alerts
- CloudWatch Alarm Notifications
- EC2 Monitoring Alerts
- Application Event Notifications
- Order Confirmation Messages
- Payment Notifications
- Password Reset Notifications
- Microservices Communication
- Fan-Out Messaging
- Event-Driven Applications
- Server Health Monitoring

---

# 🔵 Real-World Example

Suppose an online shopping application receives a new order.

```
Customer Places Order
          │
          ▼
     Order Service
          │
Publish Message
          ▼
      SNS Topic
   │      │      │
   ▼      ▼      ▼
 Email    SQS   Lambda
(Customer)(Inventory)(Billing)
```

Here:

- Customer receives an order confirmation email.
- Inventory service updates stock.
- Billing service generates an invoice.

The application publishes only **one message**, while SNS automatically distributes it to all required services.

---

# 🔵 Summary

- Amazon SNS is a fully managed messaging and notification service.
- It follows the Publish-Subscribe (Pub/Sub) messaging model.
- One published message can be delivered to multiple subscribers.
- SNS Topics act as communication channels between publishers and subscribers.
- SNS supports Email, SMS, HTTP/HTTPS, SQS, Lambda, and Mobile Push Notifications.
- Message Filtering allows subscribers to receive only relevant messages.
- SNS integrates seamlessly with CloudWatch, Lambda, SQS, EC2, S3, and many other AWS services.
- IAM, KMS encryption, and Topic Policies provide secure access control.
- The Fan-Out pattern enables one message to be processed by multiple independent services.
- Amazon SNS is commonly used for notifications, alerts, monitoring, and event-driven architectures.

```

```

---

# 🔵 Hands-on Demo – Amazon SNS

In this hands-on demo, you will learn how to:

- Create an SNS Topic
- Subscribe an Email
- Publish a Message
- Send Email Notifications
- Integrate SNS with Amazon SQS
- Integrate SNS with AWS Lambda
- Send CloudWatch Alarm Notifications
- Monitor SNS using CloudWatch

---

# 🔵 Step 1 – Create an SNS Topic

An SNS Topic is a communication channel where publishers send messages and subscribers receive them.

### Navigation

```text
AWS Console → Amazon SNS → Topics → Create Topic
```

### Configuration

- Type → Standard
- Name → My-SNS-Topic
- Leave the remaining settings as default
- Click **Create Topic**

After creation, the Topic is ready to receive published messages.

---

# 🔵 Step 2 – Create an Email Subscription

A subscription tells SNS where to send notifications.

### Navigation

```text
Amazon SNS → Topics → My-SNS-Topic → Create Subscription
```

### Configuration

- Protocol → Email
- Endpoint → your-email@example.com

Click **Create Subscription**.

The subscription will remain in **Pending Confirmation** until it is verified.

---

# 🔵 Step 3 – Confirm the Subscription

SNS sends a confirmation email to the specified email address.

Open your inbox and click:

```text
Confirm Subscription
```

After confirmation, the subscription status changes to:

```text
Confirmed
```

Your email address is now ready to receive notifications.

---

# 🔵 Step 4 – Publish a Message

Now publish a test notification.

### Navigation

```text
Amazon SNS → Topics → My-SNS-Topic → Publish Message
```

### Configuration

Subject (Optional)

```text
Deployment Successful
```

Message

```text
Application deployed successfully.
```

Click **Publish Message**.

SNS immediately sends the message to every subscribed endpoint.

---

# 🔵 Step 5 – Verify Email Notification

Open your email inbox.

You should receive an email similar to:

```text
Subject:
Deployment Successful

Message:
Application deployed successfully.
```

This confirms that Amazon SNS is successfully delivering notifications.

---

# 🔵 Step 6 – Add Multiple Subscribers

An SNS Topic can deliver the same message to multiple subscribers at the same time.

Example:

```text
                SNS Topic
                    │
      ┌─────────────┼─────────────┐
      │             │             │
      ▼             ▼             ▼
Admin Email   Amazon SQS    AWS Lambda
      │
      ▼
Developer Email
```

Whenever a message is published, every subscriber receives a copy.

---

# 🔵 Step 7 – SNS to Amazon SQS Integration

Amazon SNS can send messages directly to an Amazon SQS queue.

### Navigation

```text
Amazon SNS → Topic → Create Subscription
```

### Configuration

- Protocol → Amazon SQS
- Endpoint → Select your SQS Queue
- Click **Create Subscription**

### Message Flow

```text
Application
      │
Publish Message
      ▼
SNS Topic
      │
      ▼
Amazon SQS
      │
      ▼
Consumer Application
```

Whenever a message is published, it is automatically stored in the SQS queue for asynchronous processing.

---

# 🔵 Step 8 – SNS to AWS Lambda Integration

SNS can automatically invoke a Lambda function whenever a message is published.

### Navigation

```text
Amazon SNS → Topic → Create Subscription
```

### Configuration

- Protocol → AWS Lambda
- Endpoint → Select Lambda Function

Click **Create Subscription**.

### Message Flow

```text
Application
      │
Publish Message
      ▼
SNS Topic
      │
      ▼
AWS Lambda
      │
      ▼
Execute Function
```

Every published message automatically triggers the Lambda function.

This is commonly used in serverless applications.

---

# 🔵 Step 9 – CloudWatch Alarm to SNS

One of the most common uses of SNS is infrastructure monitoring.

CloudWatch publishes alarm notifications to an SNS Topic.

### Create SNS Topic

```text
AWS Console → Amazon SNS → Topics → Create Topic
```

Example:

```text
CloudWatch-Notification
```

Subscribe your email to the Topic.

---

### Create CloudWatch Alarm

```text
AWS Console → CloudWatch → Alarms → Create Alarm
```

Select:

- EC2 Instance
- CPUUtilization Metric

Set:

```text
Threshold = Greater than 80%
```

Notification

```text
Select Existing SNS Topic

↓

CloudWatch-Notification
```

Click **Create Alarm**.

---

# 🔵 CloudWatch Alarm Workflow

```text
EC2 Instance
      │
CPU Utilization > 80%
      ▼
CloudWatch Alarm
      │
      ▼
SNS Topic
      │
      ▼
Email Notification
      │
      ▼
Administrator
```

Whenever the CPU usage exceeds the configured threshold, CloudWatch publishes a message to the SNS Topic.

SNS then sends an email notification to all subscribers.

---

# 🔵 Step 10 – Monitor SNS

Amazon SNS integrates with Amazon CloudWatch for monitoring.

### Navigation

```text
AWS Console → CloudWatch → Metrics → SNS
```

You can monitor:

- Messages Published
- Notifications Delivered
- Failed Deliveries
- Number of Subscribers
- Topic Activity

These metrics help verify successful message delivery and troubleshoot issues.

---

# 🔵 Real-World Project Example

## EC2 CPU Monitoring

A DevOps team wants to receive alerts whenever an EC2 instance experiences high CPU usage.

### Architecture

```text
EC2 Instance
      │
      ▼
CloudWatch Metrics
      │
CPU Utilization > 80%
      ▼
CloudWatch Alarm
      │
      ▼
Amazon SNS Topic
      │
      ▼
Email Notification
      │
      ▼
System Administrator
```

### Workflow

1. CloudWatch continuously monitors the EC2 instance.
2. CPU utilization exceeds 80%.
3. CloudWatch changes the alarm state to **Alarm**.
4. CloudWatch publishes a notification to the SNS Topic.
5. Amazon SNS immediately sends an email to all subscribed administrators.
6. The administrator receives the alert and takes corrective action.

This is one of the most common real-world implementations of Amazon SNS for infrastructure monitoring, alerting, and incident response.

---

# 🔵 Summary

- Create an SNS Topic.
- Add one or more subscribers.
- Confirm email subscriptions.
- Publish messages to the Topic.
- SNS delivers notifications to all subscribers instantly.
- Integrate SNS with Amazon SQS for asynchronous processing.
- Integrate SNS with AWS Lambda for serverless event processing.
- Use CloudWatch Alarms with SNS for infrastructure monitoring.
- Monitor SNS metrics using Amazon CloudWatch.
- Amazon SNS is widely used for notifications, automation, and event-driven architectures.
