# ☁️ Cloud Fundamentals

![Cloud Fundamentals](./cloud-computing.png)

Cloud Fundamentals form the foundation of every cloud platform, including AWS, Azure, and Google Cloud. Understanding these concepts is essential before learning services such as IAM, VPC, EC2, S3, RDS, Lambda, EKS, and Terraform.

---

````md
# 🔵 What is Cloud Computing?

Cloud Computing is the delivery of computing resources such as servers, storage, databases, networking, and software over the internet instead of managing physical infrastructure on-premises.

Organizations can access resources on demand and pay only for what they use without purchasing and maintaining their own hardware.

### Interview Answer

Cloud Computing is a model that provides IT resources over the internet on a pay-as-you-go basis, allowing businesses to use servers, storage, databases, and applications without managing physical infrastructure.

---

# 🔵 Benefits of Cloud

Cloud computing helps organizations reduce infrastructure costs and increase agility.

Key benefits include:

- Pay only for what you use
- Faster deployment of applications
- Automatic scaling
- High availability
- Global accessibility
- Improved security and reliability
- Reduced infrastructure management

### Interview Answer

The main benefits of cloud computing are cost savings, scalability, high availability, faster deployment, and the ability to access resources on demand without managing physical hardware.

---

# 🔵 IaaS (Infrastructure as a Service)

IaaS provides basic infrastructure resources such as virtual machines, storage, networking, and operating systems.

The cloud provider manages the physical hardware, while the customer manages the operating system, applications, and data.

### Examples

- Amazon EC2
- Amazon EBS
- Google Compute Engine
- Microsoft Azure Virtual Machines

### Interview Answer

IaaS provides virtualized infrastructure resources such as servers, storage, and networking. The customer manages the operating system and applications, while the cloud provider manages the underlying hardware.

---

# 🔵 PaaS (Platform as a Service)

PaaS provides a complete platform for developing, testing, and deploying applications.

The cloud provider manages the infrastructure, operating system, middleware, and runtime environment.

### Examples

- AWS Elastic Beanstalk
- Google App Engine
- Azure App Service

### Interview Answer

PaaS provides a managed platform where developers can build and deploy applications without worrying about infrastructure or operating system management.

---

# 🔵 SaaS (Software as a Service)

SaaS delivers fully functional software applications over the internet.

Users simply access the application through a browser without managing infrastructure or installation.

### Examples

- Gmail
- Microsoft 365
- Salesforce
- Zoom

### Interview Answer

SaaS is a software delivery model where users access applications over the internet while the provider manages everything including infrastructure, maintenance, and updates.

---

# 🔵 Public Cloud

A public cloud is owned and operated by a third-party cloud provider and shared among multiple customers.

Resources are accessible over the internet.

### Examples

- AWS
- Microsoft Azure
- Google Cloud Platform

### Interview Answer

A public cloud is a cloud environment where computing resources are owned and managed by a cloud provider and shared among multiple customers.

---

# 🔵 Private Cloud

A private cloud is dedicated to a single organization and is not shared with other customers.

It provides greater control, customization, and security.

### Interview Answer

A private cloud is a cloud infrastructure used exclusively by a single organization, providing higher control and security compared to a public cloud.

---

# 🔵 Hybrid Cloud

Hybrid cloud combines public cloud and private cloud environments.

Applications and data can move between both environments based on business requirements.

### Example

Sensitive data remains in a private cloud while customer-facing applications run in a public cloud.

### Interview Answer

A hybrid cloud combines private and public cloud environments, allowing organizations to use the advantages of both while maintaining flexibility and control.

---

# 🔵 Multi Cloud

Multi-cloud means using cloud services from multiple cloud providers.

For example, an organization may use AWS for compute services and Google Cloud for analytics.

### Interview Answer

Multi-cloud is a strategy where an organization uses services from multiple cloud providers to avoid vendor lock-in and improve reliability.

---

# 🔵 Shared Responsibility Model

In cloud computing, security responsibilities are shared between the cloud provider and the customer.

### Cloud Provider Responsibilities

- Physical security
- Hardware
- Networking infrastructure
- Data center security

### Customer Responsibilities

- Application security
- User access management
- Data protection
- Operating system configuration (depending on service type)

### Interview Answer

The Shared Responsibility Model defines which security responsibilities belong to the cloud provider and which belong to the customer.

---

# 🔵 High Availability

High Availability means ensuring that applications remain accessible with minimal downtime.

This is achieved by distributing resources across multiple servers or Availability Zones.

### Interview Answer

High Availability is the ability of a system to remain operational and accessible even if some components fail.

