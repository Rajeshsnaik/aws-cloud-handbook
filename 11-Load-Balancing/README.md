# Load Balancing

![AWS](./load-balancer.png)

---

# AWS Load Balancer

## 🔵 What is a Load Balancer?

A **Load Balancer** is a service that automatically distributes incoming network traffic across multiple backend servers or resources.

Instead of sending all requests to a single server, it spreads the traffic among multiple healthy servers, ensuring better performance, scalability, and availability.

Without a load balancer, a single server can become overloaded, causing slow responses or application downtime.

---

## 🔵 Why Do We Use a Load Balancer?

We use a Load Balancer to:

- Distribute incoming traffic across multiple servers
- Improve application availability and reliability
- Prevent a single server from becoming overloaded
- Automatically route traffic only to healthy servers
- Improve application performance
- Support Auto Scaling by routing traffic to newly launched instances
- Eliminate a single point of failure
- Increase fault tolerance across multiple Availability Zones
- Enable high availability (HA)
- Simplify traffic management

---

## 🔵 Benefits of Using a Load Balancer

- High Availability (HA)
- Better Performance
- Automatic Failover
- Health Checks
- Scalability
- Fault Tolerance
- Secure Traffic Handling (SSL/TLS)
- Easy Integration with Auto Scaling
- Supports Multiple Backend Services

---

## 🔵 Types of Load Balancers in AWS

AWS provides three types of Elastic Load Balancers (ELB):

| Load Balancer                       | OSI Layer         | Protocols           | Best For                                                      |
| ----------------------------------- | ----------------- | ------------------- | ------------------------------------------------------------- |
| **Application Load Balancer (ALB)** | Layer 7           | HTTP, HTTPS         | Web applications, REST APIs, Microservices                    |
| **Network Load Balancer (NLB)**     | Layer 4           | TCP, UDP, TLS       | High-performance applications, Gaming, IoT, Financial systems |
| **Gateway Load Balancer (GWLB)**    | Layer 3 + Layer 4 | IP Traffic (GENEVE) | Firewalls, IDS/IPS, Network Security Appliances               |

---

## 🔵 AWS Elastic Load Balancing (ELB)

**Elastic Load Balancing (ELB)** is the AWS service that automatically distributes incoming application or network traffic across multiple targets such as:

- Amazon EC2 Instances
- Containers (Amazon ECS/EKS)
- AWS Lambda Functions
- IP Addresses

ELB continuously monitors the health of registered targets and sends traffic only to healthy resources.

AWS Elastic Load Balancing includes:

- Application Load Balancer (ALB)
- Network Load Balancer (NLB)
- Gateway Load Balancer (GWLB)

---

# AWS Application Load Balancer (ALB)

## 🔵 What is ALB?

An **Application Load Balancer (ALB)** is an AWS managed service that distributes **HTTP/HTTPS** traffic across multiple application servers.

It works at the **Application Layer (Layer 7)** of the OSI model.

ALB understands the content of the request, such as:

- URL path
- Domain name
- HTTP headers

This allows it to send requests to different backend applications based on routing rules.

---

## 🔵 Real-World Example

Suppose you have one domain:

```text
www.example.com
```

But multiple applications:

```text
www.example.com/shop

↓

Shopping App
```

```text
www.example.com/admin

↓

Admin App
```

```text
www.example.com/api

↓

API Server
```

Instead of using multiple load balancers, **one ALB** can route requests to the correct application based on the incoming request.

---

## 🔵 Architecture

```text
              Users
                 │
                 ▼
      Application Load Balancer
        ┌────────┼────────┐
        │        │        │
      /shop    /api    /admin
        │        │        │
        ▼        ▼        ▼
    Shop App   API App  Admin App
```

---

## 🔵 Features

