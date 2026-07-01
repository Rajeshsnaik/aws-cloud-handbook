# VPC(Virtual Private Cloud)

![AWS](./aws-vpc.png)

---

## 🔵 What is VPC?

A Virtual Private Cloud (VPC) is a logically isolated network in AWS where you launch and manage resources like EC2, RDS, and Load Balancers.

---

## 🔵 Why VPC?

VPC provides:

- Network isolation
- Security control
- IP address management
- Traffic routing
- Internet connectivity control

---

## 🔵 Default VPC

A VPC automatically created by AWS in every region.

**Features:**

- Public subnets
- Internet Gateway attached
- Ready to launch resources

---

## 🔵 Custom VPC

A VPC manually created by users.

**Benefits:**

- Full network control
- Custom CIDR ranges
- Better security
- Production-ready architecture

---

## 🔵 Regional Nature of VPC

A VPC exists only within a single AWS Region.

- **Example:** `Mumbai VPC` ≠ `Singapore VPC`
- **Note:** To communicate between regions, use VPC Peering or Transit Gateway.

---

## 🔵 VPC Components

Main building blocks of a VPC:

- CIDR Block
- Subnets
- Route Tables
- Internet Gateway
- NAT Gateway
- Security Groups
- NACL
- Elastic IP

---

## 🔵 VPC Limits

AWS applies limits on VPC resources.

**Examples:**

- VPCs per Region
- Subnets per VPC
- Route Tables per VPC
- Security Groups per VPC

> 💡 **Note:** Limits can often be increased through AWS support requests.

---

## 🔵 IPv4 Addressing

IPv4 is a 32-bit IP addressing system used to identify devices on a network.

---

## 🔵 IPv6 Addressing

IPv6 is a 128-bit IP addressing system designed to replace IPv4.

---

## 🔵 CIDR Blocks

CIDR (Classless Inter-Domain Routing) defines the IP range of a network.

- **Example:** `10.0.0.0/16`
- **Note:** The `/16` determines the network size.

---

## 🔵 CIDR Calculation

CIDR determines how many IP addresses are available.

**Formula:**
$$2^{(32 - \text{CIDR})}$$

---

## 🔵 Public IP

An IP address reachable from the internet.

---

## 🔵 Private IP

An IP address used only within private networks. It **cannot** be accessed directly from the internet.

---

## 🔵 Elastic IP (EIP)

A static public IPv4 address provided by AWS.

- **Benefit:** Fixed public IP that can be reassigned to another EC2 instance.
- **Use Case:** Production servers needing a permanent public IP.

---

## 🔵 Secondary IP Addresses

Additional private IP addresses assigned to a network interface (ENI).

- **Example:**
  - **Primary IP:** `10.0.1.10`
  - **Secondary IP:** `10.0.1.20`, `10.0.1.30`
- **Use Case:** Hosting multiple applications on one server.

---

## 🔵 BYOIP (Bring Your Own IP)

Allows you to use your own public IP ranges in AWS.

- **Benefit:** Keep existing IP reputation and experience an easier migration to AWS.
- **Example:** A company moves its owned public IP block to AWS instead of using AWS-assigned IPs.

# 🔵 What is a Subnet?

A **Subnet** is a smaller network created inside a **VPC (Virtual Private Cloud)** to organize, isolate, and manage AWS resources more efficiently. Instead of placing all resources in one large network, a VPC is divided into multiple subnets based on security, availability, and application requirements.

### Example

```text
VPC (10.0.0.0/16)

├── Subnet-1 (10.0.1.0/24)
└── Subnet-2 (10.0.2.0/24)
```

---

# 🔵 Public Subnet

A **Public Subnet** is a subnet that has a route to an **Internet Gateway (IGW)**.

Resources inside a public subnet can communicate directly with the internet if they have a public IP address.

---

# 🔵 Private Subnet

A **Private Subnet** is a subnet that does **not** have a direct route to the **Internet Gateway (IGW)**.

Resources inside a private subnet usually access the internet through a **NAT Gateway** for outbound connections while remaining inaccessible from the public internet.

---

# 🔵 Isolated Subnet

An **Isolated Subnet** is a subnet that has **no Internet Gateway (IGW)** and **no NAT Gateway** access.

Resources inside an isolated subnet cannot access the internet and cannot be accessed from the internet.

### Common Use Cases

- Databases
- Internal Services
- Highly Secure Workloads

---

# 🔵 Subnet CIDR Planning

