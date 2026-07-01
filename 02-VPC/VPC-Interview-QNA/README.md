# AWS Networking and VPC Technical Interview Guide

This document covers 70 essential technical interview questions and comprehensive answers regarding AWS Networking, VPC architecture, connectivity, security, and troubleshooting.

---

## Virtual Private Cloud Essentials

### Q1. What is a VPC?

A Virtual Private Cloud (VPC) is a logically isolated virtual network dedicated to your AWS account. It gives you complete control over your virtual networking environment, including the selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.

### Q2. Difference between Default VPC and Custom VPC?

- **Default VPC:** Automatically created by AWS in every region for a new account. It comes pre-configured with a public subnet in each Availability Zone, an attached Internet Gateway, a default security group, a network access control list (NACL), and a public IP allocation pool so you can immediately launch instances.
- **Custom VPC:** Created and configured completely from scratch by the user. It has no default subnets or internet connectivity until you manually configure CIDR blocks, subnets, route tables, network gateways, and security rules.

### Q3. Is VPC regional or global?

A VPC is a **Regional** service. It cannot span multiple AWS Regions.

### Q4. Can a VPC span multiple Availability Zones?

Yes. While a VPC itself spans all Availability Zones within its target region, the individual subnets created inside that VPC must reside strictly within a single Availability Zone.

### Q5. Can a VPC span multiple Regions?

No. A VPC is bound strictly to the single AWS Region in which it was created.

---

## CIDR and IP Addressing

### Q6. What is CIDR?

Classless Inter-Domain Routing (CIDR) is a method used to allocate IP addresses and route Internet Protocol packets. In AWS, a CIDR block (e.g., `10.0.0.0/16`) defines the overall block of IP addresses available for allocation within your VPC or individual subnets.

### Q7. How many IPs are available in a /24 subnet?

A `/24` subnet uses 24 bits for the network portion, leaving 8 bits for host addresses.
The mathematical calculation is: `2^(32 - 24) = 2^8 = 256`.
Therefore, a total of 256 IP addresses are structurally available.

### Q8. How many usable IPs are available in AWS /24 subnet?

There are 251 usable IP addresses.
Calculation: `256 - 5 = 251`.
AWS automatically reserves 5 IP addresses in every subnet for internal management purposes:

- `x.x.x.0`: Network address.
- `x.x.x.1`: VPC router address.
- `x.x.x.2`: AWS DNS (Amazon Provided DNS) server address.
- `x.x.x.3`: Reserved by AWS for future usage.
- `x.x.x.255`: Network broadcast address (AWS does not support broadcast, but the address remains reserved).

### Q9. Difference between Public IP and Private IP?

| Public IP                                            | Private IP                                                     |
| ---------------------------------------------------- | -------------------------------------------------------------- |
| Accessible directly over the global internet.        | Accessible only within the internal VPC or connected networks. |
| Must be globally unique across the entire internet.  | Must be unique only within the boundaries of the local VPC.    |
| Assigned via AWS pools or Bring Your Own IP (BYOIP). | Allocated from the internal CIDR block range of the subnet.    |

---

## Subnets

### Q10. What makes a subnet public?

A subnet is classified as public if its associated route table contains a direct route targeting an attached Internet Gateway (IGW) (typically `0.0.0.0/0 -> igw-xxxxxx`), and resources launched within it are assigned a Public IP address.

### Q11. What makes a subnet private?

A subnet is private when its associated route table does not contain a direct route to an Internet Gateway. Resources in a private subnet cannot be reached directly from the public internet.

### Q12. What is an Isolated Subnet?

An isolated subnet is a completely locked-down private subnet that contains no outbound or inbound route paths to the public internet. It features no route to an Internet Gateway and no route to a NAT Gateway.

### Q13. Which subnet is preferred for databases?

An Isolated Subnet is highly preferred for database workloads to ensure maximum security, preventing both external internet attacks and unintended outbound data exfiltration.

---

## Route Tables

### Q14. What is a Route Table?

A route table contains a set of rules (called routes) that determine where network traffic directed from your subnet or gateway should be forwarded.

### Q15. What is the Main Route Table?

The Main Route Table is the default routing configuration automatically created with every new VPC. Any subnet that is not explicitly associated with a custom route table will implicitly inherit the routing rules of the Main Route Table.

### Q16. Difference between Static and Dynamic Routes?

| Static Routes                                            | Dynamic Routes                                                        |
| -------------------------------------------------------- | --------------------------------------------------------------------- |
| Manually configured and maintained by the administrator. | Automatically advertised, learned, and updated via routing protocols. |
| Fixed paths that do not adapt automatically to failures. | Leverages Border Gateway Protocol (BGP) to adapt dynamically.         |

