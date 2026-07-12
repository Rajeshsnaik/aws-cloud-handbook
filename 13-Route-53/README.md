# Amazon Route 53

![Route53](./route53.png)

## 🔵 What is DNS?

**DNS (Domain Name System)** is the phonebook of the internet. Humans remember domain names like:

```text
www.google.com
www.amazon.com
www.github.com
```

But computers communicate using **IP addresses** such as:

```text
142.250.193.78
```

DNS translates a human-readable domain name into an IP address so browsers can connect to the correct server.

Without DNS, users would need to remember IP addresses for every website.

---

## 🔵 What is Amazon Route 53?

**Amazon Route 53** is a highly available and scalable **DNS (Domain Name System)** web service provided by AWS.

It helps route users to applications running on AWS or outside AWS by translating domain names into IP addresses.

Apart from DNS, Route 53 also provides:

- Domain Registration
- DNS Management
- Health Checks
- Traffic Routing
- High Availability
- Global Traffic Management

The name **Route 53** comes from **Port 53**, which is the standard port used for DNS services.

---

## 🔵 Why Do We Need Route 53?

Without Route 53 (or any DNS service), users would need to remember IP addresses instead of easy-to-read domain names.

For example:

Instead of typing:

```text
54.210.123.45
```

Users can simply type:

```text
www.mywebsite.com
```

Route 53 resolves the domain name and directs users to the correct server.

It also provides:

- Easy-to-remember domain names
- Global DNS service
- High availability
- Health monitoring
- Intelligent traffic routing
- Low latency for global users
- Automatic failover

---

## 🔵 How DNS Works

Suppose a user opens:

```text
www.mywebsite.com
```

The request follows this flow:

```text
User

↓

Browser

↓

DNS Resolver (ISP)

↓

Root DNS Server

↓

TLD Server (.com)

↓

Route 53 Hosted Zone

↓

Returns EC2 Public IP

↓

Browser connects to EC2
```

Finally, the application running on the EC2 instance is displayed in the browser.

---

## 🔵 How Route 53 Works

```text
User

↓

www.mywebsite.com

↓

Amazon Route 53

↓

Hosted Zone

↓

DNS Record (A Record)

↓

EC2 Public IP

↓

Application
```

Route 53 receives the DNS request, checks the Hosted Zone, finds the matching DNS record, returns the server's IP address, and the browser connects to the application.

---

## 🔵 Features

- Fully managed DNS service
- Domain registration
- Public and Private Hosted Zones
- Health checks
- DNS failover
- Multiple routing policies
- Low latency routing
- Geolocation routing
- Weighted routing
- Failover routing
- Multi-value answer routing
- Alias records
- Supports IPv4 and IPv6
- Highly available
- Global service
- CloudWatch integration
- Works with EC2, ELB, CloudFront, S3 and many AWS services

---

## 🔵 Common DNS Record Types

| Record Type | Purpose                                       |
| ----------- | --------------------------------------------- |
| A           | Maps a domain to an IPv4 address              |
| AAAA        | Maps a domain to an IPv6 address              |
| CNAME       | Maps one domain name to another               |
| NS          | Specifies the authoritative name servers      |
| SOA         | Contains information about the DNS zone       |
| MX          | Mail server record                            |
| TXT         | Stores text data (verification, SPF, etc.)    |
| Alias       | AWS-specific record pointing to AWS resources |

---

## 🔵 Routing Policies in Route 53

AWS Route 53 supports multiple routing policies:

- Simple Routing
- Weighted Routing
- Latency Routing
- Failover Routing
- Geolocation Routing
- Geoproximity Routing
- Multi-Value Answer Routing
- IP-based Routing

---

# Practical Demo 1 - Connect a Domain to an EC2 Instance

## 🔵 Before You Start

> **Important:** Amazon Route 53 is a **paid AWS service**. Domain registration and hosted zones may incur charges. If you are practicing, monitor your AWS billing and delete unused resources after completing the demo.

---

## 🔵 Step 1: Launch an EC2 Instance

Create an EC2 instance.

Install your application.

Example:

```bash
git clone <your-github-repository>

cd project-folder

npm install

npm start
```

Ensure:

- Application is running
- Security Group allows HTTP (Port 80)
- Note the **Public IPv4 Address**

Verify the application by opening:

```text
http://<EC2-Public-IP>
```

If the application opens successfully, proceed to Route 53.

---

## 🔵 Step 2: Open Amazon Route 53

Go to:

```text
AWS Console

↓

Route 53
```

Now there are two possible scenarios.

---

# Option 1 - Buy a Domain Using Route 53

## 🔵 Register a Domain

If you do not already own a domain name, you need to purchase one.