- Supports HTTP and HTTPS
- Works at Layer 7 (Application Layer)
- Path-based routing
- Host-based routing
- HTTP header-based routing
- Query string-based routing
- Redirect and fixed response support
- SSL/TLS termination
- Health checks
- Integration with Auto Scaling
- Supports EC2, ECS, EKS, Lambda, and IP targets
- Sticky sessions (Session Affinity)
- WebSocket support
- HTTP/2 support
- AWS WAF integration
- Access logs support
- CloudWatch metrics and monitoring

---

# Demo: Create an Application Load Balancer

## 🔵 Step 1: Launch Two EC2 Instances

Create two EC2 instances in the same VPC.

Example:

```text
Web-Server-1
```

```text
Web-Server-2
```

Install your application on both servers.

Both instances should be:

- Running
- In the same VPC
- In different Availability Zones (recommended)
- Allow HTTP (Port 80) in the Security Group

---

## 🔵 Step 2: Create a Target Group

Go to:

```text
EC2

↓

Target Groups

↓

Create Target Group
```

Choose:

```text
Target Type

↓

Instances
```

Configure:

```text
Protocol:
HTTP

Port:
80
```

Choose the correct VPC.

Register both EC2 instances.

Click **Create Target Group**.

---

## 🔵 Step 3: Create the ALB

Go to:

```text
EC2

↓

Load Balancers

↓

Create Load Balancer

↓

Application Load Balancer
```

Configure:

```text
Name:
My-ALB

Scheme:
Internet-facing

IP Type:
IPv4
```

Select **at least two public subnets** from different Availability Zones.

This provides high availability if one Availability Zone fails.

---

## 🔵 Step 4: Configure Listener

Create a Listener:

```text
Protocol:
HTTP

Port:
80
```

Default Action:

```text
Forward

↓

Target Group
```

Select the Target Group created earlier.

---

## 🔵 Step 5: Configure Security Group

Allow the following inbound rules:

```text
HTTP (80)

HTTPS (443)
```

Ensure the EC2 Security Group also allows traffic from the ALB.

---

## 🔵 Step 6: Create the Load Balancer

Review all settings and click **Create Load Balancer**.

AWS creates a DNS name similar to:

```text
my-alb-123456.ap-south-1.elb.amazonaws.com
```

Open the DNS name in your browser.

Refresh multiple times.

Traffic is automatically distributed between both EC2 instances.

---

## 🔵 Step 7: Test Health Checks

Stop one EC2 instance.

The ALB health check detects the unhealthy instance and removes it from the Target Group.

All new requests are automatically forwarded to the healthy EC2 instance without any manual intervention.

Restart the stopped instance.

After it passes the health check, ALB automatically starts sending traffic to it again.

---

## 🔵 When to Use ALB

Use ALB for:

- Websites
- REST APIs
- Microservices
- Container applications (ECS/EKS)
- Applications requiring URL-based routing
- Applications requiring Host-based routing
- Serverless applications using Lambda
- Applications needing SSL termination

---

# AWS Network Load Balancer (NLB)

## 🔵 What is NLB?

A **Network Load Balancer (NLB)** is an AWS managed service that distributes **TCP, UDP, and TLS** traffic across multiple backend servers.

It works at the **Transport Layer (Layer 4)** of the OSI model and forwards connections based only on **IP addresses and port numbers**. Unlike an Application Load Balancer (ALB), it does **not inspect HTTP requests** or understand URL paths, host names, or HTTP headers.

NLB is designed for applications that require:

- Very high performance
- Ultra-low latency
- Millions of concurrent connections
- High throughput
- Static IP addresses

---

## 🔵 Architecture

```text
          Users
             │
             ▼
   Network Load Balancer
        │           │
        ▼           ▼
     EC2-1       EC2-2
```

---

## 🔵 Features

- Supports TCP, UDP, and TLS protocols
- Works at Layer 4 (Transport Layer)
- Static IP addresses (Elastic IP support)
- Preserves client source IP address
- Extremely low latency
- High throughput
- Handles millions of requests per second
- Automatic health checks
- Integration with Auto Scaling
- Supports EC2, IP addresses, and Application Load Balancers as targets
- Cross-Zone Load Balancing (optional)
- Highly available across multiple Availability Zones
- CloudWatch monitoring and metrics
- TLS termination support