**Subnet CIDR Planning** is the process of dividing a VPC CIDR block into multiple smaller subnet CIDR blocks.

Proper subnet planning helps avoid IP address conflicts and allows room for future expansion.

### Example

#### VPC CIDR

```text
10.0.0.0/16
```

#### Subnets

```text
10.0.1.0/24
10.0.2.0/24
10.0.3.0/24
```

### Goal

- Avoid IP conflicts
- Support future growth
- Organize resources efficiently

---

# 🔵 Availability Zones (AZs)

An **Availability Zone (AZ)** is an isolated AWS data center within an AWS Region.

Each Availability Zone operates independently with separate power, networking, and cooling infrastructure.

### Example

#### Mumbai Region

```text
ap-south-1a
ap-south-1b
ap-south-1c
```

AWS recommends creating subnets across multiple Availability Zones to improve application availability and fault tolerance.

---

# 🔵 Multi-AZ Design

**Multi-AZ Design** means deploying application resources across multiple Availability Zones.

### Example

```text
AZ-A           AZ-B

Web Server     Web Server
Database       Database
```

### Benefit

If one Availability Zone becomes unavailable, the other Availability Zone continues serving application traffic.

---

# 🔵 Subnet Sizing

**Subnet Sizing** is the process of determining how many IP addresses a subnet should contain.

Choose subnet sizes based on your current requirements and expected future resource growth.

### Examples

```text
/24 = 256 IPs
/25 = 128 IPs
/26 = 64 IPs
/27 = 32 IPs
```

---

# 🔵 Reserved IP Addresses

AWS automatically reserves **5 IP addresses** in every subnet.

### Example

#### Subnet

```text
10.0.1.0/24
```

#### Reserved IP Addresses

```text
10.0.1.0
10.0.1.1
10.0.1.2
10.0.1.3
10.0.1.255
```

#### Usable IP Addresses

```text
256 - 5 = 251
```

---

# 🔵 Route Table

A **Route Table** is a collection of rules (called **routes**) that determines where network traffic from a subnet should be directed.

Each route specifies a destination CIDR block and a target, such as an Internet Gateway, NAT Gateway, Virtual Private Gateway, or another network device.

---

# 🔵 Main Route Table

The **Main Route Table** is the default route table that is automatically created whenever a new VPC is created.

Every subnet in the VPC is automatically associated with the Main Route Table unless it is explicitly associated with a custom route table.

### Purpose

- Provides default routing for subnets
- Used by subnets without a custom route table
- Can be modified or replaced if needed

---

# 🔵 Custom Route Tables

A **Custom Route Table** is a user-created route table designed for specific routing requirements.

Different subnets can be associated with different route tables depending on the application's networking needs.

### Example

```text
Public Subnet  → Public Route Table
Private Subnet → Private Route Table
```

---

# 🔵 Route Propagation

**Route Propagation** is the automatic addition of routes into a route table from AWS networking services.

Services such as **VPN Gateway** and **Transit Gateway** can automatically advertise routes without requiring manual configuration.

### Benefit

- Automatically updates routing information
- Reduces manual configuration
- Simplifies network management

---

# 🔵 Static Routes

**Static Routes** are routes that are manually created and managed by administrators.

These routes remain unchanged until they are manually modified or deleted.

### Example

```text
0.0.0.0/0 → Internet Gateway
```

---

# 🔵 Dynamic Routes

**Dynamic Routes** are automatically learned and updated using routing protocols such as **BGP (Border Gateway Protocol)**.

AWS commonly uses dynamic routing for hybrid networking environments.

### Used With

- VPN
- Direct Connect
- Transit Gateway

---

# 🔵 Route Priority

When multiple routes match the same destination, AWS selects the route with the **highest priority**.

Route priority is determined by the **most specific destination CIDR block**, not by the order in which routes appear.

---

# 🔵 Longest Prefix Match

AWS always uses the **Longest Prefix Match** rule when selecting a route.

The route with the most specific CIDR block is chosen.

### Example

```text
Available Routes

10.0.0.0/16
10.0.1.0/24
```

Traffic Destination

```text
10.0.1.10
```

Selected Route

```text
10.0.1.0/24
```

Because the **/24** CIDR block is more specific than the **/16** CIDR block, AWS chooses the **10.0.1.0/24** route.

---

# 🔵 Route Troubleshooting

When network connectivity fails, verify the following:

- Route Table entries
- Internet Gateway attachment
- NAT Gateway routes
- Security Group rules
- Network ACL (NACL) rules
- Subnet Route Table Association

---