A domain cannot be purchased permanently; it is registered on a **yearly basis** (renewable).

Popular domain providers include:

- Amazon Route 53
- Hostinger
- GoDaddy
- Namecheap
- Google Domains (where available)

In Route 53:

```text
Route 53

↓

Registered Domains

↓

Register Domain
```

Search for your desired domain.

Example:

```text
myawesomewebsite.com
```

If the domain is available:

- Select it
- Continue to Checkout
- Complete the payment

After registration, AWS automatically creates a **Public Hosted Zone** for your domain.

---

# Option 2 - Use a Domain Purchased from Another Provider

If you already own a domain from providers like:

- Hostinger
- GoDaddy
- Namecheap

You need to create a Hosted Zone in Route 53.

Go to:

```text
Route 53

↓

Hosted Zones

↓

Create Hosted Zone
```

Provide:

```text
Domain Name:
yourdomain.com

Description:
(Optional)

Type:
Public Hosted Zone

Tags:
(Optional)
```

Click **Create Hosted Zone**.

---

## 🔵 Why Do We Create a Hosted Zone?

A Hosted Zone stores all DNS records for your domain.

Whenever someone opens your website:

```text
www.yourdomain.com
```

The DNS request first reaches **Amazon Route 53**.

Route 53 looks inside the Hosted Zone to determine where the traffic should be sent.

Without a Hosted Zone, Route 53 cannot answer DNS requests for your domain.

---

## 🔵 Update Name Servers (Only for External Domains)

If your domain was purchased outside AWS:

Open your domain provider dashboard.

Example:

```text
Hostinger

↓

Domain

↓

DNS / Nameservers

↓

Edit
```

Copy the **NS (Name Server)** records generated by Route 53 Hosted Zone.

Replace the existing Name Servers with the Route 53 Name Servers.

Save the changes.

DNS propagation can take several minutes to 48 hours.

---

## 🔵 Step 3: Create an A Record

Inside the Hosted Zone:

```text
Create Record
```

Configure:

```text
Record Name:
(Optional)

Example:
www

Record Type:
A

Value:
EC2 Public IPv4 Address

Routing Policy:
Simple Routing
```

Click:

```text
Create Record
```

The A Record maps your domain name to the EC2 Public IP address.

---

## 🔵 Step 4: Test the Domain

Open:

```text
http://yourdomain.com
```

or

```text
http://www.yourdomain.com
```

The website should open from your EC2 instance.

> **Note:** DNS propagation may take a few minutes (sometimes longer). Initially, use **HTTP** unless HTTPS has been configured with an SSL/TLS certificate.

---

# Practical Demo 2 - Latency-Based Routing

## 🔵 Problem Statement

Suppose your application is deployed only in the **Mumbai Region (ap-south-1)**.

Users from:

- India
- USA
- Europe
- Australia

All access the same EC2 instance located in Mumbai.

This means users who are far away experience **higher latency**, slower page loads, and increased response times.

Example:

```text
USA User

↓

Mumbai EC2

High Latency
```

---

## 🔵 Solution

Deploy identical copies of your application in multiple AWS Regions.

Example:

- Mumbai (ap-south-1)
- Singapore (ap-southeast-1)

Users will automatically be routed to the nearest AWS Region.

---

## 🔵 Architecture

```text
            Global Users
                  │
                  ▼
            Amazon Route 53
                  │
        Latency-Based Routing
          ┌────────┴────────┐
          ▼                 ▼
 Mumbai EC2          Singapore EC2
```

---

## 🔵 Step 1: Launch Two EC2 Instances

Deploy the same application in:

- Mumbai Region
- Singapore Region

Both applications should be identical.

---

## 🔵 Step 2: Edit the Existing A Record

Go to:

```text
Route 53

↓

Hosted Zone

↓

A Record

↓

Edit Record
```

Change:

```text
Routing Policy

From:
Simple Routing

To:
Latency Routing
```

Select:

```text
Region:
Asia Pacific (Mumbai)
```

Save the record.

---

## 🔵 Step 3: Create Another A Record

Create another **A Record** with:

```text
Record Name:
Same as previous record

Record Type:
A

Value:
Singapore EC2 Public IP

Routing Policy:
Latency Routing

Region:
Asia Pacific (Singapore)
```

Create the record.

Now Route 53 has two records for the same domain.

---

## 🔵 Step 4: Test

Users are automatically directed to the AWS Region with the **lowest network latency**.

Example:

```text
India User

↓

Mumbai EC2
```

```text
Singapore User

↓

Singapore EC2
```

No application changes are required.

Route 53 automatically selects the best endpoint based on network latency.

---

## 🔵 Best Practices