---

# Demo: Create a Network Load Balancer

## 🔵 Step 1: Launch Backend Servers

Create two EC2 instances running a TCP application.

Example applications:

- Game Server
- Custom TCP Service
- Socket Application
- Database Proxy

Ensure both instances:

- Are running
- Are in the same VPC
- Allow the application port (for example, **8080**) in the Security Group

---

## 🔵 Step 2: Create a Target Group

Go to:

```text
EC2

↓

Target Groups

↓

Create Target Group
```

Choose:

```text
Target Type:
Instances
```

Configure:

```text
Protocol:
TCP

Port:
8080
```

Select the correct VPC.

Register both EC2 instances.

Click **Create Target Group**.

---

## 🔵 Step 3: Create the Network Load Balancer

Go to:

```text
EC2

↓

Load Balancers

↓

Create Load Balancer

↓

Network Load Balancer
```

Configure:

```text
Name:
My-NLB

Scheme:
Internet-facing

IP Type:
IPv4
```

Select the required Availability Zones.

Optionally assign **Elastic IP addresses** if you need fixed public IPs.

---

## 🔵 Step 4: Create a Listener

Configure the Listener:

```text
Protocol:
TCP

Port:
8080
```

Default Action:

```text
Forward

↓

Target Group
```

Select the TCP Target Group created earlier.

---

## 🔵 Step 5: Configure Security Group

Although the Network Load Balancer itself does not use a Security Group, ensure that the backend EC2 instances allow incoming traffic on the application port.

Example:

```text
TCP

Port:
8080
```

---

## 🔵 Step 6: Create the Network Load Balancer

Review the configuration and click **Create Load Balancer**.

AWS generates a DNS name similar to:

```text
my-nlb-123456.ap-south-1.elb.amazonaws.com
```

Use this DNS name to connect your TCP or UDP application.

Traffic is automatically distributed between the healthy backend servers.

---

## 🔵 Step 7: Test Health Checks

Stop one EC2 instance.

The Network Load Balancer health check detects that the instance is unhealthy and removes it from the Target Group.

All new connections are automatically forwarded to the healthy EC2 instance.

Restart the stopped instance.

Once it passes the health check, it automatically starts receiving traffic again.

---

## 🔵 When to Use NLB

Use NLB for:

- Gaming servers
- TCP applications
- UDP applications
- Financial trading systems
- IoT platforms
- Voice-over-IP (VoIP) applications
- Database proxy services
- Applications requiring static public IP addresses
- Applications requiring ultra-low latency
- Applications handling millions of concurrent connections

---

# AWS Gateway Load Balancer (GWLB)

## 🔵 What is GWLB?

A **Gateway Load Balancer (GWLB)** is an AWS managed service that distributes **network traffic to virtual security appliances** such as firewalls, intrusion detection systems (IDS), and intrusion prevention systems (IPS).

Unlike **Application Load Balancer (ALB)** and **Network Load Balancer (NLB)**, a Gateway Load Balancer is **not used to load balance web applications or backend servers**.

Its primary purpose is to **inspect, analyze, and secure network traffic** before it reaches your application servers.

GWLB operates transparently by forwarding all traffic through security appliances while maintaining high availability and scalability.

---

## 🔵 Architecture

```text
                 Internet
                     │
                     ▼
        Gateway Load Balancer
                     │
                     ▼
        Firewall / IDS / IPS
                     │
                     ▼
          Application Servers
```

Every network packet passes through the security appliance before reaching the application.

---

## 🔵 Features

- Centralized traffic inspection
- Automatic scaling of security appliances
- High availability across multiple Availability Zones
- Automatic health checks
- Transparent traffic forwarding
- Supports third-party virtual firewalls
- Supports IDS and IPS appliances
- Works with Gateway Load Balancer Endpoints (GWLBE)
- GENEVE protocol (Port 6081)
- CloudWatch monitoring and metrics
- Highly scalable and fault tolerant

