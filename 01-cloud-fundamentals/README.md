# Cloud Fundamentals

![AWS](/aws-cloud.png)

---

### 🔵 What is Cloud Computing

Cloud Computing is the delivery of IT resources (servers, storage, databases, networking, software) over the internet on a pay-as-you-go basis.

**Example:** Launching an AWS EC2 server instead of buying a physical server.

---

### 🔵 Benefits of Cloud

Advantages of using cloud services.

**Benefits:**

- Pay only for what you use
- No hardware management
- Global availability
- Faster deployment
- High scalability
- Better security options
- Automatic backups and recovery

---

### 🔵 IaaS (Infrastructure as a Service)

Provides virtualized infrastructure resources.

**You Manage:** OS, Applications, Data

**Cloud Provider Manages:** Hardware, Networking, Storage

**Examples:**

Amazon Web Services EC2, Microsoft Virtual Machines, Google Compute Engine

---

### 🔵 PaaS (Platform as a Service)

Provides a platform for developing and deploying applications.

**You Manage:** Application code, Data

**Cloud Provider Manages:** Infrastructure, OS, Runtime

**Examples:** AWS Elastic Beanstalk, Azure App Service, Google App Engine

---

### 🔵 SaaS (Software as a Service)

Ready-to-use software delivered over the internet.

**Examples:** Gmail, Microsoft 365, Salesforce

Users simply use the application without managing infrastructure.

---

### 🔵 Public Cloud

Cloud resources are owned and managed by a cloud provider and shared among multiple customers.

**Examples:** AWS, Azure, Google Cloud

**Use Case:** Most startups and enterprises.

---

### 🔵 Private Cloud

Cloud infrastructure dedicated to a single organization.

**Advantages:**

- More control
- Enhanced security
- Compliance requirements

**Example:** Internal company data center.

---

### 🔵 Hybrid Cloud

Combination of public cloud and private cloud.

**Example:**

- Database in private data center
- Application hosted on AWS

**Benefit:** Flexibility and cost optimization.

---

### 🔵 Multi Cloud

Using multiple cloud providers simultaneously.

**Example:**

- AWS for applications
- Azure for analytics
- Google Cloud for AI services

**Benefit:** Avoid vendor lock-in.

---

### 🔵 Shared Responsibility Model

Defines security responsibilities between customer and cloud provider.

**Cloud Provider Secures:**

- Physical data centers
- Hardware
- Networking infrastructure

**Customer Secures:**

- Data
- Applications
- User access
- Configurations

**Remember:**

```
Security OF the Cloud = Provider
Security IN the Cloud = Customer
```

---

### 🔵 High Availability

Ensures applications remain accessible even if a component fails.

**Methods:**

- Multiple servers
- Multiple Availability Zones
- Load Balancers

**Goal:** Minimize downtime.

---

### 🔵 Scalability

Ability to handle increased workload.

**Types:**

- Vertical Scaling (Scale Up)
- Horizontal Scaling (Scale Out)

**Example:**

- Add more CPU/RAM
- Add more servers

---

### 🔵 Elasticity

Automatically adds or removes resources based on demand.

**Example:**

- Traffic increases → Add servers
- Traffic decreases → Remove servers

**Benefit:** Cost savings.

---

### 🔵 Fault Tolerance

System continues operating even when components fail.

**Example:**

- One server crashes
- Another server immediately serves traffic

**Goal:** Zero interruption.

---

### 🔵 Disaster Recovery (DR)

Process of recovering applications and data after major failures.

**Disasters Include:**

- Data center failure
- Natural disasters
- Cyber attacks

**Common DR Strategies:**

- Backup & Restore
- Pilot Light
- Warm Standby
- Multi-Site Active Active

---

### 🔵 Regions

A Region is a geographical area containing multiple AWS data centers.

**Examples:**

- US East (N. Virginia)
- Asia Pacific (Mumbai)
- Europe (Frankfurt)

**Purpose:**

- Low latency
- Data residency
- Disaster recovery

---

### 🔵 Availability Zones (AZs)

An Availability Zone is an isolated data center within a Region.

**Example:**

```
Mumbai Region
 ├── ap-south-1a
 ├── ap-south-1b
 └── ap-south-1c
```

**Benefits:**

- High Availability
- Fault Isolation
- Disaster Recovery

---

### 🔵 Edge Locations

Locations used to deliver content closer to users.

**Used By:**

- Amazon CloudFront

**Benefits:**

- Lower latency
- Faster content delivery
- Better user experience

**Example:**

A user in Chennai receives website content from the nearest edge location instead of a distant AWS region.

---

# 🔵 Virtualization

**Virtualization** is the technology of creating virtual (software-based) versions of physical computing resources such as servers, operating systems, storage, or networks.

