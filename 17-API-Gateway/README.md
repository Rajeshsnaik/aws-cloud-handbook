# Amazon API Gateway

![AWS](./aws-api-gateway.png)

---

# 🔵 What is an API?

An **API (Application Programming Interface)** is a communication interface that allows two or more software applications to exchange data without exposing their internal implementation.

For example, when a user opens a mobile application and requests profile information, the application sends a request to an API. The API communicates with the backend service or database, retrieves the required data, and returns it to the application.

The client never communicates directly with the database; all communication happens through APIs.

---

# 🔵 What is Amazon API Gateway?

**Amazon API Gateway** is a fully managed AWS service that enables developers to **create, publish, maintain, monitor, and secure APIs** at any scale.

It acts as a single entry point for client requests and routes them to backend services such as:

- AWS Lambda
- Amazon EC2
- Amazon ECS/EKS
- Private Applications
- External HTTP Endpoints

API Gateway also provides authentication, authorization, throttling, monitoring, logging, versioning, and request routing without requiring additional infrastructure.

---

# 🔵 Why Do We Need API Gateway?

Without API Gateway, clients would communicate directly with backend services, making security, authentication, request routing, monitoring, and rate limiting difficult to manage.

API Gateway solves these problems by acting as a centralized gateway between clients and backend services. It improves scalability, simplifies API management, and provides built-in security and monitoring capabilities.

---

# 🔵 Key Features

- Create and publish APIs
- Secure APIs using IAM, Cognito, Lambda Authorizers, or JWT
- Request validation
- Traffic throttling and rate limiting
- API versioning
- Request and response transformation
- CloudWatch logging and monitoring
- Caching support (REST APIs)
- Custom domain names
- Automatic scaling
- Integration with AWS services

---

# 🔵 What Can We Build Using API Gateway?

API Gateway can expose different backend workloads, including:

- REST APIs
- HTTP APIs
- WebSocket APIs
- AWS Lambda Functions
- EC2 Applications
- ECS Applications
- EKS Applications
- Private Services
- External HTTP Services

---

# 🔵 Types of APIs

AWS API Gateway supports three API types.

| API Type      | Description                                                            |
| ------------- | ---------------------------------------------------------------------- |
| HTTP API      | High-performance and low-cost APIs for serverless and web applications |
| REST API      | Feature-rich APIs with advanced API management capabilities            |
| WebSocket API | Real-time bidirectional communication between client and server        |

---

# 🔵 Common HTTP Methods

## GET

Retrieves data from the server without modifying it.

Example:

```http
GET /users
```

Use Cases

- Fetch user details
- View products
- Read records

---

## POST

Creates a new resource by sending data to the server.

```http
POST /users
```

```json
{
  "name": "Virat Kohli"
}
```

Use Cases

- Register users
- Create orders
- Submit forms

---

## PUT

Completely replaces an existing resource.

```http
PUT /users/1
```

Use Cases

- Update an entire profile
- Replace existing records

---

## PATCH

Updates only specific fields of an existing resource.

```http
PATCH /users/1
```

```json
{
  "age": 37
}
```

Use Cases

- Update email
- Change phone number
- Modify profile details

---

## DELETE

Removes an existing resource.

```http
DELETE /users/1
```

Use Cases

- Delete users
- Remove products
- Cancel orders

---

# 🔵 Architecture

```
User
   │
   ▼
API Gateway
   │
   ▼
AWS Lambda
```

---

# 🔵 Request Flow

1. The client sends an HTTP request.
2. API Gateway receives the request.
3. API Gateway validates and routes the request.
4. Lambda executes the business logic.
5. Lambda returns the response.
6. API Gateway sends the response back to the client.

---

# 🔵 Demo - HTTP API with Lambda

## Step 1 - Create Lambda Function

Navigate to

```
AWS Lambda
```

Click

```
Create Function
```

Provide

```
Function Name

get-user-api
```

Runtime

```
Python 3.10
```

Click

```
Create Function
```

Replace the default handler code and click

```
Deploy
```

---

## Step 2 - Test Lambda

Click

```
Test
```

Event Name

```
GetUserTest
```

Keep Event JSON empty.

```json
{}
```

Click

```
Save
```

Click

```
Test
```

Verify the response.

---

## Step 3 - Create HTTP API

Navigate to

```
Amazon API Gateway
```

Choose

```
HTTP API
```

Click

```
Build
```

Provide

```
API Name
```

Choose

```
IPv4
```

Keep other settings as default.

Click

```
Create
```

