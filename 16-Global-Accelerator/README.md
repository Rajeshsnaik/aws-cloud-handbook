# 🔵 AWS Global Accelerator

![Accelarator](./Global-accelerator.png)

**AWS Global Accelerator** is a **networking service** that improves the **availability, performance, and reliability** of your applications by routing user traffic through the **AWS Global Network** instead of the public internet.

It provides users with **two static Anycast IP addresses**, allowing clients to connect to the nearest AWS Edge Location. From there, traffic travels over AWS's private global backbone to the healthiest application endpoint.

Unlike a CDN, Global Accelerator **does not cache content**. Instead, it optimizes **network traffic routing** to reduce latency and improve application availability worldwide.

---

# 🔵 Why Use AWS Global Accelerator?

Applications are often accessed by users from different countries and continents.

Without an optimized global network, requests travel through the unpredictable public internet, which can result in:

- Higher latency
- Network congestion
- Packet loss
- Slower response times
- Poor user experience

AWS Global Accelerator solves these problems by routing traffic through the **AWS Global Network**, providing faster and more reliable connectivity.

---

# 🔵 Real-World Example

Imagine your application is deployed in two AWS Regions:

- 🇮🇳 Mumbai (`ap-south-1`)
- 🇺🇸 Virginia (`us-east-1`)

### Without Global Accelerator

```text
User (Germany)
       │
 Public Internet
       │
       ▼
    Mumbai
```

The request travels entirely over the public internet.

Problems:

- Higher latency
- Network congestion
- Packet loss
- Slower application performance

---

### With Global Accelerator

```text
User (Germany)
       │
       ▼
Nearest AWS Edge Location
       │
       ▼
AWS Global Backbone Network
       │
       ▼
Best Healthy AWS Region
```

Instead of relying on the public internet, AWS routes traffic over its private global backbone.

Result:

- Lower latency
- Better performance
- Automatic failover
- Higher availability

---

# 🔵 Problem Without Global Accelerator

Without Global Accelerator:

```text
Users Worldwide
        │
        ▼
 Public Internet
        │
        ▼
Application Endpoint
```

Challenges include:

- Traffic uses the public internet.
- Network routes constantly change.
- Higher latency.
- Network congestion.
- Packet loss.
- No automatic regional failover.
- IP addresses may change if infrastructure changes.
- User experience varies depending on internet conditions.

---

# 🔵 How Global Accelerator Solves the Problem

With Global Accelerator:

```text
Users Worldwide
        │
        ▼
Nearest AWS Edge Location
        │
        ▼
AWS Global Backbone Network
        │
        ▼
Healthy Application Endpoint
```

Benefits:

- Uses the AWS Global Network.
- Lower latency.
- Stable network routing.
- Static Anycast IP addresses.
- Continuous health monitoring.
- Automatic failover.
- Improved global user experience.

---

# 🔵 How AWS Global Accelerator Works

```text
             Users Worldwide
                     │
                     ▼
        AWS Edge Locations (Anycast IP)
                     │
                     ▼
        AWS Global Backbone Network
                     │
                     ▼
          AWS Global Accelerator
                     │
        ┌────────────┴─────────────┐
        │                          │
        ▼                          ▼
   Region 1                   Region 2
  (Mumbai)                  (Virginia)
        │                          │
        ▼                          ▼
   Application               Application
```

### Request Flow

1. Users connect using a Static Anycast IP address.
2. The request reaches the nearest AWS Edge Location.
3. Traffic enters the AWS Global Backbone Network.
4. Global Accelerator checks endpoint health.
5. Traffic is routed to the nearest healthy endpoint.
6. The application returns the response.

Since most of the journey happens over AWS's private global network, latency is significantly reduced.

---

# 🔵 Components of AWS Global Accelerator

Global Accelerator consists of four main components:

- Accelerator
- Listener
- Endpoint Group
- Endpoint

---

# 🔵 1. Accelerator

