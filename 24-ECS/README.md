# 🔵 Amazon ECS

![aws](./ECS.png)

# 🔵 Introduction to Amazon ECS

Modern applications are no longer deployed as a single application running on one server. Instead, they are divided into multiple small services called **microservices**, and each service is packaged as a **container**.

Managing a few containers manually is easy, but as applications grow, organizations may need to run hundreds or even thousands of containers across multiple servers. Managing these containers manually becomes difficult and error-prone.

This is where **Amazon Elastic Container Service (Amazon ECS)** comes into the picture.

Amazon ECS is a fully managed container orchestration service provided by AWS. It helps you deploy, manage, scale, monitor, and maintain Docker containers without worrying about the underlying infrastructure.

Whether you're deploying a single container or thousands of containers across multiple Availability Zones, Amazon ECS automates the entire process and ensures your applications remain highly available and scalable.

> **Prerequisites**
>
> Before learning Amazon ECS, you should understand the basics of **Docker**, **Containers**, **Docker Images**, and **Amazon ECR**, since ECS uses container images stored in Amazon ECR or another container registry.

---

# 🔵 What is Amazon ECS?

**Amazon Elastic Container Service (Amazon ECS)** is a fully managed container orchestration service provided by AWS that allows you to deploy, manage, scale, and monitor containerized applications.

Think of Amazon ECS as a service that automatically manages the lifecycle of your containers.

Instead of manually starting, stopping, monitoring, or replacing Docker containers, Amazon ECS handles these tasks for you.

It continuously monitors your application and ensures that the desired number of containers are always running.

For example, suppose your application should always run **three containers**.

If one container crashes unexpectedly, Amazon ECS automatically detects the failure and launches a new container without requiring any manual intervention.

This self-healing capability is one of the biggest advantages of Amazon ECS.

> **Simple Definition**
>
> **Amazon ECS is a fully managed container orchestration service that automatically deploys, manages, scales, and monitors Docker containers running on AWS.**

---

# 🔵 Why Do We Need Amazon ECS?

Managing containers manually involves many operational tasks, including:

- Starting containers
- Restarting failed containers
- Scaling applications
- Load balancing
- Health monitoring
- Zero-downtime deployments
- High availability

Performing these tasks manually becomes difficult as applications grow.

**Amazon ECS** automates all of these operations, allowing developers to focus on building applications instead of managing infrastructure.

---

# 🔵 What Problem Does Amazon ECS Solve?

Docker is excellent for creating and running containers, but it doesn't provide enterprise-level container management.

Imagine managing hundreds of containers across multiple EC2 instances.

Questions quickly arise:

- Which server should run the next container?
- What happens if a container crashes?
- How do you scale during high traffic?
- How do you deploy updates without downtime?
- How do you distribute traffic across multiple containers?

Doing all of this manually would be extremely difficult.

**Amazon ECS** solves these problems by acting as a **container orchestrator**. It automates:

- Container scheduling
- Container placement
- Auto Scaling
- Self-healing
- Deployments
- Health monitoring
- Load balancing

---

# 🔵 What is Container Orchestration?

**Container orchestration** is the automated management of containers throughout their entire lifecycle.

Instead of manually managing containers one by one, an orchestration platform performs these tasks automatically.

It manages:

- Deploying containers
- Starting containers
- Stopping containers
- Restarting failed containers
- Auto Scaling
- Scheduling
- Networking
- Service Discovery
- Rolling Updates
- Rollbacks
- Health Monitoring

Amazon ECS is AWS's managed container orchestration platform.

### Example

Imagine a company owns **500 taxis**.

Instead of every driver deciding where to go, a fleet manager automatically assigns drivers, replaces broken taxis, balances workloads, and ensures customers always get a taxi.

Similarly, **Amazon ECS manages hundreds or thousands of containers automatically.**

---

# 🔵 What is a Container Cluster?

A **Cluster** is a logical collection of computing resources where Amazon ECS runs your containers.

The compute resources inside a cluster can be:

- Amazon EC2 instances
- AWS Fargate