### Q17. What is Longest Prefix Match?

AWS route tables use the Longest Prefix Match rule to determine routing priority when multiple matching rules exist. The router selects the most specific configuration (the one with the longest prefix/largest CIDR mask).

**Example:**

- Route 1: `10.0.0.0/16`
- Route 2: `10.0.1.0/24`

If traffic is destined for `10.0.1.10`, AWS evaluates both rules and selects `10.0.1.0/24` because its prefix match is longer and more specific.

---

## Internet Gateway

### Q18. What is an Internet Gateway?

An Internet Gateway (IGW) is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC resources and the public internet. It performs network address translation (NAT) for your public instances.

### Q19. Can a private subnet use an IGW directly?

No. Private subnets do not have a direct route to an IGW. To access the internet, they must route traffic through an intermediary component like a NAT Gateway located in a public subnet.

### Q20. Requirements for Internet access?

To achieve bidirectional public internet communication, a resource requires:

- Location within a Public Subnet.
- An active route table entry directing internet traffic (`0.0.0.0/0`) to an Internet Gateway.
- A globally unique Public IP or Elastic IP address.

---

## NAT Gateway

### Q21. Why do we need a NAT Gateway?

A Network Address Translation (NAT) Gateway allows instances located inside a private subnet to securely connect outbound to the internet or other AWS services (e.g., for software patches or system updates), while blocking any inbound connections initiated from the internet.

### Q22. Can Internet users initiate connections to resources behind NAT Gateway?

No. NAT Gateways are strictly unidirectional; they accept only outbound-initiated traffic and its corresponding return packets.

### Q23. Where should NAT Gateway be deployed?

A NAT Gateway must always be deployed within a designated Public Subnet, as it requires its own public route to the Internet Gateway to function.

### Q24. NAT Gateway vs NAT Instance?

| NAT Gateway                                              | NAT Instance                                                        |
| -------------------------------------------------------- | ------------------------------------------------------------------- |
| Managed enterprise service by AWS.                       | Self-managed EC2 instance running a custom NAT AMI.                 |
| Scales automatically to handle traffic spikes.           | Throughput is limited to the specific EC2 instance type capability. |
| Built-in high availability within its Availability Zone. | Requires custom scripting or autoscaling groups to manage failover. |

---

## Security Groups

### Q25. What is a Security Group?

A Security Group acts as a virtual firewall that controls inbound and outbound traffic at the individual instance level (e.g., EC2 instances or RDS databases).

### Q26. Is Security Group Stateful?

Yes. Security Groups are stateful.

### Q27. What does Stateful mean?

Stateful means that if an inbound request is permitted through the firewall rules, the corresponding outbound return traffic is automatically allowed to pass back out, regardless of any outbound security rules. The inverse is also true for outbound-initiated requests.

### Q28. Can Security Groups deny traffic?

No. Security Groups support "Allow" rules only. You cannot explicitly write a rule to block or deny specific IP addresses; any traffic not explicitly allowed is implicitly denied.

### Q29. At which level does Security Group operate?

It operates strictly at the Instance Level (Network Interface / ENI layer), meaning rules are applied independently per resource.

---

## Network Access Control Lists (NACL)

### Q30. What is NACL?

