# AWS Lambda

![AWS](./aws-lambda.png)

AWS Lambda is a fully managed **serverless compute service** that allows you to run code without provisioning, managing, or maintaining servers. You simply upload your code, configure a trigger, and AWS automatically handles infrastructure provisioning, scaling, availability, and maintenance.

Lambda is designed for **event-driven applications**, where code executes only when an event occurs. Since you only pay for the actual execution time, Lambda is a cost-effective solution for workloads that do not require continuously running servers.

---

# 🔵 What is AWS Lambda?

AWS Lambda is a serverless compute service provided by AWS that runs your application code in response to events. AWS automatically manages the underlying infrastructure, including server provisioning, operating system maintenance, scaling, monitoring, and high availability.

Developers only need to focus on writing business logic while AWS handles everything else.

---

# 🔵 Why AWS Lambda?

Traditional applications require servers that remain running continuously, even when there is little or no traffic. This leads to infrastructure management overhead and unnecessary costs.

AWS Lambda eliminates these challenges by executing code only when an event occurs.

## Before AWS Lambda

In a traditional architecture, a user request reaches an application hosted on EC2 instances.

```
User
   ↓
Load Balancer
   ↓
EC2 Instance
   ↓
Application
```

The development or operations team is responsible for:

- Managing servers
- Installing operating system updates
- Scaling infrastructure
- Monitoring server health
- Applying security patches
- Capacity planning

---

## With AWS Lambda

In a serverless architecture, incoming requests directly invoke a Lambda function.

```
User
   ↓
API Gateway
   ↓
AWS Lambda
   ↓
Business Logic
   ↓
Database / AWS Services
```

AWS automatically manages:

- Server provisioning
- Infrastructure scaling
- High availability
- Patch management
- Operating system updates

Developers only focus on writing and deploying application code.

---

# 🔵 Lambda Architecture

A typical Lambda workflow looks like this:

```text
Event Source
      │
      ▼
Lambda Function
      │
      ▼
Business Logic
      │
      ▼
AWS Services / Database
```

Examples of event sources include API Gateway, Amazon S3, Amazon SQS, Amazon SNS, EventBridge, and DynamoDB Streams.

---

# 🔵 Core Components

Every Lambda function consists of several important components.

## Lambda Function

The Lambda Function contains the application code that executes whenever it is triggered.

---

## Event

An **Event** is the input data passed to the Lambda function. Different AWS services generate different event formats.

Examples:

- API Request
- File Upload
- Queue Message
- Scheduled Event

---

## Handler

The **Handler** is the entry point of a Lambda function. AWS invokes this method whenever the function executes.

Example (Node.js):

```javascript
exports.handler = async (event) => {
  return {
    statusCode: 200,
    body: "Hello World",
  };
};
```

---

## Runtime

The **Runtime** defines the programming language environment used to execute your code.

Supported runtimes include:

- Node.js
- Python
- Java
- Go
- .NET
- Ruby
- Custom Runtime

---

# 🔵 How AWS Lambda Works

Whenever Lambda receives a request, AWS creates or reuses an execution environment and runs the function.

```text
Request Received
        │
        ▼
Execution Environment Created
        │
        ▼
Function Code Executes
        │
        ▼
Response Returned
        │
        ▼
Environment Reused (if available)
```

---

# 🔵 Lambda Execution Lifecycle

## Cold Start

A **Cold Start** occurs when no execution environment is available.

AWS performs the following steps:

- Creates a new execution environment
- Initializes the runtime
- Loads the function code
- Initializes dependencies
- Executes the function

Because of these initialization steps, the first request usually experiences slightly higher latency.

---

## Warm Start

If an execution environment already exists, AWS reuses it for subsequent requests.

Benefits:

- Faster execution
- Lower latency
- No runtime initialization

Warm starts significantly improve application performance.

---

# 🔵 Lambda Scaling

AWS Lambda automatically scales based on incoming requests.

If one request arrives, Lambda creates one execution environment.

If one thousand requests arrive simultaneously, Lambda automatically creates multiple execution environments to process them concurrently.

There is no need to manually provision servers or configure scaling policies.

---

# 🔵 Invocation Types

Lambda supports two invocation models.

## Synchronous Invocation

The caller waits until the Lambda function finishes executing and returns a response.

Common use cases:

- REST APIs
- Web Applications
- Mobile Applications
- Real-time Processing

---

## Asynchronous Invocation

The caller immediately receives an acknowledgment while Lambda executes the function in the background.

Common use cases:

- File Processing
- Notifications
- Background Jobs
- Event Processing
- Data Pipelines

---

# 🔵 Event Sources

AWS Lambda integrates with many AWS services that can automatically trigger function execution.

| AWS Service        | Common Use Case            |
| ------------------ | -------------------------- |
| API Gateway        | REST APIs                  |
| Amazon S3          | File Upload Processing     |
| Amazon SNS         | Notifications              |
| Amazon SQS         | Queue Processing           |
| Amazon EventBridge | Scheduled Jobs             |
| DynamoDB Streams   | Database Change Processing |

---

# 🔵 Lambda Limits

Some important Lambda service limits include:

| Feature                    | Limit                                     |
| -------------------------- | ----------------------------------------- |
| Maximum Execution Time     | 15 Minutes                                |
| Memory                     | 128 MB – 10 GB                            |
| Ephemeral Storage (`/tmp`) | Up to 10 GB                               |
| Deployment Package         | AWS Size Limits Apply                     |
| Concurrency                | Controlled by Account and Function Limits |