Think of a cluster as a workspace where Amazon ECS deploys and manages all of your containers.

A single cluster can run:

- One application
- Multiple applications
- Hundreds of services
- Thousands of containers

### Simple Analogy

```
Apartment Building
        │
        ▼
     Cluster

Apartment
        │
        ▼
   EC2 Instance

Family
        │
        ▼
   Docker Container
```

---

# 🔵 Breaking Down the Term "ECS"

**ECS** stands for:

### Elastic

Automatically scales your containers based on application demand.

### Container

Runs applications packaged together with:

- Application Code
- Libraries
- Dependencies
- Runtime
- Configuration

### Service

Keeps the required number of containers running by automatically replacing failed containers.

Together,

**Amazon ECS** is a fully managed container orchestration service that automates deployment, scaling, monitoring, and availability of containerized applications.

---

# 🔵 Components of Amazon ECS

Amazon ECS consists of several core components that work together to run containerized applications.

| Component              | Description                                                                                                                          |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **Cluster**            | Logical group of compute resources where containers run.                                                                             |
| **Task Definition**    | Blueprint that defines how containers should run, including Docker image, CPU, memory, ports, environment variables, and IAM roles.  |
| **Task**               | A running instance of a Task Definition.                                                                                             |
| **Service**            | Maintains the desired number of running tasks and provides self-healing, load balancing, rolling deployments, and high availability. |
| **Container Instance** | An EC2 instance registered with an ECS Cluster (only for the EC2 launch type).                                                       |

These components work together to automate the deployment, management, monitoring, and scaling of containerized applications.

---

# 🔵 Amazon ECS Architecture Overview

A typical Amazon ECS deployment follows this workflow:

```text
Developer

      │

Build Docker Image

      │

Docker Image

      │

Push Image

      │

Amazon ECR

      │

Amazon ECS Cluster

      │

Task Definition

      │

ECS Service

      │

Running Tasks (Containers)

      │

Application Users
```

### How It Works

1. The developer builds a Docker image.
2. The Docker image is pushed to **Amazon ECR**.
3. Amazon ECS pulls the image from Amazon ECR.
4. A **Task Definition** tells ECS how the container should run.
5. An **ECS Service** launches the required number of tasks.
6. ECS continuously monitors the running tasks.
7. If a task fails, ECS automatically launches a replacement.
8. Users access the running application through a Load Balancer or public endpoint.

This automated workflow enables applications to remain highly available, scalable, and fault tolerant.

---

# 🔵 Amazon ECS vs Running Docker Manually

| Feature              | Docker Only | Amazon ECS           |
| -------------------- | ----------- | -------------------- |
| Run Containers       | ✅          | ✅                   |
| Automatic Restart    | ❌          | ✅                   |
| Auto Scaling         | ❌          | ✅                   |
| Load Balancing       | Manual      | Built-in Integration |
| High Availability    | Manual      | Automatic            |
| Monitoring           | Manual      | AWS Integration      |
| Rolling Deployments  | Manual      | Automatic            |
| Container Scheduling | Manual      | Automatic            |

Docker is responsible for **building and running containers**, while **Amazon ECS** manages those containers automatically in production environments.

---

# 🔵 Amazon ECS vs Kubernetes

Both **Amazon ECS** and **Kubernetes** are container orchestration platforms, but they are designed for different use cases.

| Feature          | Amazon ECS             | Kubernetes               |
| ---------------- | ---------------------- | ------------------------ |
| Managed By       | AWS                    | CNCF                     |
| Learning Curve   | Easy                   | Steep                    |
| AWS Integration  | Excellent              | Good                     |
| Setup Complexity | Simple                 | Complex                  |
| Best For         | AWS-based Applications | Multi-cloud Applications |

### Choose Amazon ECS When

- Your workloads run primarily on AWS.
- You want a simple, fully managed container platform.
- You prefer deep integration with AWS services.

### Choose Kubernetes When

- You need multi-cloud deployments.
- You require advanced orchestration capabilities.
- You need maximum portability across cloud providers.

---

# 🔵 ECS Launch Types