At this point, the API exists but is not connected to Lambda.

---

## Step 4 - Create Route

Navigate to

```
Routes
```

Method

```
GET
```

Path

```
/users
```

Click

```
Create
```

---

## Step 5 - Attach Lambda Integration

Select the GET route.

Click

```
Attach Integration
```

Choose

```
AWS Lambda
```

Select the Lambda function.

Click

```
Create
```

---

## Step 6 - Test the API

Navigate to

```
Stages
```

Copy the

```
Invoke URL
```

Example

```
https://abcd123.execute-api.us-east-1.amazonaws.com/users
```

Open it in

- Browser
- Postman
- curl

For POST, PUT, PATCH, and DELETE requests, use **Postman**.

---

# 🔵 HTTP API vs REST API

| Feature            | HTTP API                | REST API        |
| ------------------ | ----------------------- | --------------- |
| Cost               | Lower                   | Higher          |
| Performance        | Faster                  | Slightly Slower |
| Latency            | Lower                   | Higher          |
| API Keys           | No                      | Yes             |
| Usage Plans        | No                      | Yes             |
| Request Validation | No                      | Yes             |
| AWS WAF            | Limited                 | Yes             |
| Best For           | Serverless Applications | Enterprise APIs |

---

# 🔵 Endpoint Types (REST API)

REST APIs support three endpoint types.

| Endpoint Type  | Description                                            |
| -------------- | ------------------------------------------------------ |
| Edge Optimized | Best for globally distributed users using CloudFront   |
| Regional       | Best for clients within the same AWS Region            |
| Private        | Accessible only within a VPC using Interface Endpoints |

---

# 🔵 API Deployment Stages

A **Stage** represents a deployed version of an API.

Common stages include:

- Development (dev)
- Testing (test)
- Staging
- Production (prod)

Each stage has its own Invoke URL and configuration, allowing multiple environments to use the same API definition.

---

# 🔵 Authorization

API Gateway supports multiple authentication and authorization mechanisms.

- IAM Authentication
- Amazon Cognito User Pools
- JWT Authorizers
- Lambda Authorizers
- API Keys (REST API)

These mechanisms help secure APIs and restrict access to authorized users and applications.

---

# 🔵 Monitoring with CloudWatch

API Gateway integrates with Amazon CloudWatch for monitoring and logging.

CloudWatch provides:

- Request count
- Error rate
- Latency
- Access logs
- Execution logs
- API metrics
- Alarms

These metrics help monitor API performance and troubleshoot issues.

---

# 🔵 Proxy vs Non-Proxy Integration

## Proxy Integration

API Gateway forwards the complete HTTP request to Lambda, including headers, path parameters, query parameters, and request body.

Advantages

- Simpler configuration
- Flexible request handling
- Recommended for modern serverless applications

---

## Non-Proxy Integration

API Gateway transforms requests before forwarding them to Lambda using mapping templates.

Advantages

- Greater control over request and response formats
- Suitable for enterprise integrations
- Supports request transformation

---

# 🔵 Demo - Non-Proxy Integration (REST API)

## Step 1 - Create Lambda Function

Create a Lambda function.

Runtime

```
Python 3.10
```

Replace the handler with

```python
import json

def lambda_handler(event, context):

    name = event['queryStringParameters']['name']

    return {
        "statusCode":200,
        "body":f"Hello, {name}!"
    }
```

Deploy the function.

---

## Step 2 - Test Lambda

Create a test event.

```json
{
  "queryStringParameters": {
    "name": "Virat Kohli"
  }
}
```

Click

```
Test
```

Expected Output

```
Hello, Virat Kohli!
```

---

## Step 3 - Create REST API

Navigate to

```
Amazon API Gateway
```

Choose

```
REST API
```

Click

```
Create API
```

Provide an API name.

---

## Step 4 - Create Resource

Navigate to

```
Resources
```

Click

```
Create Resource
```

Example

```
/greeting
```

---

## Step 5 - Create Method

Select the resource.

Click

```
Create Method
```

Configure

```
Method

POST

Integration Type

Lambda Function

Lambda Function

greeting-api
```

Click

```
Create Method
```

You can now configure request mapping templates, response mapping templates, deploy the API, and test it using Postman.

---

# 🔵 Pricing

Amazon API Gateway follows a **pay-as-you-go** pricing model.

Charges are based on:

- Number of API requests
- Data transferred
- Caching (REST API)
- WebSocket connection minutes and messages

There are no upfront costs, and you pay only for the resources you use.