---

# 🔵 Lambda Permissions

AWS Lambda uses **IAM Roles** to securely access other AWS services.

Instead of storing AWS credentials inside your code, attach an IAM Role with the required permissions.

Example IAM Policy:

```json
{
  "Effect": "Allow",
  "Action": ["s3:GetObject"],
  "Resource": "*"
}
```

---

# 🔵 Lambda Layers

Lambda Layers allow you to package and share common libraries, dependencies, and custom code across multiple Lambda functions.

Benefits include:

- Code Reusability
- Smaller Deployment Packages
- Easier Dependency Management
- Centralized Library Updates

---

# 🔵 Lambda Destinations

Lambda Destinations determine where execution results are sent after a function completes.

If execution succeeds, Lambda can send the result to services such as:

- Amazon SNS
- Amazon SQS
- Amazon EventBridge
- Another Lambda Function

If execution fails, the failure event can also be routed for additional processing or error handling.

This helps build reliable event-driven workflows.

---

# 🔵 Lambda Versions

Each published Lambda Version is an immutable snapshot of the function code and configuration.

Once published:

- It cannot be modified.
- It always points to the same code.
- It enables safe deployments and rollbacks.

---

# 🔵 Provisioned Concurrency

Provisioned Concurrency keeps Lambda execution environments initialized and ready before requests arrive.

Benefits:

- Eliminates most Cold Starts
- Lower latency
- Predictable response times
- Better user experience for real-time applications

---

# 🔵 Reserved Concurrency

Reserved Concurrency limits the maximum number of concurrent executions for a Lambda function.

Example:

```
Reserved Concurrency = 100
```

The function will never execute more than 100 concurrent requests.

Benefits:

- Controls scaling
- Protects downstream systems
- Prevents resource exhaustion

---

# 🔵 Lambda Monitoring

AWS Lambda integrates with **Amazon CloudWatch** for monitoring, logging, and debugging.

CloudWatch automatically stores application logs such as:

```javascript
console.log("Hello World");
```

Important CloudWatch metrics include:

- Invocations
- Errors
- Duration
- Throttles
- Concurrent Executions
- Success Rate

---

# 🔵 Lambda Pricing

AWS Lambda follows a **pay-per-use** pricing model.

You are charged based on:

- Number of Requests
- Execution Duration
- Allocated Memory

Unlike EC2, there is no cost when the function is idle.

---

# 🔵 Lambda Deployment Methods

Lambda functions can be deployed using two methods.

## ZIP Deployment

Application code and dependencies are packaged into a ZIP file and uploaded to AWS Lambda.

Suitable for:

- Small Applications
- Standard Deployments

---

## Container Image Deployment

The Lambda function is packaged as a Docker image and stored in Amazon ECR before deployment.

Suitable for:

- Large Applications
- Custom Runtimes
- Complex Dependencies

---

# 🔵 Difference Between EC2 and Lambda

| Amazon EC2                             | AWS Lambda                             |
| -------------------------------------- | -------------------------------------- |
| Requires server management             | Serverless                             |
| Manual scaling                         | Automatic scaling                      |
| Pay for running instances              | Pay only for execution                 |
| Suitable for long-running applications | Suitable for event-driven applications |
| Always running                         | Runs only when triggered               |

---

# 🔵 What is Cold Start?

A **Cold Start** is the additional startup time required when AWS Lambda creates a new execution environment before executing a function.

---

# 🔵 Maximum Lambda Timeout

```text
15 Minutes
```

---

# 🔵 Can Lambda Access a Private RDS Database?

Yes.

A Lambda function can access a private Amazon RDS database by configuring the Lambda function inside the same VPC (or a connected VPC) and allowing the required Security Group permissions.

---

# 🔵 Why Use Lambda Layers?

Lambda Layers allow multiple Lambda functions to share common libraries, frameworks, and dependencies, reducing deployment package size and improving code reusability.

---

# 🔵 Difference Between Reserved and Provisioned Concurrency

| Reserved Concurrency                 | Provisioned Concurrency           |
| ------------------------------------ | --------------------------------- |
| Limits maximum concurrent executions | Keeps execution environments warm |
| Controls scaling                     | Reduces Cold Starts               |
| Protects downstream services         | Improves response time            |

---

# 🔵 What is a Dead Letter Queue (DLQ)?

A **Dead Letter Queue (DLQ)** stores failed asynchronous events that Lambda could not process successfully.

DLQs help developers:

- Investigate failures
- Retry failed events
- Prevent data loss

Amazon SQS and Amazon SNS are commonly used as Dead Letter Queues.

---

# 🔵 Lambda in One Line

> AWS Lambda is a fully managed serverless compute service that executes code in response to events, automatically manages infrastructure, scales on demand, and charges only for the actual execution time.

---

# 🔵 Why Was AWS Lambda Introduced?

Before Lambda, applications were commonly deployed on EC2 instances that continued running even when there were no incoming requests. This resulted in unnecessary infrastructure costs and operational overhead.

AWS Lambda was introduced to eliminate server management by providing an event-driven, serverless execution model where code runs only when required. This allows developers to build scalable applications while paying only for actual execution time.
