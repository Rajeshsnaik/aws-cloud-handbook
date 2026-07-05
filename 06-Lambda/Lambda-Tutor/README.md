# AWS Lambda Tutorial

This guide walks through the complete process of creating, deploying, testing, and invoking an AWS Lambda function using the AWS Management Console. By the end of this tutorial, you will understand how to create a serverless function, write code, test it, attach event sources, and deploy it for production use.

---

# 🔵 Prerequisites

Before creating a Lambda function, make sure you have:

- An AWS Account
- Access to the AWS Management Console
- Basic knowledge of IAM
- Permission to create Lambda functions and IAM Roles

---

# 🔵 AWS Lambda Function Creation Flow

The following workflow represents the complete lifecycle of creating a Lambda function from the AWS Console.

```text
AWS Console
     │
     ▼
Search Lambda
     │
     ▼
Create Function
     │
     ▼
Author From Scratch
     │
     ▼
Enter Function Name
     │
     ▼
Select Runtime
(Node.js / Python / Java)
     │
     ▼
Select Architecture
(x86_64 / ARM64)
     │
     ▼
Configure IAM Role
(Default or Custom)
     │
     ▼
Create Function
     │
     ▼
Write / Upload Code
     │
     ▼
Deploy Function
     │
     ▼
Create Test Event
     │
     ▼
Test Function
     │
     ▼
Add Trigger
(S3 / API Gateway / SNS / SQS)
     │
     ▼
Lambda Ready
```

> **Architecture Diagram**

```text
docs/images/lambda-function-creation-flow.png
```

Replace the above path with your actual image location inside the repository.

---

# 🔵 Step 1: Open AWS Lambda Console

Sign in to the AWS Management Console.

Navigate to:

```text
AWS Console
    ↓
Services
    ↓
Lambda
```

Click **Create Function** to begin creating a new Lambda function.

---

# 🔵 Step 2: Choose a Creation Method

AWS provides multiple methods for creating a Lambda function.

| Method              | Description                              |
| ------------------- | ---------------------------------------- |
| Author from Scratch | Create a new function from the beginning |
| Use a Blueprint     | Create using AWS-provided templates      |
| Container Image     | Deploy a Docker image from Amazon ECR    |

For beginners, choose:

```text
Author From Scratch
```

---

# 🔵 Step 3: Enter Function Details

Provide the basic information required for the function.

Example:

```text
Function Name

hello-world-lambda
```

Choose a meaningful name that clearly identifies the function's purpose.

Recommended naming convention:

```text
<Application>-<Environment>-<Purpose>

Examples

inventory-dev-api

payment-prod-handler

image-upload-processor
```

---

# 🔵 Step 4: Select Runtime

The runtime specifies the programming language used by the Lambda function.

AWS currently supports several programming languages.

Common runtimes include:

```text
Node.js

Python

Java

.NET

Go

Ruby
```

Choose the runtime that matches your application.

Example:

```text
Node.js 22.x
```

---

# 🔵 Step 5: Select Architecture

AWS Lambda supports two processor architectures.

| Architecture     | Description                                      |
| ---------------- | ------------------------------------------------ |
| x86_64           | Traditional Intel/AMD processors                 |
| ARM64 (Graviton) | Better price-performance for supported workloads |

For learning purposes:

```text
x86_64
```

For production workloads focused on cost optimization:

```text
ARM64
```

---

# 🔵 Step 6: Configure Execution Role

Every Lambda function requires an **IAM Execution Role**.

The execution role determines which AWS services the function can access.

Options include:

```text
Create a new role with basic Lambda permissions

OR

Use an existing IAM Role
```

Examples of permissions:

- Amazon S3
- DynamoDB
- CloudWatch Logs
- SNS
- SQS

Always follow the **Principle of Least Privilege**, granting only the permissions required.

---

# 🔵 Step 7: Create the Function

After configuring the settings, click:

```text
Create Function
```

AWS automatically provisions the serverless execution environment and opens the Lambda Function dashboard.

---

# 🔵 Step 8: Write or Upload Function Code

You can write code directly inside the AWS Console or upload your application.