A Network Access Control List (NACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.

### Q31. Is NACL Stateful?

No. NACLs are entirely stateless.

### Q32. Can NACL deny traffic?

Yes. NACLs support both explicit "Allow" and explicit "Deny" rules. This makes them ideal for blocking specific malicious IP ranges or CIDR blocks before they reach your subnets.

### Q33. At which level does NACL operate?

It operates at the Subnet Level. Any rule written inside a NACL evaluates all traffic crossing the boundary of the entire subnet.

### Q34. Difference between Security Group and NACL?

| Feature        | Security Group                     | NACL                                            |
| -------------- | ---------------------------------- | ----------------------------------------------- |
| **State**      | Stateful (Evaluates one way)       | Stateless (Must evaluate both ways explicitly)  |
| **Rule Type**  | Allow rules only                   | Supports both Allow and Deny rules              |
| **Scope**      | Instance Level (ENI)               | Subnet Level                                    |
| **Rule Order** | Evaluates all rules simultaneously | Evaluates rules sequentially in numerical order |

---

## DNS and Route 53

### Q35. What is DNS?

Domain Name System (DNS) is a hierarchical and decentralized naming system used to translate human-readable domain names (such as `example.com`) into computer-routable numerical IP addresses.

### Q36. What is Route 53?

Route 53 is a highly available and scalable cloud Domain Name System (DNS) web service provided by AWS, designed to handle public domain registration, DNS routing, and resource health checking.

### Q37. Difference between Public and Private Hosted Zone?

- **Public Hosted Zone:** Contains DNS records accessible anywhere from the global internet.
- **Private Hosted Zone:** Contains DNS records accessible exclusively internally within one or more specified VPCs.

### Q38. What is Split Horizon DNS?

Split Horizon DNS is a architecture pattern where the exact same domain name resolves to completely different IP addresses depending on whether the query originates from inside the internal corporate network (VPC) or from the external public internet.

---

## VPC Peering

### Q39. What is VPC Peering?

VPC Peering is a direct networking connection between two VPCs that enables you to route traffic between them privately using internal IPv4 or IPv6 addresses.

### Q40. Does VPC Peering support transitive routing?

No. VPC Peering is non-transitive. If VPC A is peered with VPC B, and VPC B is peered with VPC C, VPC A cannot communicate with VPC C through VPC B. A distinct peer connection must be made directly between A and C.

### Q41. Can VPCs with overlapping CIDRs be peered?

No. AWS blocks any peering attempts between two VPCs if their primary or secondary CIDR blocks overlap, as it would cause unresolvable network routing conflicts.

### Q42. Is VPC Peering private?

Yes. The traffic flows entirely across the secure AWS internal global backbone network; it never traverses the public internet, ensuring high security and consistent latency.

### Q43. Can VPC Peering be Cross-Region?

Yes. Inter-Region VPC Peering allows you to peer VPCs across different geographic AWS regions.

---

## Transit Gateway

### Q44. What is Transit Gateway?

AWS Transit Gateway acts as a centralized cloud router that connects thousands of VPCs, on-premises networks, and VPN connections through a single, managed hub-and-spoke gateway architectural framework.

### Q45. Does Transit Gateway support transitive routing?

Yes. Transit Gateway natively supports transitive routing, enabling full mesh or complex multi-VPC connectivity patterns without individual point-to-point peering connections.

### Q46. Transit Gateway vs VPC Peering?

- **VPC Peering:** Recommended for connecting a small number of VPCs (e.g., less than 10). It is highly cost-effective but becomes complex to manage at scale due to `N*(N-1)/2` peer mesh configurations.
- **Transit Gateway:** Recommended for complex enterprise networks with many VPCs and on-premises sites, simplifying the architecture into a centralized hub management system.

### Q47. What architecture does Transit Gateway use?

It uses a **Hub and Spoke** architecture.

---

## PrivateLink

### Q48. What is AWS PrivateLink?

AWS PrivateLink provides highly secure, private connectivity between VPCs, AWS services, and on-premises applications, keeping all data traffic entirely inside the AWS network without exposing it to the public internet.

### Q49. Does PrivateLink require Internet Gateway?

No. PrivateLink routes all traffic through internal AWS endpoints, removing any dependency on an Internet Gateway.

### Q50. Does PrivateLink require NAT Gateway?

No. Traffic to PrivateLink services is handled internally, bypassing the need for a NAT Gateway.

### Q51. What component does PrivateLink use?

It uses an **Interface Endpoint** (which allocates an Elastic Network Interface with a private IP directly inside your subnet).

---

## VPC Endpoints

### Q52. What are VPC Endpoints?

VPC Endpoints are virtual devices that enable private connections between your VPC and supported AWS services or VPC endpoint services powered by PrivateLink, avoiding traffic exposure to public routing pathways.

### Q53. Types of VPC Endpoints?

- **Gateway Endpoints:** Implemented as a routing target entry in your route table.
- **Interface Endpoints:** Implemented as physical private network interfaces (ENIs) with private IP addresses.

### Q54. Which services support Gateway Endpoints?

Gateway Endpoints support only two specific services:

- Amazon S3
- Amazon DynamoDB

### Q55. Which services use Interface Endpoints?

Interface Endpoints power nearly all other supported AWS integrations, including Amazon SQS, SNS, CloudWatch, Secrets Manager, Systems Manager, and AWS KMS.

### Q56. How can a private EC2 access S3 without NAT Gateway?

By configuring and associating an **S3 Gateway Endpoint** to the private subnet's route table. This establishes a direct internal network path from the instance to S3, bypassing the internet entirely.

---

## Virtual Private Network (VPN)

### Q57. What is Site-to-Site VPN?

An AWS Site-to-Site VPN creates a secure, encrypted connection (IPsec tunnel) between your remote on-premises data center or office network and your AWS cloud VPC environment.

### Q58. VPN uses Public Internet or Private Connection?

It travels over the **Public Internet**, but the data payload is fully encrypted using IPsec to ensure absolute privacy during transit.

### Q59. Which protocol is commonly used for dynamic routing?

**Border Gateway Protocol (BGP)** is used to establish and manage dynamic routing advertisements over the VPN tunnels.

---

## Direct Connect

### Q60. What is AWS Direct Connect?

AWS Direct Connect is a cloud service solution that links an on-premises network directly to AWS locations over dedicated, private physical network connections, completely bypassing internet service providers.

### Q61. Direct Connect vs VPN?

| Feature              | Direct Connect                                 | VPN                                                       |
| -------------------- | ---------------------------------------------- | --------------------------------------------------------- |
| **Transport Medium** | Private, dedicated fiber-optic link.           | Public internet pathing.                                  |
| **Latency**          | Highly consistent and predictable low latency. | Variable latency depending on public internet congestion. |
| **Cost**             | High fixed costs; meant for enterprise setups. | Highly cost-effective; rapid deployment model.            |

### Q62. Which is more reliable?

**Direct Connect** is substantially more reliable because it operates over a physical dedicated network link with no exposure to public internet routing shifts, dropouts, or bandwidth bottlenecks.

---

## Elastic IP

### Q63. What is Elastic IP?

An Elastic IP address is a static, reserved IPv4 address designed for dynamic cloud computing. It is allocated to your AWS account and can be reassigned between instances as needed to mask system failures.

### Q64. Why use Elastic IP?

By utilizing an Elastic IP, your resource retains the exact same public-facing IP address even if the underlying instance is stopped, started, rebooted, or completely replaced.

### Q65. Can Elastic IP be reassigned?

Yes. You can programmatically reassociate an Elastic IP from an unhealthy EC2 instance to a healthy standby instance instantly, preventing application downtime for external clients.

---

## Troubleshooting Scenarios

### Q66. EC2 cannot access Internet. What will you check?

To troubleshoot this architecture break, sequentially inspect the following elements:

1. **Route Table:** Ensure a valid route exists directing internet traffic (`0.0.0.0/0`) to either an IGW or a NAT Gateway.
2. **Internet Access Components:** Check that the Internet Gateway is correctly attached to the VPC, or that the target NAT Gateway is live and healthy.
3. **Network Configuration:** Confirm the instance possesses a valid Public IP address (if attempting to communicate via an IGW).
4. **Security Groups & NACLs:** Ensure inbound and outbound configurations explicitly permit the required ports and protocols.

### Q67. VPC Peering exists but communication fails. What will you check?

When a configured peer connection fails to route packets, troubleshoot the following:

1. **Peering Status:** Ensure the peer lifecycle status is actively marked as `Active` (and not pending acceptance).
2. **Route Tables:** Verify that both VPC route tables contain specific lines pointing to the opposing VPC's CIDR block with the target set to the `pcx-xxxxxx` resource ID.
3. **Firewalls:** Ensure security groups and subnet NACLs are not blocking the peer network IP ranges.
4. **CIDRs:** Re-verify that no overlapping CIDR ranges exist between the networks.

### Q68. Private EC2 cannot access S3. What will you check?

If an isolated instance cannot access an S3 bucket, audit the following points:

1. **VPC Endpoint Configuration:** Verify that an S3 Gateway Endpoint exists and is explicitly associated with the subnet's route table.
2. **IAM Policies:** Check that the EC2 Instance Profile has an active IAM policy allowing access to S3.
3. **S3 Bucket Policies:** Ensure the bucket policy does not contain restrictive explicit Deny blocks targeting the VPC's internal components.

### Q69. Security Group allows traffic but still blocked. Why?

If a request is blocked despite an explicit Security Group allow rule, the issue is likely caused by:

1. **NACL Rule Conflict:** A stateless NACL rule at the subnet boundary is explicitly dropping either the incoming traffic or the corresponding outbound return packet.
2. **Local Firewall Rules:** An internal operating-system-level firewall (such as `iptables`, `ufw`, or Windows Firewall) is blocking the ports natively.
3. **Missing Routing Path:** The route table lacks a functional route to deliver the packet back out of the subnet.

### Q70. One AZ fails. How do you maintain application availability?

To withstand an Availability Zone failure, implement a **Multi-AZ Architecture**:

- Distribute compute resources equally across multiple subnets spanning different Availability Zones.
- Use an Application Load Balancer (ALB) to perform continuous health checks and automatically route web traffic away from the degraded zone.
- Enable Auto Scaling Groups to automatically launch replacement instances in the surviving, healthy Availability Zones.