Amazon ECS supports two launch types for running containers.

## EC2 Launch Type

With the EC2 launch type, **you manage the servers**, while Amazon ECS manages container orchestration.

Responsibilities include:

- Managing EC2 instances
- Installing updates
- Capacity planning
- Scaling EC2 instances
- Patching operating systems

This option provides complete control over the infrastructure.

---

## AWS Fargate Launch Type

With AWS Fargate, **AWS manages the infrastructure** for you.

You simply provide:

- Container image
- CPU
- Memory

AWS automatically provisions and manages the underlying servers.

No EC2 management is required.

This is the recommended option for most modern applications.

---

# 🔵 EC2 Launch Type vs AWS Fargate

```text
EC2 Launch Type

You Manage Servers
        │
        ▼
ECS Runs Containers


AWS Fargate

AWS Manages Servers
        │
        ▼
You Only Deploy Containers
```

| Feature                   | EC2                         | Fargate                            |
| ------------------------- | --------------------------- | ---------------------------------- |
| Manage EC2 Instances      | ✅                          | ❌                                 |
| Serverless                | ❌                          | ✅                                 |
| Infrastructure Management | User                        | AWS                                |
| Pay For                   | EC2 Instance                | Running Tasks                      |
| Best For                  | Full Infrastructure Control | Simplicity & Serverless Containers |

---

# 🔵 ECS Capacity Providers

Capacity Providers determine **where Amazon ECS should run your containers**.

Supported Capacity Providers include:

- Amazon EC2
- AWS Fargate
- AWS Fargate Spot

Capacity Providers allow ECS to intelligently distribute workloads while balancing:

- Cost
- Performance
- Availability
- Scalability

---

# 🔵 Deep Dive into Amazon ECS Components

Amazon ECS consists of several core components that work together to deploy, manage, and monitor containerized applications.

Understanding these components is essential before deploying your first ECS application.

---

# 🔵 ECS Cluster

A **Cluster** is a logical group of compute resources where ECS runs your containers.

A cluster can use:

- Amazon EC2
- AWS Fargate

Think of a cluster as the environment where ECS deploys all of your applications.

A single cluster can host:

- Multiple services
- Hundreds of tasks
- Thousands of containers

---

# 🔵 Task Definition

A **Task Definition** is a blueprint that tells Amazon ECS **how to run your containers**.

It defines:

- Docker Image
- CPU
- Memory
- Environment Variables
- Ports
- IAM Roles
- Logging Configuration
- Networking Mode

Every ECS Task is created from a Task Definition.

---

# 🔵 Task

A **Task** is a running instance of a Task Definition.

If your service requires:

```text
Desired Count = 3
```

Amazon ECS creates:

```text
Task 1

Task 2

Task 3
```

Each task runs one or more Docker containers.

---

# 🔵 Service

An **ECS Service** manages running tasks.

Responsibilities include:

- Keeping the desired number of tasks running
- Replacing failed tasks
- Load balancing
- Rolling deployments
- High availability
- Auto Scaling

Without a Service, ECS simply launches a task once.

With a Service, ECS continuously monitors and maintains your application.

---

# 🔵 Container Instance

A **Container Instance** is an EC2 instance that has been registered with an ECS Cluster.

Container Instances are used **only** with the **EC2 Launch Type**.

Each Container Instance runs:

- Docker
- ECS Agent
- Your Containers

Fargate does **not** use Container Instances.

---

# 🔵 ECS Scheduler

The **ECS Scheduler** decides **where containers should run**.

It considers:

- Available CPU
- Available Memory
- Resource Requirements
- Placement Constraints
- Availability Zones

The Scheduler automatically selects the best compute resource for each task.

---

# 🔵 ECS Agent

The **ECS Agent** is software installed on every EC2 Container Instance.

It communicates with the Amazon ECS control plane.

Responsibilities include:

- Starting containers
- Stopping containers
- Monitoring task status
- Reporting health information

Without the ECS Agent, Amazon ECS cannot manage containers running on EC2 instances.

---