The **Accelerator** is the primary resource in AWS Global Accelerator.

It provides:

- Two Static Anycast IP addresses
- A DNS name
- Global traffic routing

Example:

```text
75.2.xxx.xxx
99.83.xxx.xxx
```

Clients always connect using these IP addresses or the provided DNS name.

Benefits:

- Static public IPs
- Global entry point
- Simplified application access

---

# 🔵 2. Listener

A **Listener** defines how Global Accelerator accepts incoming traffic.

It specifies:

- Protocol
- Port

Example:

```text
Protocol:
TCP

Port:
80

Port:
443
```

The listener receives incoming requests and forwards them to the appropriate Endpoint Group.

---

# 🔵 3. Endpoint Group

An **Endpoint Group** represents an AWS Region.

Example:

```text
Mumbai

Virginia

Singapore
```

Each Endpoint Group can be configured with:

- Traffic Dial
- Health Checks
- Endpoint Weight

Endpoint Groups determine which AWS Region receives traffic.

---

# 🔵 4. Endpoint

An **Endpoint** is the actual AWS resource that serves application traffic.

Supported endpoint types include:

- EC2 Instance
- Application Load Balancer (ALB)
- Network Load Balancer (NLB)
- Elastic IP Address

Example:

```text
Endpoint Group
       │
       ├── Application Load Balancer
       ├── EC2 Instance
       └── Network Load Balancer
```

Global Accelerator forwards traffic to these endpoints after determining the healthiest destination.

---

# 🔵 Traffic Flow

The complete traffic flow looks like this:

```text
User
        │
        ▼
Static Anycast IP
        │
        ▼
Nearest AWS Edge Location
        │
        ▼
AWS Global Backbone Network
        │
        ▼
Healthy Endpoint Group
        │
        ▼
Application Endpoint
```

This routing minimizes latency while maximizing application availability.

---

# 🔵 Health Checks

Global Accelerator continuously performs health checks on all configured endpoints.

Example:

```text
Mumbai = Healthy

Virginia = Healthy
```

Traffic is routed according to the configured traffic distribution.

If one Region becomes unavailable:

```text
Mumbai = Down
```

Traffic automatically shifts to:

```text
Virginia
```

Benefits:

- Automatic failover
- High availability
- No manual intervention
- No DNS propagation delay

---

# 🔵 Static IP Addresses

Unlike CloudFront, AWS Global Accelerator provides **two Static Anycast IP addresses**.

Example:

```text
75.xx.xx.xx

99.xx.xx.xx
```

These IP addresses never change, even if your backend infrastructure changes.

Common use cases:

- Firewall allowlists
- Banking applications
- Enterprise applications
- Third-party integrations
- Partners who whitelist your IP address

---

# 🔵 Traffic Dial

**Traffic Dial** controls how much traffic is routed to a specific AWS Region.

Example:

```text
Mumbai

Traffic Dial = 100%
```

All available traffic can be routed to Mumbai.

Another example:

```text
Virginia

Traffic Dial = 50%
```

Only half of the potential traffic is directed to Virginia.

Traffic Dial is useful for:

- Gradual deployments
- Disaster recovery
- Blue/Green deployments
- Regional maintenance
- Testing

---

# 🔵 Endpoint Weights

If multiple endpoints exist within the same Region, **Endpoint Weights** determine how traffic is distributed among them.

Example:

```text
ALB-1 Weight = 100

ALB-2 Weight = 50
```

Traffic distribution becomes approximately:

```text
ALB-1 → 67%

ALB-2 → 33%
```

Benefits:

- Load distribution
- Controlled traffic allocation
- Canary deployments
- Performance testing
- Easy migration between resources

`````

---

````markdown
# 🔵 Supported Endpoints

AWS Global Accelerator supports several AWS resources as application endpoints.

Supported endpoint types include:

- Amazon EC2 Instance
- Elastic IP Address
- Application Load Balancer (ALB)
- Network Load Balancer (NLB)

Example:

```text
Endpoint Group
       │
       ├── Application Load Balancer
       ├── EC2 Instance
       ├── Network Load Balancer
       └── Elastic IP