---

# 🔵 Scalability

Scalability is the ability of a system to handle increasing workloads by adding resources.

### Example

A website receiving more users can add additional servers to handle the traffic.

### Interview Answer

Scalability is the ability of a system to increase or decrease resources to handle changes in workload.

---

# 🔵 Elasticity

Elasticity is the automatic adjustment of resources based on demand.

Resources are added during high traffic and removed when demand decreases.

### Interview Answer

Elasticity is the ability of a cloud system to automatically scale resources up or down based on workload requirements.

---

# 🔵 Fault Tolerance

Fault Tolerance is the ability of a system to continue operating even when a component fails.

Redundant components ensure uninterrupted service.

### Example

If one server fails, another server immediately takes over.

### Interview Answer

Fault Tolerance is the capability of a system to continue functioning without interruption despite hardware or software failures.

---

# 🔵 Disaster Recovery

Disaster Recovery is the process of restoring applications and data after a major failure or disaster.

Examples include backups, replication, and failover mechanisms.

### Interview Answer

Disaster Recovery is a strategy used to recover systems, applications, and data after unexpected failures such as natural disasters, cyberattacks, or infrastructure outages.

---

# 🔵 Regions

A Region is a geographical area where a cloud provider operates data centers.

Each region is isolated from other regions.

### Example

- Mumbai Region
- Singapore Region
- Frankfurt Region

### Interview Answer

A Region is a physical geographic location containing multiple Availability Zones where cloud resources are deployed.

---

# 🔵 Availability Zones

Availability Zones (AZs) are isolated data centers within a region.

Each Availability Zone has independent power, networking, and cooling systems.

### Example

```text
Mumbai Region
 ├── AZ-1
 ├── AZ-2
 └── AZ-3
```

### Interview Answer

An Availability Zone is an isolated data center within a region designed to provide fault tolerance and high availability.

---

# 🔵 Edge Locations

Edge Locations are locations used to cache content closer to users.

They help reduce latency and improve application performance.

### Example

Used by CloudFront CDN.

When a user requests content, it is served from the nearest Edge Location instead of the origin server.

### Interview Answer

Edge Locations are distributed locations that cache content closer to end users, reducing latency and improving delivery speed.
````

````md
# 🔵 On-Premises vs Cloud

Before cloud computing became popular, organizations hosted applications in their own data centers. They purchased servers, networking equipment, storage devices, and were responsible for maintaining everything themselves. This model is known as **On-Premises Infrastructure**.

Cloud computing changed this approach by allowing organizations to rent infrastructure from cloud providers such as AWS, Azure, and Google Cloud instead of owning and managing physical hardware.

## On-Premises Challenges

- High upfront hardware investment
- Infrastructure maintenance responsibility
- Limited scalability
- Complex disaster recovery planning
- Hardware replacement costs
- Dedicated operations teams required

## Cloud Benefits Over On-Premises

- No upfront hardware purchase
- Pay only for resources used
- Easy scalability
- Global infrastructure availability
- Faster deployment
- Managed services

### Interview Answer

On-Premises infrastructure is hosted and managed by the organization within its own data center, whereas Cloud infrastructure is hosted and managed by cloud providers such as AWS, Azure, or GCP and is available on demand over the internet.

---

# 🔵 Capital Expenditure (CapEx) vs Operational Expenditure (OpEx)

One of the biggest advantages of cloud computing is the shift from Capital Expenditure to Operational Expenditure.

## Capital Expenditure (CapEx)

CapEx refers to large upfront investments made to purchase and own infrastructure.

### Examples

- Buying physical servers
- Building data centers
- Purchasing networking devices
- Storage systems
- Backup hardware

### Characteristics

- Large initial investment
- Long-term ownership
- Hardware depreciation
- Maintenance responsibility

## Operational Expenditure (OpEx)

OpEx refers to ongoing expenses based on actual resource usage.

### Examples

- AWS EC2 hourly usage
- S3 storage charges
- Database consumption costs
- Data transfer costs

### Characteristics

- No upfront investment
- Pay-as-you-go pricing
- Flexible scaling
- Lower financial risk

### Interview Answer

CapEx involves purchasing and owning infrastructure with a large upfront investment, while OpEx follows a pay-as-you-go model where organizations pay only for the resources they consume.

---

# 🔵 Pay-As-You-Go Model

Cloud providers charge customers only for the resources they use instead of requiring long-term infrastructure investments.

This model allows organizations to optimize costs and avoid paying for unused resources.

## Examples

- Storage consumed in S3
- Compute hours used in EC2
- Database usage
- Data transfer
- API requests