Instead of running one operating system on one physical server, virtualization allows multiple virtual machines (VMs) to run on a single physical server.

---

## Why is Virtualization Required?

Before virtualization:

- One application usually required one physical server.
- Many servers used only a small portion of their CPU and memory.
- Hardware costs were high.
- Managing many servers was difficult.

With virtualization:

- Better hardware utilization
- Lower infrastructure cost
- Easier server management
- Faster deployment of new servers
- Better scalability

---

# 🔵 Hypervisor

A **Hypervisor** is software that creates, runs, and manages Virtual Machines (VMs).

It sits between the physical hardware and the virtual machines, allocating CPU, memory, storage, and network resources to each VM.

### Architecture

```
Applications
      │
Operating System
      │
Virtual Machine
      │
---------------------
    Hypervisor
---------------------
Physical Hardware
```

The hypervisor ensures that each VM operates independently, even though they share the same physical hardware.

---

## Responsibilities of a Hypervisor

- Creates Virtual Machines
- Allocates CPU and RAM
- Manages Storage
- Provides Network Connectivity
- Isolates Virtual Machines
- Optimizes Hardware Usage

---

# 🔵  Benefits of Virtualization

- Better Hardware Utilization - Multiple virtual machines share one physical server, reducing unused resources.
- Cost Savings - Fewer physical servers mean lower hardware, power, and cooling costs.
- Faster Provisioning - New virtual machines can be created in minutes instead of purchasing and configuring new hardware.
- Scalability - Virtual machines can be added, removed, or resized easily based on demand.
- Isolation - Each virtual machine runs independently.
- Disaster Recovery - Virtual machines can be backed up, cloned, and restored quickly.
- Easy Testing - Developers can create isolated environments for testing applications without affecting production systems.
- Portability - Virtual machines can be moved between physical servers with minimal downtime.

---

# 🔵 Type 1 vs Type 2 Hypervisor

| Feature     | Type 1 Hypervisor         | Type 2 Hypervisor              |
| ----------- | ------------------------- | ------------------------------ |
| Runs On     | Physical Hardware         | Host Operating System          |
| Performance | High                      | Moderate                       |
| Use Case    | Production, Cloud         | Development, Testing           |
| Security    | More Secure               | Less Secure                    |
| Examples    | VMware ESXi, Hyper-V, KVM | VirtualBox, VMware Workstation |

---

# 🔵  Virtualization vs Containers

| Virtualization         | Containers               |
| ---------------------- | ------------------------ |
| Uses Hypervisor        | Uses Container Runtime   |
| Each VM has its own OS | Containers share Host OS |
| Higher Resource Usage  | Lightweight              |
| Slower Startup         | Starts in Seconds        |
| Strong Isolation       | Process-Level Isolation  |
| Example: VMware        | Example: Docker          |

---

# 🔵 Quick Recap

---

## 🔵 Cloud & Virtualization Concept Matrix

| Concept               | One-Line Definition                          |
| :-------------------- | :------------------------------------------- |
| **Cloud Computing**   | Delivering IT services over the internet     |
| **IaaS**              | Infrastructure provided as a service         |
| **PaaS**              | Platform provided for application deployment |
| **SaaS**              | Ready-to-use software over the internet      |
| **Public Cloud**      | Shared cloud infrastructure                  |
| **Private Cloud**     | Dedicated cloud infrastructure               |
| **Hybrid Cloud**      | Combination of public and private cloud      |
| **Multi Cloud**       | Using multiple cloud providers               |
| **High Availability** | Minimize downtime                            |
| **Scalability**       | Handle increasing workloads                  |
| **Elasticity**        | Auto scale resources                         |
| **Fault Tolerance**   | Continue working after failures              |
| **Disaster Recovery** | Recover from disasters                       |
| **Region**            | Geographic cloud location                    |
| **Availability Zone** | Isolated data center in a region             |
| **Edge Location**     | Content delivery location near users         |

---

## 🔵 Core Engineering Definitions

- **Virtualization** → Running multiple virtual machines on one physical server.
- **Hypervisor** → Software that creates and manages virtual machines.
- **Type 1 Hypervisor** → Runs directly on hardware (best for production).
- **Type 2 Hypervisor** → Runs on top of a host operating system (best for development).

---

## 🔵 Types of Virtualization

- **Server Virtualization** → Multiple VMs on one server.
- **Storage Virtualization** → Combines multiple storage devices into one.
- **Network Virtualization** → Creates virtual networks (e.g., AWS VPC).
- **OS Virtualization** → Uses containers like Docker.

---

## 🔵 Key Takeaways

- **Main Benefits** → Cost savings, better resource utilization, scalability, isolation, high availability, and faster deployment.