```

Global Accelerator continuously monitors these endpoints using health checks and routes traffic only to healthy endpoints.

---

# 🔵 Architecture Diagram

A typical Global Accelerator architecture looks like this:

```text
                  Users Worldwide
                         │
                         ▼
              Static Anycast IP Address
                         │
                         ▼
              AWS Edge Location (Nearest)
                         │
                         ▼
           AWS Global Backbone Network
                         │
                         ▼
              AWS Global Accelerator
                         │
          ┌──────────────┴──────────────┐
          │                             │
          ▼                             ▼
 Endpoint Group (Mumbai)      Endpoint Group (Virginia)
          │                             │
     ┌────┴────┐                  ┌─────┴─────┐
     ▼         ▼                  ▼           ▼
    ALB      EC2 Instance       ALB      EC2 Instance
```

Users always connect through the nearest AWS Edge Location, while AWS routes traffic over its private global network to the healthiest endpoint.

---

# 🔵 Common Use Cases

AWS Global Accelerator is designed for applications that require low latency, high availability, and reliable global connectivity.

---

# 🔵 Multi-Region Applications

Applications deployed in multiple AWS Regions can automatically serve users from the nearest healthy region.

Example:

```text
Users Worldwide
        │
        ▼
Global Accelerator
        │
   ┌────┴────┐
   ▼         ▼
Mumbai    Virginia
```

Benefits:

- Lower latency
- Better user experience
- High availability
- Automatic regional routing

---

# 🔵 Disaster Recovery

If one AWS Region becomes unavailable, Global Accelerator automatically redirects traffic to another healthy Region.

Example:

```text
Before Failure

Users
   │
   ▼
Mumbai

After Failure

Users
   │
   ▼
Virginia
```

Benefits:

- Automatic failover
- Minimal downtime
- No DNS propagation delay
- High availability

---

# 🔵 Gaming Applications

Online multiplayer games require extremely low latency.

Global Accelerator routes players through the AWS global backbone network instead of the public internet.

Benefits:

- Lower latency
- Stable connections
- Reduced packet loss
- Better gaming experience

---

# 🔵 Financial Applications

Banking and financial systems require:

- High availability
- Low latency
- Static IP addresses
- Reliable connectivity

Global Accelerator provides all of these while automatically handling regional failures.

---

# 🔵 Global APIs

Applications exposing APIs worldwide can use Global Accelerator to improve request routing.

Instead of relying entirely on the public internet, requests travel over AWS's optimized backbone.

Benefits:

- Faster API responses
- Lower latency
- Improved availability
- Better user experience

---

# 🔵 CloudFront vs Global Accelerator

This is one of the most common AWS interview questions.

Although both services improve application performance for global users, they solve different problems.

CloudFront is a **Content Delivery Network (CDN)** that improves performance by caching content closer to users.

Global Accelerator is a **Network Accelerator** that improves performance by routing traffic over the AWS global backbone without caching.

---

# 🔵 CloudFront Architecture

```text
User
      │
      ▼
CloudFront Edge Location
      │
      ├── Cache Hit
      │       │
      │       ▼
      │   Cached Content
      │
      └── Cache Miss
              │
              ▼
      Origin Server
```

CloudFront caches frequently accessed content at Edge Locations.

---

# 🔵 Global Accelerator Architecture

```text
User
      │
      ▼
Static Anycast IP
      │
      ▼
Nearest AWS Edge Location
      │
      ▼
AWS Global Backbone Network
      │
      ▼