# 🔵 Internet Gateway (IGW)

An **Internet Gateway (IGW)** is a VPC component that enables communication between a **VPC** and the **Internet**.

It serves as the gateway that allows resources inside a VPC to send and receive internet traffic.

### Purpose

- Provide internet access for public resources
- Enable inbound and outbound internet communication
- Connect a VPC to the public internet

---

# 🔵 IGW Architecture

An **Internet Gateway** is attached directly to a **VPC**.

Resources inside a public subnet use the Internet Gateway to communicate with the internet.

### Architecture

```text
Internet
    |
   IGW
    |
   VPC
    |
Public Subnet
    |
   EC2
```

Without an Internet Gateway, resources inside the VPC cannot communicate with the public internet.

---

# 🔵 Public Internet Access

For an EC2 instance to be publicly accessible from the internet, all of the following conditions must be met:

- The instance must be in a **Public Subnet**
- The subnet's Route Table must contain a route to the **Internet Gateway**
- The instance must have a **Public IP Address** or an **Elastic IP Address**
- The **Security Group** must allow the required inbound traffic

### Example

```text
Internet
    |
   IGW
    |
Public Subnet
    |
EC2 (Public IP)
```

---

# 🔵 Outbound Internet Access

**Outbound Internet Access** allows AWS resources to initiate connections to the internet.

This is commonly used when instances need to download updates, install software, or communicate with external services.

### Examples

- Download software updates
- Install application packages
- Access external APIs
- Connect to third-party services

### Flow

```text
EC2
 |
IGW
 |
Internet
```

---

# 🔵 Inbound Internet Access

**Inbound Internet Access** allows users or applications on the internet to connect to AWS resources.

Only resources that are properly configured for public access can receive inbound internet traffic.

### Examples

- Website access
- API access
- SSH access
- HTTPS traffic

### Flow

```text
User
 |
Internet
 |
IGW
 |
EC2
```

**Security Groups** and **Network ACLs (NACLs)** control which inbound traffic is allowed.

---

# 🔵 Public Website Architecture

A common AWS architecture for hosting public web applications.

### Architecture

```text
Users
   |
Internet
   |
Internet Gateway
   |
Public Subnet
   |
Load Balancer
   |
Web Servers
```

### Components

- VPC
- Public Subnet
- Internet Gateway
- Load Balancer
- EC2 Web Servers

This is one of the most common production architectures used to host highly available public websites and web applications on AWS.

---

# 🔵 NAT Gateway

A **NAT Gateway (Network Address Translation Gateway)** is a managed AWS service that allows resources in a **Private Subnet** to access the Internet while preventing inbound connections from the Internet.

Resources in a private subnet can initiate outbound connections, but external users cannot directly connect to them.

### Use Cases

- Operating System Updates
- Package Downloads
- Access External APIs
- Software Installation
- Access Third-Party Services

---

# 🔵 Cost Optimization

A **NAT Gateway** can become expensive in large environments because AWS charges for both hourly usage and data processing.

### Cost Optimization Tips

- Use **VPC Endpoints** for Amazon S3 and DynamoDB
- Avoid unnecessary Internet traffic
- Place workloads in private subnets only when required
- Use NAT Gateway only for resources that need outbound Internet access

---

# 🔵 NAT Instance

A **NAT Instance** is an EC2 instance configured to provide Network Address Translation (NAT) functionality.

Unlike a NAT Gateway, a NAT Instance is fully managed by the customer.

### Requirements

- Must be launched in a **Public Subnet**
- Must have a **Public IP Address** or **Elastic IP**
- Proper **Route Table** configuration
- IP Forwarding enabled
- Source/Destination Check disabled

---

# 🔵 NAT Gateway vs NAT Instance

| NAT Gateway             | NAT Instance                             |
| ----------------------- | ---------------------------------------- |
| Managed AWS Service     | EC2 Instance                             |
| Highly Available        | Manual High Availability                 |
| Automatic Scaling       | Manual Scaling                           |
| Higher Cost             | Lower Cost                               |
| AWS Managed             | Customer Managed                         |
| Better Performance      | Performance depends on EC2 Instance Type |
| No Maintenance Required | Requires OS Updates and Maintenance      |

---

# 🔵 Security Group

A **Security Group (SG)** is a virtual firewall that protects AWS resources such as EC2 instances.

It controls which traffic is allowed to enter and leave the resource.

### Controls

- Inbound Traffic
- Outbound Traffic

---

# 🔵 Stateful Firewall

**Security Groups are stateful.**