### Benefits

- Cost optimization
- No upfront investment
- Easy experimentation
- Better budgeting

### Interview Answer

Pay-As-You-Go is a cloud pricing model where customers are charged only for the resources they consume rather than purchasing infrastructure upfront.

---

# 🔵 Cloud Migration

Cloud Migration is the process of moving applications, databases, workloads, and infrastructure from on-premises environments to cloud platforms.

Organizations migrate to the cloud to improve scalability, reduce costs, increase reliability, and simplify infrastructure management.

## Migration Targets

- Applications
- Databases
- Storage
- Servers
- Networking

### Benefits

- Reduced infrastructure costs
- Improved scalability
- Better availability
- Faster deployment cycles

### Interview Answer

Cloud Migration is the process of moving applications, data, and infrastructure from traditional on-premises environments to cloud platforms such as AWS, Azure, or Google Cloud.

---

# 🔵 6 R's of Cloud Migration

The 6 R's are strategies used to determine how an application should be migrated to the cloud.

## Rehost (Lift and Shift)

Move the application to the cloud without making any code changes.

### Example

Moving a Java application from a physical server directly to an EC2 instance.

---

## Replatform

Make small optimizations while migrating.

### Example

Moving an application to EC2 while changing the database to Amazon RDS.

---

## Refactor / Re-Architect

Redesign the application to fully utilize cloud-native services.

### Example

Breaking a monolithic application into microservices.

---

## Repurchase

Replace the existing application with a SaaS solution.

### Example

Replacing a custom CRM with Salesforce.

---

## Retire

Remove applications that are no longer required.

### Example

Decommissioning legacy software.

---

## Retain

Keep specific workloads on-premises.

### Example

Applications with regulatory or compliance requirements.

### Interview Answer

The 6 R's are migration strategies used to decide how workloads should be moved to the cloud: Rehost, Replatform, Refactor, Repurchase, Retire, and Retain.

---

# 🔵 Cloud Security

Cloud Security involves protecting cloud resources, applications, and data from unauthorized access, attacks, and data breaches.

Security remains one of the most important responsibilities in cloud environments.

## Key Areas

### Identity Management

Controls who can access resources.

### Encryption

Protects sensitive data.

### Network Security

Secures communication between systems.

### Monitoring

Detects suspicious activities and threats.

### Compliance

Ensures regulatory requirements are met.

### Interview Answer

Cloud Security refers to the practices, technologies, and policies used to protect cloud infrastructure, applications, and data from security threats and unauthorized access.

---

# 🔵 Identity and Access Management (IAM)

IAM is used to manage users, groups, roles, and permissions within a cloud environment.

It ensures that only authorized users and services can access resources.

## IAM Controls

- Who can access resources
- What actions can be performed
- Which services can be used

### Example

A developer can access EC2 instances but cannot delete S3 buckets.

### Interview Answer

IAM is a security service used to control authentication and authorization for users, groups, applications, and cloud resources.

---

# 🔵 Authentication vs Authorization

These are two fundamental security concepts often asked in interviews.

## Authentication

Authentication verifies identity.

### Question

Who are you?

### Examples

- Username and Password
- Multi-Factor Authentication (MFA)
- Fingerprint Authentication

---

## Authorization

Authorization determines permissions after identity verification.

### Question

What are you allowed to access?

### Examples

- Read S3 Bucket
- Create EC2 Instance
- Deny Database Deletion

### Interview Answer

Authentication verifies a user's identity, while Authorization determines what resources and actions the authenticated user is allowed to access.

---

# 🔵 Encryption

Encryption protects data by converting readable information into an unreadable format.

Only authorized users with the correct key can decrypt and access the data.

## Data at Rest

Data stored on disks or storage systems.

### Examples

- S3 Objects
- Database Records
- EBS Volumes

---

## Data in Transit

Data moving across networks.

### Examples

- HTTPS
- TLS Connections
- API Communication

### Interview Answer

Encryption converts readable data into an unreadable format to protect it from unauthorized access both during storage and transmission.

---

# 🔵 Load Balancing

Load Balancing distributes incoming traffic across multiple servers or resources.

Instead of sending all requests to a single server, requests are spread evenly to improve performance and reliability.

## Benefits

- High Availability
- Better Performance
- Fault Tolerance
- Improved User Experience

### Example

If one server fails, traffic is automatically routed to healthy servers.

### Interview Answer

Load Balancing distributes incoming traffic across multiple resources to improve availability, reliability, and application performance.

---

# 🔵 CDN (Content Delivery Network)

A CDN is a globally distributed network of servers that caches content closer to users.