Healthy Application Endpoint
```

Global Accelerator never caches content.

Instead, it finds the best network path to the healthiest endpoint.

---

# 🔵 CloudFront vs Global Accelerator Comparison

| Feature                     | CloudFront                               | Global Accelerator                               |
| --------------------------- | ---------------------------------------- | ------------------------------------------------ |
| Service Type                | CDN                                      | Network Accelerator                              |
| Purpose                     | Cache Content                            | Accelerate Network Traffic                       |
| Uses Cache                  | Yes                                      | No                                               |
| Static Content              | Excellent                                | No                                               |
| Dynamic Content             | Good                                     | Excellent                                        |
| Static IP                   | No                                       | Yes                                              |
| Anycast IP                  | No                                       | Yes                                              |
| Uses AWS Backbone           | Partially                                | Yes                                              |
| Supports TCP & UDP          | No                                       | Yes                                              |
| Automatic Regional Failover | Limited                                  | Excellent                                        |
| Best For                    | Images, CSS, JS, Videos, Static Websites | APIs, Gaming, Banking, Multi-Region Applications |

---

# 🔵 CloudFront vs Global Accelerator Explained

## Purpose

### CloudFront

Designed to deliver static content faster by caching it at AWS Edge Locations.

Examples:

- Images
- CSS
- JavaScript
- Videos
- PDF files

---

### Global Accelerator

Designed to improve network performance by routing requests through the AWS global backbone.

Examples:

- APIs
- Gaming
- Banking applications
- Multi-region applications
- Real-time systems

---

## Caching

### CloudFront

Stores frequently accessed content in Edge Locations.

The next request is served directly from the cache.

Result:

- Faster page loads
- Reduced origin load

---

### Global Accelerator

Does not cache any content.

Every request reaches the backend application using the fastest available network path.

---

## Static IP Addresses

### CloudFront

Does not provide static IP addresses.

Users access the CloudFront distribution using its domain name.

---

### Global Accelerator

Provides two Static Anycast IP addresses.

These IPs remain the same even if backend infrastructure changes.

---

## Dynamic Applications

### CloudFront

Can improve performance for dynamic applications but is mainly optimized for static content.

---

### Global Accelerator

Designed specifically for dynamic applications requiring low latency.

Examples:

- APIs
- Gaming
- Financial services

---

## Automatic Failover

### CloudFront

Provides limited failover capabilities.

---

### Global Accelerator

Continuously monitors endpoint health.

If one Region becomes unavailable, traffic automatically shifts to another healthy Region without waiting for DNS propagation.

---

# 🔵 Global Accelerator vs Route 53

Although both services help direct user traffic, they operate differently.

Route 53 is a **DNS service**.

Global Accelerator is a **network acceleration service**.

---

# 🔵 Route 53 Architecture

```text
User
     │
DNS Query
     │
     ▼
Route 53
     │
     ▼
Application Endpoint
```

Route 53 responds with the IP address of the application.

After DNS resolution, traffic travels over the public internet.

---

# 🔵 Global Accelerator Architecture

```text
User
      │
      ▼
Static Anycast IP
      │
      ▼
Nearest AWS Edge Location
      │
      ▼
AWS Global Backbone Network
      │
      ▼
Application Endpoint
```

Traffic enters the AWS global network immediately after reaching the nearest Edge Location.

---

# 🔵 Global Accelerator vs Route 53 Comparison

| Feature           | Route 53           | Global Accelerator              |
| ----------------- | ------------------ | ------------------------------- |
| Service Type      | DNS Service        | Network Accelerator             |
| Routing Method    | DNS-Based          | Network-Based                   |
| Static IP Address | No                 | Yes                             |
| Health Checks     | Yes                | Yes                             |
| Traffic Routing   | DNS                | AWS Global Network              |
| Failover Time     | Depends on DNS TTL | Almost Instant                  |
| Uses AWS Backbone | No                 | Yes                             |
| Best For          | DNS Management     | Low-Latency Global Applications |

---

# 🔵 Global Accelerator vs Route 53 Explained

## Route 53

Route 53 determines **where** users should connect.

It resolves a domain name into an IP address.

After DNS resolution, traffic travels across the public internet.

Best suited for:

- Domain registration
- DNS management
- DNS routing policies
- Health checks

---

## Global Accelerator

Global Accelerator determines **how** traffic reaches the application.

Instead of relying on internet routing, traffic travels across the AWS global backbone.

Best suited for:

- Multi-region applications
- APIs
- Gaming
- Banking
- Low-latency applications
- Disaster recovery

---

# 🔵 Summary

CloudFront, Route 53, and Global Accelerator each serve different purposes.

- **CloudFront** accelerates content delivery by caching content closer to users.
- **Route 53** resolves domain names and performs DNS-based traffic routing.
- **Global Accelerator** improves network performance by routing traffic over the AWS global backbone using Static Anycast IP addresses.

In many production architectures, these services are used together to deliver fast, reliable, and highly available applications worldwide.
`````