If an inbound request is allowed, the corresponding outbound response is automatically allowed, even if an outbound rule is not explicitly configured.

Similarly, if outbound traffic is allowed, the return traffic is automatically permitted.

---

# 🔵 Security Group Best Practices

- Follow the principle of least privilege
- Open only the required ports
- Avoid using **0.0.0.0/0** unless necessary
- Use Security Group referencing instead of IP addresses where possible
- Regularly review and remove unused rules
- Separate Security Groups based on application roles

---

# 🔵 Network ACL (NACL)

A **Network Access Control List (NACL)** is a firewall that operates at the **Subnet Level**.

It controls all inbound and outbound traffic entering or leaving an entire subnet.

### Controls

- Entire Subnet Traffic

---

# 🔵 Stateless Firewall

**Network ACLs are stateless.**

Inbound and outbound traffic are evaluated independently.

If inbound traffic is allowed, you must also explicitly allow the outbound response traffic.

---

# 🔵 NACL Best Practices

- Use specific allow and deny rules
- Keep rule numbers organized
- Use deny rules when required
- Document important rules
- Remove unnecessary rules periodically

---

# 🔵 Security Group vs NACL

| Security Group                  | NACL                           |
| ------------------------------- | ------------------------------ |
| Instance Level                  | Subnet Level                   |
| Stateful                        | Stateless                      |
| Supports Allow Rules Only       | Supports Allow and Deny Rules  |
| Rule Order Does Not Matter      | Rule Order Matters             |
| Easier to Manage                | Provides More Granular Control |
| Applied to Individual Resources | Applied to Entire Subnets      |

---

# 🔵 Amazon DNS

**Amazon DNS** is the built-in Domain Name System (DNS) service provided automatically within every **Amazon VPC**.

It enables AWS resources inside the VPC to communicate using hostnames instead of IP addresses.

### Purpose

- Resolve internal AWS hostnames
- Enable communication between AWS resources
- Automatically assign DNS names to supported resources
- Simplify resource discovery within a VPC

### Example

```text
ip-10-0-1-10.ec2.internal
```

---

# 🔵 Route 53

**Amazon Route 53** is AWS's fully managed and highly available Domain Name System (DNS) service.

It helps route users to applications hosted in AWS or outside AWS.

### Features

- Domain Registration
- DNS Management
- Health Checks
- Traffic Routing
- High Availability
- Failover Routing
- Routing Policies

---

# 🔵 Public Hosted Zones

A **Public Hosted Zone** is a Route 53 hosted zone that manages DNS records accessible from the **public Internet**.

When someone accesses your domain, Route 53 responds with the appropriate DNS records.

### Common Use Cases

- Public Websites
- APIs
- Public Load Balancers
- Internet-facing Applications

---

# 🔵 Private Hosted Zones

A **Private Hosted Zone** is a Route 53 hosted zone that is accessible **only from associated VPCs**.

DNS records in a private hosted zone cannot be resolved from the public Internet.

### Examples

```text
database.internal
app.company.local
```

### Characteristics

- Accessible only inside associated VPCs
- Not reachable from the Internet
- Used for internal applications and services

---

# 🔵 DNS Hostnames

**DNS Hostnames** are DNS names automatically assigned to supported AWS resources.

Instead of connecting using IP addresses, users and applications can connect using DNS names.

### Example

```text
ec2-54-12-34-56.compute.amazonaws.com
```

### Benefits

- Easier resource identification
- Simplifies application configuration
- Supports dynamic IP address changes
- Reduces dependency on static IP addresses

---

# 🔵 Route 53 Resolver

**Route 53 Resolver** is the DNS resolution service used by Amazon Route 53 to answer DNS queries.

It enables DNS resolution between AWS environments and external networks such as on-premises data centers.

### Purpose

- Resolve DNS queries
- Integrate AWS and On-Premises DNS
- Forward DNS requests between networks
- Enable hybrid cloud DNS resolution

### Common Use Cases

- Hybrid Cloud Environments
- VPN Connectivity
- AWS Direct Connect
- Multi-VPC Architectures

---

# 🔵 VPC Peering

**VPC Peering** is a private networking connection between two **Amazon VPCs** that allows resources in both VPCs to communicate with each other using **private IP addresses**.

The communication occurs over the AWS global private network without traversing the public internet.

### Purpose

- Allow private communication between different VPCs
- Share resources securely across VPCs
- Reduce internet exposure
- Enable low-latency communication

### Architecture