Instead of fetching content from the origin server every time, users receive content from the nearest edge location.

## Example

AWS CloudFront

## Benefits

- Faster content delivery
- Lower latency
- Reduced server load
- Improved user experience

### Interview Answer

A CDN is a distributed network that caches content closer to users to reduce latency and improve application performance.

---

# 🔵 Vertical Scaling vs Horizontal Scaling

Scaling is the process of increasing system capacity to handle additional workload.

## Vertical Scaling (Scale Up)

Increase the resources of a single server.

### Example

```text
2 CPU  →  8 CPU
8 GB RAM → 32 GB RAM
```

### Advantages

- Easy implementation
- No architecture changes

### Limitations

- Hardware limits exist

---

## Horizontal Scaling (Scale Out)

Increase capacity by adding more servers.

### Example

```text
1 Server → 5 Servers
```

### Advantages

- Better availability
- Better fault tolerance
- Virtually unlimited scaling

### Interview Answer

Vertical Scaling increases the capacity of a single server, whereas Horizontal Scaling increases capacity by adding more servers.

---

# 🔵 RTO and RPO

These are critical Disaster Recovery concepts.

## RTO (Recovery Time Objective)

Defines the maximum acceptable downtime after a failure.

### Example

```text
RTO = 30 Minutes
```

The system must be restored within 30 minutes.

---

## RPO (Recovery Point Objective)

Defines the maximum acceptable amount of data loss.

### Example

```text
RPO = 5 Minutes
```

Only 5 minutes of data loss is acceptable.

### Interview Answer

RTO defines how quickly systems must recover after a disaster, while RPO defines the maximum acceptable amount of data loss.

---

# 🔵 SLA (Service Level Agreement)

An SLA is a formal agreement between a service provider and customer that defines expected service availability and performance.

## Example

```text
99.99% Availability
```

This means the service can experience only a limited amount of downtime annually.

### Why SLA Matters

- Defines expectations
- Measures service quality
- Provides accountability

### Interview Answer

An SLA is a commitment made by a cloud provider regarding service availability, performance, and reliability.

---

# 🔵 Cloud Native Applications

Cloud-Native Applications are designed specifically to take advantage of cloud environments.

They are built for scalability, resilience, automation, and rapid deployment.

## Characteristics

- Microservices Architecture
- Containers
- Auto Scaling
- API-Driven Design
- DevOps Integration

### Benefits

- Faster deployments
- Better scalability
- Improved fault tolerance

### Interview Answer

Cloud-Native Applications are applications designed to fully utilize cloud capabilities such as scalability, resilience, automation, and distributed architecture.

---

# 🔵 Monolithic vs Microservices

## Monolithic Architecture

All application components exist within a single codebase and deployment unit.

### Characteristics

- Single deployment
- Tightly coupled components
- Easier initial development

---

## Microservices Architecture

Application functionality is split into multiple independent services.

### Characteristics

- Independent deployment
- Independent scaling
- Better fault isolation

### Example

```text
User Service
Order Service
Payment Service
Notification Service
```

### Interview Answer

A Monolithic application contains all functionality within a single application, whereas a Microservices architecture divides functionality into independent services that can be developed, deployed, and scaled separately.

---

# 🔵 Serverless Computing

Serverless Computing allows developers to run applications without managing servers.

The cloud provider automatically handles infrastructure provisioning, scaling, maintenance, and availability.

## Examples

- AWS Lambda
- Azure Functions
- Google Cloud Functions

## Benefits

- No server management
- Automatic scaling
- Pay only for usage

### Interview Answer

Serverless Computing is a cloud execution model where developers run code without managing servers, while the cloud provider handles infrastructure and scaling.

---

# 🔵 Containerization

Containerization packages an application and all its dependencies into a lightweight, portable unit called a container.

Containers ensure applications run consistently across different environments.

## Popular Tools

- Docker
- Podman

## Benefits

- Portability
- Faster deployment
- Consistent environments
- Efficient resource utilization

### Interview Answer

Containerization packages applications and their dependencies into lightweight, portable containers that can run consistently across different environments.

---

# V🔵 irtualization

Virtualization allows multiple virtual machines to run on a single physical server.

A software layer called a Hypervisor creates and manages virtual machines.

## Benefits

- Better hardware utilization
- Reduced costs
- Isolation between workloads

### Example

```text
Physical Server
      ↓
Hypervisor
      ↓
VM1
VM2
VM3
```

### Interview Answer

Virtualization is the technology that enables multiple virtual machines to run on a single physical server by abstracting underlying hardware resources.
````