---

# 🔵 Create AWS Global Accelerator

To create a Global Accelerator, navigate to:

```text
AWS Console → Global Accelerator
```

Click:

```text
Create Accelerator
```

---

# 🔵 Step 1: Configure Accelerator

Provide the basic accelerator details.

Example:

```text
Name:
MyGlobalAccelerator
```

Choose the IP address type.

Example:

```text
IPv4
```

(Optional)

Enable **Flow Logs** if you want to capture traffic logs for monitoring and troubleshooting.

Click:

```text
Next
```

---

# 🔵 Step 2: Create a Listener

A **Listener** defines how Global Accelerator accepts incoming traffic.

Configure:

```text
Listener Name:
HTTP-Listener

Protocol:
TCP

Port:
80
```

For HTTPS applications:

```text
Protocol:
TCP

Port:
443
```

Multiple ports can be added if required.

Click:

```text
Next
```

---

# 🔵 Step 3: Add an Endpoint Group

Choose the AWS Region where your application is deployed.

Example:

```text
ap-south-1 (Mumbai)
```

Configure the endpoint group settings.

Example:

```text
Traffic Dial:
100%

Health Check Protocol:
HTTP

Health Check Port:
80

Health Check Path:
/health
```

These settings determine:

- How much traffic is sent to the Region.
- How endpoint health is monitored.

Click:

```text
Next
```

---

# 🔵 Step 4: Add an Endpoint

Select the backend resource that serves your application.

Supported endpoint types include:

- Application Load Balancer (Recommended)
- Network Load Balancer
- EC2 Instance
- Elastic IP Address

Example:

```text
Application Load Balancer
```

If multiple endpoints exist, configure an **Endpoint Weight** for each one.

Click:

```text
Create Accelerator
```

---

# 🔵 Step 5: Wait for Deployment

AWS provisions the Global Accelerator.

After a few minutes, the accelerator becomes active.

You'll receive:

```text
Static IP Address 1:
75.x.x.x

Static IP Address 2:
99.x.x.x

DNS Name:
abc123.awsglobalaccelerator.com
```

Clients can now connect using either the DNS name or the Static Anycast IP addresses.

---

# 🔵 Verify the Accelerator

Open the newly created accelerator.

Verify the following information:

- Accelerator Status
- Static IP Addresses
- DNS Name
- Listener Configuration
- Endpoint Groups
- Endpoints
- Health Status

Ensure that all configured endpoints show a **Healthy** status.

---

# 🔵 Testing the Accelerator

After deployment, test the application using the provided DNS name or Static IP addresses.

Example:

```text
https://abc123.awsglobalaccelerator.com
```

or

```text
http://75.x.x.x
```

The request should be routed through:

```text
User
      │
      ▼
Nearest AWS Edge Location
      │
      ▼
AWS Global Backbone
      │
      ▼
Healthy Application Endpoint
```

Verify that your application loads successfully.

---

# 🔵 Test Automatic Failover

If multiple AWS Regions are configured, Global Accelerator can automatically reroute traffic when one Region becomes unavailable.

Example:

```text
Before Failure

Mumbai  → Healthy

Virginia → Healthy
```

Traffic is routed according to your configuration.

If the Mumbai Region becomes unavailable:

```text
Mumbai

↓

Unhealthy
```

Traffic automatically shifts to:

```text
Virginia

↓

Healthy
```

No DNS propagation delay is required because clients continue using the same Static Anycast IP addresses.

---

# 🔵 Monitor Health Checks

Open the **Endpoint Groups** section.

Here you can monitor:

- Endpoint Health
- Health Check Status
- Traffic Distribution
- Regional Availability

Global Accelerator continuously performs health checks and updates routing decisions automatically.

---

# 🔵 Best Practices

- Use an **Application Load Balancer (ALB)** or **Network Load Balancer (NLB)** instead of individual EC2 instances whenever possible.
- Deploy applications across multiple AWS Regions for high availability.
- Configure lightweight `/health` endpoints for accurate health checks.
- Use **Traffic Dial** for gradual deployments and disaster recovery testing.
- Configure appropriate **Endpoint Weights** when multiple endpoints exist within the same Region.
- Monitor accelerator metrics using **Amazon CloudWatch**.
- Use Global Accelerator for latency-sensitive and globally distributed applications.
- Combine Global Accelerator with **Route 53** and **CloudFront** when appropriate for complete global architectures.

---

# 🔵 Interview Questions

## 1. What is AWS Global Accelerator?

AWS Global Accelerator is a networking service that improves application performance, availability, and reliability by routing traffic through the AWS global backbone network using Static Anycast IP addresses.

---

## 2. Does Global Accelerator cache content?

No.

Global Accelerator **does not cache content**.

It accelerates network traffic by routing requests over the AWS global backbone network.

---

## 3. What are the supported endpoint types?

Supported endpoints include:

- Amazon EC2 Instance
- Elastic IP Address
- Application Load Balancer (ALB)
- Network Load Balancer (NLB)

---

## 4. What is the difference between CloudFront and Global Accelerator?

CloudFront is a **Content Delivery Network (CDN)** that improves performance by caching content at Edge Locations.

Global Accelerator is a **Network Accelerator** that improves performance by routing traffic over the AWS global backbone without caching content.

---

## 5. Why does Global Accelerator provide two Static Anycast IP addresses?

The two Static Anycast IP addresses provide a consistent entry point for users.

Even if backend endpoints or AWS Regions change, clients continue using the same IP addresses.

This simplifies firewall allowlists and improves high availability.

---

## 6. What is an Endpoint Group?

An Endpoint Group represents an AWS Region.

It contains one or more application endpoints and includes settings such as:

- Traffic Dial
- Health Checks
- Endpoint Weights

---

## 7. What is the purpose of Traffic Dial?

Traffic Dial controls how much traffic is routed to a particular AWS Region.

It is commonly used for:

- Gradual deployments
- Disaster recovery
- Regional maintenance
- Testing

---

## 8. What happens if an AWS Region becomes unavailable?

Global Accelerator continuously monitors endpoint health.

If a Region becomes unhealthy, traffic is automatically redirected to another healthy Region without waiting for DNS propagation.

---

## 9. When should you use AWS Global Accelerator?

Use Global Accelerator for:

- Multi-Region applications
- APIs
- Gaming platforms
- Banking applications
- Financial systems
- Low-latency applications
- Disaster recovery
- Automatic regional failover

---

# 🔵 Summary

AWS Global Accelerator improves the performance, availability, and reliability of applications by routing traffic through the AWS Global Network instead of the public internet.

Unlike Amazon CloudFront, it **does not cache content**. Instead, it uses Static Anycast IP addresses, AWS Edge Locations, health checks, and intelligent traffic routing to deliver requests to the nearest healthy application endpoint.

Global Accelerator is an ideal choice for applications that require:

- Low latency
- High availability
- Automatic failover
- Static IP addresses
- Multi-Region deployments
- Reliable global application access
