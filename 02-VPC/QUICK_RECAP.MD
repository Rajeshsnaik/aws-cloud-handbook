# Quick Recap

```text
IPv4
=
32-bit IP Addressing System

IPv6
=
128-bit IP Addressing System

CIDR
=
Defines Network IP Range

CIDR Calculation
=
Calculates Available IP Addresses

Public IP
=
Internet-Accessible IP Address

Private IP
=
Internal Network IP Address

Elastic IP (EIP)
=
Static Public IP from AWS

Secondary IP
=
Additional IP on the Same ENI

BYOIP
=
Bring Your Own Public IP Addresses to AWS
```

---

```text
Subnet
=
Smaller Network Inside a VPC

Public Subnet
=
Subnet with Internet Gateway Access

Private Subnet
=
Subnet Without Direct Internet Access

Isolated Subnet
=
Subnet with No Internet Access

Subnet CIDR Planning
=
Dividing VPC IP Range into Smaller Subnets

Availability Zone (AZ)
=
Isolated AWS Data Center

Multi-AZ Design
=
Deploy Resources Across Multiple AZs

Subnet Sizing
=
Selecting Subnet IP Capacity

Reserved IP Addresses
=
AWS Reserves 5 IP Addresses Per Subnet
```

---

```text
Route Table
=
Traffic Direction

Main Route Table
=
Default Route Table

Custom Route Table
=
User-Created Route Table

Static Route
=
Manually Added Route

Dynamic Route
=
Automatically Learned Route

Route Propagation
=
Automatically Adds Routes from VPN/TGW

Route Priority
=
Most Specific Route Wins

Longest Prefix Match
=
Most Specific CIDR Wins
```

---

```text
Internet Gateway (IGW)
      |
      |-- Provides Internet Connectivity
      |
      |-- Supports Inbound Traffic
      |
      |-- Supports Outbound Traffic
      |
      |-- Attached to VPC
```

**Remember:**

```text
Public Subnet
      +
Route to IGW
      +
Public IP / Elastic IP
      =
Internet Accessible Resource
```

---

```text
NAT Gateway
=
Managed Outbound Internet Access

NAT Instance
=
EC2-Based NAT Solution

Security Group
=
Instance Firewall

Stateful
=
Return Traffic Automatically Allowed

Allow Rules
=
Supported

NACL
=
Subnet Firewall

Stateless
=
Inbound and Outbound Rules Required

Allow & Deny Rules
=
Supported
```

---

```text
Amazon DNS
=
Internal AWS DNS

Route 53
=
AWS Managed DNS Service

Public Hosted Zone
=
Internet DNS

Private Hosted Zone
=
Internal VPC DNS

DNS Hostnames
=
Automatic DNS Names for AWS Resources

DNS Resolution
=
Domain → IP Address

Route 53 Resolver
=
AWS ↔ On-Premises DNS Communication
```

---

```text
VPC Peering
      =
Private VPC-to-VPC Connection

Same Region Peering
      =
Within Same AWS Region

Cross Region Peering
      =
Across Different AWS Regions

Traffic
      =
Stays on AWS Private Network

Limitations
      =
No Transitive Routing
No Overlapping CIDRs

Requirement
      =
Routes + Security Rules

---

Can VPC-A communicate with VPC-C through VPC-B?

VPC-A ↔ VPC-B ↔ VPC-C

Answer
=
No

Reason
=
VPC Peering Does Not Support Transitive Routing

Solution
=
Use Transit Gateway
```

---

```text
Transit Gateway
      =
Central Network Hub

Hub and Spoke
      =
All Networks Connect to TGW

Attachment
      =
Connection Between TGW and Network

TGW Route Table
      =
Controls Traffic Flow

Cross Account
      =
Supported

Cross Region
      =
Supported

Transitive Routing
      =
Supported
```

**Rule to Remember:**

```text
Few VPCs
      →
VPC Peering

Many VPCs
      →
Transit Gateway
```

---

```text
AWS PrivateLink
      =
Private Service Connectivity

Interface Endpoint
      =
Private Access to AWS Services

Endpoint Service
      =
Expose Your Service Privately

Benefits
      =
No Internet
No NAT Gateway
No Public IP

Traffic
      =
Stays Inside AWS Private Network
```

---

```text
VPC Endpoint
      =
Private Access to AWS Services

Gateway Endpoint
      =
Amazon S3
Amazon DynamoDB

Interface Endpoint
      =
Most AWS Services

Gateway Endpoint Uses
      =
Route Tables

Interface Endpoint Uses
      =
Elastic Network Interface (ENI)

Private DNS
      =
Use Standard AWS DNS Names

Endpoint Policy
      =
Controls Access Through Endpoint
```