---

# Demo: Create a Gateway Load Balancer

## 🔵 Step 1: Deploy Security Appliances

Launch one or more virtual security appliances from **AWS Marketplace**.

Examples:

- Palo Alto Firewall
- Fortinet FortiGate
- Check Point Firewall
- Cisco Secure Firewall

Deploy the appliances into your VPC.

Ensure they are properly configured for traffic inspection.

---

## 🔵 Step 2: Create a Target Group

Go to:

```text
EC2

↓

Target Groups

↓

Create Target Group
```

Choose:

```text
Target Type:
Instances
```

Select the firewall appliance EC2 instances as targets.

Register all security appliance instances.

Click **Create Target Group**.

---

## 🔵 Step 3: Create the Gateway Load Balancer

Go to:

```text
EC2

↓

Load Balancers

↓

Create Load Balancer

↓

Gateway Load Balancer
```

Configure:

```text
Name:
My-GWLB
```

Choose the appropriate VPC.

Attach the Target Group containing the firewall appliances.

Click **Create Load Balancer**.

---

## 🔵 Step 4: Create a Gateway Load Balancer Endpoint (GWLBE)

Go to:

```text
VPC

↓

Endpoints

↓

Create Endpoint
```

Choose:

```text
Gateway Load Balancer Endpoint
```

Select:

- Your Gateway Load Balancer
- The VPC where your applications are running
- The required subnet

Create the endpoint.

---

## 🔵 Step 5: Update Route Tables

Modify the VPC Route Tables so that application traffic is routed through the Gateway Load Balancer Endpoint before reaching the destination.

Example traffic flow:

```text
User

↓

Gateway Load Balancer Endpoint

↓

Gateway Load Balancer

↓

Firewall Appliance

↓

Application Server
```

Now every packet is inspected before reaching the application.

---

## 🔵 Step 6: Test

Generate traffic to your application.

Verify that the firewall appliance receives and inspects the traffic.

If one firewall appliance becomes unhealthy or fails, the Gateway Load Balancer automatically forwards traffic to another healthy appliance without interrupting the application.

---

## 🔵 When to Use GWLB

Use GWLB when you need:

- Centralized firewalls
- IDS (Intrusion Detection Systems)
- IPS (Intrusion Prevention Systems)
- Deep packet inspection
- Network traffic monitoring
- Third-party virtual security appliances
- Scalable network security architecture
- Security inspection across multiple VPCs

---

## 🔵 Interview Question

### Q. Why do we use ALB instead of directly accessing an EC2 instance?

**Answer:**

ALB distributes traffic across multiple healthy servers instead of sending all requests to a single EC2 instance. It performs automatic health checks, integrates with Auto Scaling, provides SSL/TLS termination, and supports intelligent routing based on HTTP/HTTPS requests such as URL paths, host names, headers, and query strings. This improves application availability, scalability, performance, and reliability.

### Q. Why choose NLB instead of ALB?

**Answer:**

Choose a **Network Load Balancer (NLB)** when your application uses **TCP, UDP, or TLS** protocols instead of HTTP/HTTPS. NLB is ideal for applications that require **static IP addresses**, **preservation of the client source IP**, **extremely low latency**, **high throughput**, and the ability to handle **millions of concurrent connections**. Unlike ALB, it operates at **Layer 4** and forwards traffic based only on IP addresses and port numbers.

### Q. Can GWLB be used to distribute HTTP requests like ALB?

**Answer:**

No. **Gateway Load Balancer (GWLB)** is designed to route network traffic through **security appliances** such as firewalls, IDS, and IPS for inspection and protection. It does **not perform application-level routing** like ALB. If you need to distribute **HTTP/HTTPS** requests based on URLs, host names, or headers, you should use an **Application Load Balancer (ALB)** instead.