AWS supports:

- Inline Editor
- ZIP Deployment
- Container Image Deployment

Example (Node.js):

```javascript
export const handler = async (event) => {
  return {
    statusCode: 200,
    body: JSON.stringify({
      message: "Hello from AWS Lambda!",
    }),
  };
};
```

---

# 🔵 Step 9: Deploy the Function

After writing or modifying the code, click:

```text
Deploy
```

Deployment uploads the latest version of your code to AWS Lambda.

Only deployed code is executed during testing or production invocations.

---

# 🔵 Step 10: Create a Test Event

A Test Event simulates a request without requiring another AWS service.

Click:

```text
Test
    ↓
Configure Test Event
```

Example Event:

```json
{
  "name": "John"
}
```

Save the event.

---

# 🔵 Step 11: Test the Function

Click:

```text
Test
```

AWS executes the Lambda function and displays:

- Response
- Execution Duration
- Memory Usage
- Request ID
- Logs

Example Output:

```json
{
  "statusCode": 200,
  "body": "{\"message\":\"Hello from AWS Lambda!\"}"
}
```

---

# 🔵 Step 12: View Logs

Every Lambda execution automatically writes logs to Amazon CloudWatch.

Navigate to:

```text
Monitor
      ↓
View CloudWatch Logs
```

You can monitor:

- Console Output
- Errors
- Execution Time
- Memory Usage

Example:

```javascript
console.log("Lambda Started");
```

The output appears automatically inside CloudWatch Logs.

---

# 🔵 Step 13: Add a Trigger

A Lambda function becomes useful when connected to an event source.

Click:

```text
Add Trigger
```

Supported trigger sources include:

| Service            | Use Case               |
| ------------------ | ---------------------- |
| API Gateway        | REST APIs              |
| Amazon S3          | File Upload Processing |
| Amazon SQS         | Queue Processing       |
| Amazon SNS         | Notifications          |
| Amazon EventBridge | Scheduled Jobs         |
| DynamoDB Streams   | Database Changes       |

Example:

```text
S3 Upload
      ↓
Lambda Triggered
      ↓
Resize Image
      ↓
Save Processed Image
```

---

# 🔵 Common Lambda Workflow

A typical serverless application follows this workflow:

```text
User Request
      │
      ▼
API Gateway
      │
      ▼
AWS Lambda
      │
      ▼
Business Logic
      │
      ▼
Amazon DynamoDB
      │
      ▼
Response
```

AWS automatically manages scaling, infrastructure, monitoring, and availability throughout the process.

---

# 🔵 Best Practices

When creating Lambda functions, follow these best practices:

- Keep functions small and focused on a single task.
- Use IAM Roles instead of storing AWS credentials.
- Store secrets in AWS Secrets Manager or Systems Manager Parameter Store.
- Reuse SDK clients outside the handler to improve performance.
- Enable CloudWatch logging for monitoring and debugging.
- Minimize deployment package size.
- Use Lambda Layers for shared dependencies.
- Configure appropriate memory and timeout values.
- Monitor errors and throttles using CloudWatch Metrics.
- Use Provisioned Concurrency for latency-sensitive applications.

---

# 🔵 Video Tutorials

The following videos provide practical demonstrations of AWS Lambda concepts and implementation.

```text
https://youtu.be/_z07kmHEMzM?si=u0pHCfm7vrNatGI2
```

```text
https://youtu.be/XFGSuj83wdc?si=KIu08qAhHL04N5tv
```

```text
https://youtu.be/4AgWKVBOjVc?si=zyQCCdhTrEkGw6g9
```

```text
https://youtu.be/5fTtmeCpSRw?si=IhPoKanVL5XTvV0O
```

---

# 🔵 Summary

AWS Lambda enables developers to build scalable, event-driven applications without managing servers. Creating a Lambda function involves selecting a runtime, configuring an IAM role, writing and deploying code, testing the function, and attaching event sources such as API Gateway, Amazon S3, Amazon SQS, or Amazon SNS. By leveraging automatic scaling, integrated monitoring, and pay-per-use pricing, Lambda simplifies application development while reducing operational overhead.