- Use Hosted Zones to manage DNS records centrally.
- Use Alias records for AWS resources whenever possible.
- Configure Health Checks for critical applications.
- Use Latency Routing for global applications.
- Use Failover Routing for disaster recovery.
- Use TTL values appropriately to balance performance and DNS updates.
- Monitor DNS health using Amazon CloudWatch.
- Remove unused Hosted Zones and domains to avoid unnecessary charges.

---

## 🔵 Route 53 Health Checks

Route 53 Health Checks continuously monitor your application's availability.

Instead of routing traffic to every server, Route 53 sends traffic **only to healthy endpoints**.

If an endpoint becomes unavailable, Route 53 automatically stops returning its DNS record (depending on the routing policy), directing users to healthy resources.

Health Checks are commonly used with:

- Failover Routing
- Latency Routing
- Weighted Routing
- Multi-Value Answer Routing

---

# Practical Demo - Configure Route 53 Health Checks

## 🔵 Step 1: Open Route 53

Go to:

```text
AWS Console

↓

Route 53

↓

Health Checks

↓

Create Health Check
```

---

## 🔵 Step 2: Configure the Health Check

Provide the following details:

```text
Name:
Mumbai-EC2-HealthCheck

What to Monitor:
Endpoint

Specify Endpoint By:
IP Address

Protocol:
HTTP or HTTPS

IP Address:
<EC2 Public IPv4 Address>

Port:
80 (HTTP)
or
443 (HTTPS)

Request Interval:
30 Seconds (Default)
or
10 Seconds (Faster Monitoring)
```

Click:

```text
Create Health Check
```

> Create a separate Health Check for every EC2 instance or endpoint you want Route 53 to monitor.

Example:

```text
Mumbai EC2

↓

Health Check 1
```

```text
Singapore EC2

↓

Health Check 2
```

---

## 🔵 Step 3: Attach the Health Check to DNS Records

Go to:

```text
Route 53

↓

Hosted Zones

↓

Select Your Domain

↓

A Record

↓

Edit Record
```

Under **Health Check**, select the Health Check created for that EC2 instance.

Save the record.

Repeat the same steps for every A Record pointing to different EC2 instances.

---

## 🔵 Step 4: Test the Health Check

Suppose you have two EC2 instances:

- Mumbai EC2
- Singapore EC2

Initially:

```text
Both EC2 Instances Running

↓

Health Check Status

Healthy

↓

Traffic Routed Normally
```

Now stop one EC2 instance.

Example:

```text
Stop Mumbai EC2
```

After a short time, Route 53 detects that the health check has failed.

Result:

```text
Mumbai EC2

↓

Unhealthy
```

```text
Singapore EC2

↓

Healthy
```

Route 53 automatically stops returning the unhealthy endpoint in DNS responses.

All new user requests are directed only to the healthy EC2 instance.

```text
Users

↓

Amazon Route 53

↓

Healthy EC2 Only
```

Start the stopped EC2 instance again.

Once the Health Check reports it as **Healthy**, Route 53 automatically includes it in DNS responses again.

---

## 🔵 Why Use Route 53 Health Checks?

- Automatically detects unhealthy servers
- Prevents users from reaching failed applications
- Improves application availability
- Enables automatic failover
- Works with multiple routing policies
- Reduces downtime without manual intervention
- Supports global applications running in multiple AWS Regions

---

## 🔵 Interview Questions

### Q1. What is the difference between DNS and Route 53?

**Answer:** DNS is the technology that translates domain names into IP addresses. Amazon Route 53 is AWS's managed DNS service that provides DNS resolution, domain registration, health checks, and intelligent traffic routing.

### Q2. Why do we use a Hosted Zone?

**Answer:** A Hosted Zone stores the DNS records for a domain. Route 53 looks up these records to determine where user requests should be routed.

### Q3. What is the purpose of an A Record?

**Answer:** An A Record maps a domain name to an IPv4 address, allowing users to access a server using a domain name instead of an IP address.

### Q4. What is Latency-Based Routing?

**Answer:** Latency-Based Routing directs users to the AWS Region that provides the lowest network latency, improving application performance for global users.

### Q5. Why is Route 53 called Route 53?

**Answer:** It is named after **Port 53**, the standard TCP/UDP port used by the Domain Name System (DNS).

### Q6. What happens if one EC2 instance fails when using Route 53 Health Checks?

**Answer:**

Route 53 continuously monitors each endpoint using Health Checks. If an EC2 instance becomes unhealthy, Route 53 stops returning its DNS record (for supported routing policies) and automatically routes users to the remaining healthy endpoint. When the failed instance becomes healthy again, Route 53 starts routing traffic to it automatically.