```text
VPC-A  ←────→  VPC-B
```

Traffic between the VPCs remains entirely on the AWS private network.

---

# 🔵 Cross-Region Peering

**Cross-Region VPC Peering** connects two VPCs located in different **AWS Regions**.

It enables private communication between resources across regions using the AWS global backbone network.

### Purpose

- Connect applications deployed in multiple AWS Regions
- Support global architectures
- Enable Disaster Recovery (DR) environments
- Improve cross-region application connectivity

### Example

```text
Mumbai (ap-south-1)
        │
        │
VPC-A ───────────── VPC-B
        │
        │
Frankfurt (eu-central-1)
```

Traffic continues to use the AWS private global network instead of the public internet.

---

# 🔵 AWS PrivateLink

**AWS PrivateLink** is a networking service that provides **private connectivity** between **Amazon VPCs**, **AWS services**, and **SaaS applications** without sending traffic over the public Internet.

Traffic remains entirely on the AWS private network, improving security and reducing exposure to the public internet.

### Purpose

- Secure private access to services
- Eliminate public Internet exposure
- Simplify service sharing between VPCs
- Enable private access to AWS and third-party services

---

# 🔵 Interface Endpoints

An **Interface Endpoint** is an **Elastic Network Interface (ENI)** created inside your subnet that enables private access to supported AWS services or PrivateLink endpoint services.

Each Interface Endpoint receives one or more private IP addresses within your subnet.

### Benefits

- Private connectivity to AWS services
- No Internet Gateway required
- No NAT Gateway required
- Traffic stays within the AWS network

---

# 🔵 Endpoint Services

An **Endpoint Service** allows you to expose your own application or service privately to other AWS accounts or VPCs using **AWS PrivateLink**.

Consumers connect to your service through an **Interface Endpoint**, without requiring VPC Peering or public IP addresses.

### Architecture

```text
Provider VPC
     |
Endpoint Service
     |
PrivateLink
     |
Consumer VPC
```

### Common Use Cases

- Sharing internal applications
- SaaS platforms
- Multi-account architectures
- Private API services

---

# 🔵 AWS PrivateLink vs VPC Peering

| AWS PrivateLink                    | VPC Peering                         |
| ---------------------------------- | ----------------------------------- |
| Service-Level Access               | Network-Level Access                |
| More Secure                        | Broader Network Access              |
| Supports Overlapping CIDR Blocks   | CIDR Overlap Not Supported          |
| One-Way Service Access             | Full Two-Way VPC Communication      |
| Scales Well for Large Environments | Becomes Complex at Large Scale      |
| Does Not Expose Entire VPC         | Entire VPC Networks Can Communicate |
| No Route Table Changes Required    | Route Table Configuration Required  |

```

```

---

# 🔵 VPC Endpoint

A **VPC Endpoint** is a networking feature that allows resources inside a **VPC** to privately connect to supported **AWS services** without using an **Internet Gateway (IGW)**, **NAT Gateway**, **VPN**, or **AWS Direct Connect**.

Traffic remains entirely on the AWS private network, improving security and reducing network costs.

### Types

- Gateway Endpoint
- Interface Endpoint

---

# 🔵 S3 Gateway Endpoint

An **S3 Gateway Endpoint** provides private connectivity between your VPC and **Amazon S3**.

Resources inside the VPC can access S3 without requiring internet connectivity.

### Benefits

- No Internet Gateway required
- No NAT Gateway required
- Lower networking costs
- Improved security
- Traffic remains on the AWS private network

### Architecture

```text
EC2
 |
S3 Gateway Endpoint
 |
S3 Bucket
```

---

# 🔵 DynamoDB Gateway Endpoint

A **DynamoDB Gateway Endpoint** provides private access to **Amazon DynamoDB** from resources within a VPC.

Traffic never leaves the AWS private network.

### Benefits

- Secure communication
- No Internet Gateway required
- No NAT Gateway required
- Reduced NAT Gateway costs
- Improved application security

### Architecture

```text
EC2
 |
DynamoDB Gateway Endpoint
 |
DynamoDB
```

---

# 🔵 Route Tables Integration

**Gateway Endpoints** work by automatically adding routes to the **Route Tables** associated with your VPC.

When traffic is destined for supported AWS services such as Amazon S3 or DynamoDB, it is automatically routed through the Gateway Endpoint instead of the public internet.

### Example

```text
Destination          Target
S3 Prefix List       Gateway Endpoint
```

This ensures that traffic reaches the AWS service privately through the VPC Endpoint.