# 🔵 IAM Roles

IAM Roles allow Amazon ECS and your applications to securely access AWS services without storing access keys.

### Task Execution Role

Used by ECS to:

- Pull Docker images from Amazon ECR
- Send logs to Amazon CloudWatch

### Task Role

Used by the application running inside the container.

Examples:

- Access Amazon S3
- Read from DynamoDB
- Retrieve secrets from Secrets Manager
- Read parameters from Parameter Store

---

# 🔵 Networking Modes

Amazon ECS supports multiple networking modes.

### Bridge

Containers share Docker's default bridge network.

### Host

Containers use the EC2 instance's network directly.

### awsvpc

Each task receives its own Elastic Network Interface (ENI) and private IP address.

This is the recommended networking mode for AWS Fargate.

### None

Containers have no external network connectivity.

---

# 🔵 ECS with Amazon ECR

Amazon ECS retrieves Docker images from **Amazon Elastic Container Registry (Amazon ECR)** before launching containers.

Typical workflow:

```text
Build Docker Image
        │
        ▼
Push Image to Amazon ECR
        │
        ▼
Create Task Definition
        │
        ▼
Amazon ECS Pulls Image
        │
        ▼
Run Containers
```

---

# 🔵 ECS Deployment Workflow

A typical Amazon ECS deployment consists of the following steps:

1. Build a Docker image.
2. Push the image to Amazon ECR.
3. Create an ECS Task Definition.
4. Create an ECS Service.
5. Amazon ECS launches the required tasks.
6. ECS continuously monitors and replaces failed tasks.

---

# 🔵 ECS Auto Scaling

Amazon ECS supports automatic scaling based on application demand.

When traffic increases:

```text
2 Tasks

↓

5 Tasks

↓

10 Tasks
```

When traffic decreases:

```text
10 Tasks

↓

5 Tasks

↓

2 Tasks
```

This ensures high performance while optimizing costs.

---

# 🔵 High Availability

Amazon ECS distributes tasks across multiple **Availability Zones (AZs)**.

Example:

```text
Availability Zone A

Task 1
Task 2

Availability Zone B

Task 3
Task 4
```

If one Availability Zone becomes unavailable, tasks running in other Availability Zones continue serving users.

---

# 🔵 Health Checks

Amazon ECS continuously monitors task health.

If a container becomes unhealthy:

```text
Task Failed
      │
      ▼
ECS Detects Failure
      │
      ▼
Launch New Task
```

This self-healing capability keeps applications highly available.

---

# 🔵 Logging

Amazon ECS integrates with **Amazon CloudWatch Logs**.

Benefits include:

- Centralized logs
- Easier troubleshooting
- Log retention
- Log monitoring
- Application debugging

---

# 🔵 Monitoring

Amazon ECS integrates with **Amazon CloudWatch** for monitoring.

Common metrics include:

- CPU Utilization
- Memory Utilization
- Running Tasks
- Desired Tasks
- Service Health

These metrics help monitor application performance and scaling.

---

# 🔵 Summary

Amazon ECS is AWS's fully managed container orchestration service that automates the deployment, management, scaling, and monitoring of containerized applications.

Instead of manually managing Docker containers, Amazon ECS ensures that applications remain highly available, scalable, and fault tolerant.

The primary building blocks of Amazon ECS include:

- **Cluster** – Groups the compute resources where containers run.
- **Task Definition** – Blueprint that defines how containers should run.
- **Task** – A running instance of a Task Definition.
- **Service** – Maintains the desired number of running tasks and provides self-healing.
- **Container Instance** – An EC2 instance registered with an ECS Cluster (EC2 Launch Type only).
- **ECS Scheduler** – Decides where tasks should run.
- **ECS Agent** – Communicates between EC2 instances and the ECS control plane.
- **IAM Roles** – Provide secure access to AWS services.
- **Networking** – Controls communication between containers and external services.
- **CloudWatch Integration** – Provides centralized logging and monitoring.

Understanding these components provides the foundation for deploying, managing, scaling, and monitoring containerized applications using Amazon ECS.
