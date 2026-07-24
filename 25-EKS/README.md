# Amazon EKS (Elastic Kubernetes Service)

![eks](./eks.png)

---

# 🔵 What is Amazon EKS?

Amazon **Elastic Kubernetes Service (EKS)** is a fully managed Kubernetes service provided by AWS.

It allows you to run Kubernetes without installing, configuring, or maintaining the Kubernetes Control Plane yourself. AWS manages the Control Plane, while you focus on deploying and managing your applications.

In simple terms:

```text
Kubernetes = Container Orchestration Platform

Amazon EKS = AWS Managed Kubernetes Service
```

---

# 🔵 Why Was Amazon EKS Introduced?

Before Amazon EKS, organizations had to create and manage their own Kubernetes clusters.

A typical production cluster might look like this:

```text
Master-1
Master-2
Master-3

Worker-1
Worker-2
Worker-3
```

To build this cluster, you would install Kubernetes using tools such as:

- kubeadm
- kops
- Kubespray
- RKE2
- K3s
- OpenShift
- Rancher
- Self-managed Kubernetes

After installation, you would configure every Kubernetes component, including:

- kube-apiserver
- etcd
- kube-scheduler
- kube-controller-manager
- CoreDNS
- kube-proxy
- CNI Plugin
- Certificates
- Networking

Initially everything works well.

---

## Problems with Self-Managed Kubernetes

Over time, operational challenges begin to appear.

For example:

```text
Master-2 Down
```

Possible causes include:

- EC2 instance failure
- Disk corruption
- etcd corruption
- Expired certificates
- kube-apiserver failure
- kube-scheduler stopped
- kube-controller-manager failure
- Network issues
- Upgrade failures
- Configuration mistakes
- Storage failures

Now you must investigate questions such as:

- Which master node failed?
- Is etcd healthy?
- Is quorum maintained?
- Are certificates valid?
- Is the API Server responding?
- Why did the scheduler stop?
- Why are controllers not working?
- Is leader election functioning correctly?

Imagine managing:

```text
20 Master Nodes
200 Worker Nodes
```

The operational complexity increases significantly.

You also become responsible for:

- Kubernetes upgrades
- Security patches
- High Availability
- Disaster Recovery
- Certificate rotation
- Control Plane monitoring
- etcd backups
- etcd recovery

Managing all of these tasks requires experienced Kubernetes administrators.

---

# 🔵 How Amazon EKS Solves This

Amazon EKS removes the operational burden of managing the Kubernetes Control Plane.

AWS automatically manages:

- Kubernetes API Server
- etcd
- Scheduler
- Controller Manager
- High Availability
- Control Plane backups
- Certificate management
- Kubernetes version upgrades
- Security patches
- Multi-AZ deployment
- Control Plane monitoring

You never log in to the Kubernetes master nodes because AWS manages them.

---

# 🔵 What Does AWS Manage?

## Control Plane (Managed by AWS)

```text
Kubernetes API Server
          │
          ▼
        etcd
          │
          ▼
     Scheduler
          │
          ▼
Controller Manager
```

AWS automatically deploys these components across multiple Availability Zones for High Availability.

---

## Data Plane (Managed by You)

Unless you choose AWS Fargate, you manage:

- Worker Nodes (EC2)
- Applications
- Pods
- Deployments
- Services
- Storage
- Node scaling
- Node patching

If you choose **AWS Fargate**, AWS also manages the worker infrastructure.

---

# 🔵 Amazon EKS Architecture

```text
                           AWS Cloud
    ┌───────────────────────────────────────────┐
    │         Amazon EKS Control Plane          │
    │-------------------------------------------│
    │ Kubernetes API Server                     │
    │ etcd                                      │
    │ Scheduler                                 │
    │ Controller Manager                        │
    └───────────────────────────────────────────┘
                      │
                      │
        ┌─────────────┴─────────────┐
        │                           │
        ▼                           ▼
  EC2 Worker Nodes            AWS Fargate
        │                           │
        └─────────────┬─────────────┘
                      │
                    Pods
                      │
                Applications
```

---

# 🔵 Compute Options in Amazon EKS

Amazon EKS supports two compute options.

---

## Option 1 - EC2 Worker Nodes

You provision and manage EC2 instances.

You are responsible for:

- Operating System updates
- Node patching
- AMI updates
- Node scaling
- Node security

AWS manages only the Kubernetes Control Plane.

---

## Option 2 - AWS Fargate

AWS manages both the Kubernetes Control Plane and the compute infrastructure for your Pods.

You only manage:

- Kubernetes manifests
- Applications
- Containers

Benefits:

- No EC2 instances
- No SSH access
- No node maintenance
- No capacity planning

---

# 🔵 Ways to Run Kubernetes

| Method     | Description                                                                      |
| ---------- | -------------------------------------------------------------------------------- |
| kubeadm    | Official tool for manually creating Kubernetes clusters.                         |
| kops       | Automates Kubernetes cluster creation on AWS while you manage the Control Plane. |
| Kubespray  | Uses Ansible to deploy production-ready Kubernetes clusters.                     |
| RKE2       | Rancher's enterprise-ready Kubernetes distribution.                              |
| K3s        | Lightweight Kubernetes for Edge, IoT, and development.                           |
| OpenShift  | Enterprise Kubernetes platform from Red Hat.                                     |
| Amazon EKS | AWS-managed Kubernetes Control Plane.                                            |
| Google GKE | Google-managed Kubernetes service.                                               |
| Azure AKS  | Microsoft-managed Kubernetes service.                                            |

---

# 🔵 Kubernetes Networking Example

Consider the following cluster:

```text
3 Master Nodes

2 Worker Nodes
```

Applications run inside Pods.

```text
Pod
 │
 ▼
Application
```

Every Pod receives its own IP address.

However:

- Pod IPs are temporary.
- Pod IPs change when Pods are recreated.
- Users should not access Pods directly.

Instead, Kubernetes uses **Services**.

---

# 🔵 Kubernetes Service

A Kubernetes Service provides a stable network endpoint for one or more Pods.

```text
Pods
  │
  ▼
Service
  │
  ▼
Stable IP Address
```

Services automatically load balance traffic across healthy Pods.

---

# 🔵 Types of Kubernetes Services

## 1. ClusterIP (Default)

Provides internal communication within the Kubernetes cluster.

```text
Frontend Pod
      │
      ▼
Backend Service
      │
      ▼
Backend Pods
```

### Characteristics

- Internal access only
- Default Service type
- Most commonly used for microservices communication

---

## 2. NodePort

Exposes the application through a port on every worker node.

```text
Client
   │
   ▼
Node IP:30080
   │
   ▼
Service
   │
   ▼
Pods
```

### Characteristics

- Accessible using `<Node-IP>:Port`
- Useful for testing
- Not recommended for exposing production applications directly

---

## 3. LoadBalancer

Creates an external cloud load balancer.

On AWS, Kubernetes provisions an Elastic Load Balancer (ELB) based on your configuration.

```text
Internet
    │
    ▼
AWS Load Balancer
    │
    ▼
Kubernetes Service
    │
    ▼
Pods
```

### Advantages

- Publicly accessible
- Highly available
- Automatically managed by AWS

### Disadvantages

- Typically creates one AWS Load Balancer per Service
- Costs can increase if many Services require external access

---

# 🔵 Summary

Amazon EKS provides a fully managed Kubernetes Control Plane, allowing you to focus on deploying applications instead of managing Kubernetes infrastructure.

AWS manages:

- Kubernetes Control Plane
- High Availability
- Multi-AZ deployment
- Upgrades
- Security patches
- Certificate management
- Control Plane monitoring

You manage:

- Applications
- Containers
- Kubernetes manifests
- Worker Nodes (EC2) or simply Pods when using AWS Fargate

Amazon EKS combines the flexibility of Kubernetes with the operational simplicity of a managed AWS service.

---

# 🔵 Why Use Ingress?

Suppose you have multiple applications running in your Kubernetes cluster.

```text
Service A

Service B

Service C
```

If every Service is of type **LoadBalancer**, AWS creates a separate Load Balancer for each Service.

This leads to:

- Higher AWS costs
- More Load Balancers to manage
- Increased operational complexity

Instead, we use **Ingress**.

Ingress provides a **single external entry point** and routes traffic to multiple Services based on rules such as URL paths or hostnames.

---

# 🔵 What is Ingress?

An **Ingress** is a Kubernetes API object that defines **HTTP/HTTPS routing rules** for incoming traffic.

For example:

```text
example.com/shop
        │
        ▼
 Shop Service
        │
        ▼
   Shop Pods
```

```text
example.com/payment
        │
        ▼
Payment Service
        │
        ▼
 Payment Pods
```

```text
example.com/profile
        │
        ▼
Profile Service
        │
        ▼
 Profile Pods
```

These routing rules are defined in an **ingress.yaml** file.

Overall request flow:

```text
Internet
    │
    ▼
Ingress
    │
    ▼
Service
    │
    ▼
Pods
```

---

# 🔵 Does Ingress Work Automatically?

No.

When you create an Ingress resource using:

```bash
kubectl apply -f ingress.yaml
```

Kubernetes simply stores the routing rules.

It does **not** automatically route traffic.

To make Ingress work, you must install an **Ingress Controller**.

---

# 🔵 What is an Ingress Controller?

An **Ingress Controller** watches Ingress resources and configures a real proxy or load balancer to route traffic based on the Ingress rules.

Popular Ingress Controllers include:

- NGINX Ingress Controller
- AWS Load Balancer Controller
- Traefik
- HAProxy Ingress
- Kong

Without an Ingress Controller:

```text
Ingress YAML
      │
      ▼
Nothing Happens
```

With an Ingress Controller:

```text
Ingress YAML
      │
      ▼
Ingress Controller
      │
      ▼
Traffic Starts Routing
```

---

# 🔵 NGINX Ingress Controller

The **NGINX Ingress Controller** runs inside the Kubernetes cluster as a reverse proxy.

Request flow:

```text
Internet
    │
    ▼
AWS Load Balancer (or NodePort)
    │
    ▼
NGINX Ingress Controller
    │
    ▼
Kubernetes Service
    │
    ▼
Pods
```

The NGINX Ingress Controller reads the Ingress rules and forwards requests to the correct Kubernetes Service.

> **Important:** The NGINX Ingress Controller is **not** an AWS Application Load Balancer (ALB). It is an NGINX-based reverse proxy running inside your Kubernetes cluster. On AWS, it is commonly exposed using a Service of type **LoadBalancer**, which creates an AWS Load Balancer in front of the NGINX pods.

---

# 🔵 AWS Load Balancer Controller

AWS provides the **AWS Load Balancer Controller**, which integrates Kubernetes with AWS Elastic Load Balancing.

Request flow:

```text
Internet
    │
    ▼
AWS Application Load Balancer (ALB)
    │
    ▼
Ingress Rules
    │
    ▼
Kubernetes Service
    │
    ▼
Pods
```

The controller automatically:

- Creates Application Load Balancers (ALB)
- Creates Network Load Balancers (NLB)
- Configures listeners and listener rules
- Creates Target Groups
- Registers and deregisters Kubernetes Pods

This is the recommended approach for many production Amazon EKS deployments.

---

# 🔵 Public and Private Subnets

A typical production Amazon EKS architecture uses public and private subnets.

```text
Internet
    │
    ▼
Public Subnet
    │
    ▼
AWS Application Load Balancer
    │
    ▼
Private Subnets
    │
    ▼
Worker Nodes
    │
    ▼
Pods
```

Benefits:

- Worker nodes remain private.
- Pods are not directly exposed to the internet.
- Only the Load Balancer is publicly accessible.

This improves both security and scalability.

---

# 🔵 What is an IngressClass?

A Kubernetes cluster can have multiple Ingress Controllers.

For example:

- NGINX Ingress Controller
- AWS Load Balancer Controller
- Traefik

Kubernetes needs to know which controller should process a particular Ingress resource.

This is done using an **IngressClass**.

Example:

```yaml
spec:
  ingressClassName: nginx
```

or

```yaml
spec:
  ingressClassName: alb
```

Each Ingress Controller watches only the Ingress resources that match its IngressClass.

---

# 🔵 Complete Request Flow in Amazon EKS

```text
User
    │
    ▼
Route 53 (DNS)
    │
    ▼
AWS Application Load Balancer
(or AWS Load Balancer in front of NGINX)
    │
    ▼
Ingress Controller
    │
    ▼
Ingress Rules
    │
    ▼
Kubernetes Service
    │
    ▼
Pod
    │
    ▼
Application
```

---

# 🔵 When to Use EC2-based EKS?

Use **EC2 Worker Nodes** when:

- You want full control over the worker nodes.
- Your application runs continuously (24/7).
- You have many long-running applications.
- You want lower cost for large workloads.
- You need custom operating system or node configurations.

**Simple sentence:**

> **Choose EC2 when you're comfortable managing servers and want greater control with lower cost for long-running workloads.**

---

# 🔵 When to Use AWS Fargate?

Use **AWS Fargate** when:

- You don't want to manage servers.
- You only want to deploy applications.
- Your workloads are small or run occasionally.
- You want the simplest Kubernetes experience.
- You want AWS to manage the compute infrastructure.

**Simple sentence:**

> **Choose Fargate when you want AWS to manage the servers so you can focus entirely on your applications.**

---

# 🔵 Easy Way to Remember

```text
EC2 = "I manage the servers."

Fargate = "AWS manages the servers."
```

---

# 🔵 Summary

- **Amazon EKS** is AWS's managed Kubernetes service.
- AWS manages the **Control Plane** (API Server, etcd, Scheduler, and Controller Manager).
- You manage the **Data Plane** (EC2 worker nodes and applications), or let AWS manage compute with **AWS Fargate**.
- Kubernetes **Services** provide stable network endpoints for Pods.
- **ClusterIP** is for internal communication, **NodePort** exposes a port on each worker node, and **LoadBalancer** provisions an AWS Load Balancer.
- **Ingress** provides a single entry point and defines HTTP/HTTPS routing rules for multiple Services.
- **Ingress** requires an **Ingress Controller** to process routing rules and direct traffic.
- **NGINX Ingress Controller** is an in-cluster reverse proxy, while the **AWS Load Balancer Controller** creates and manages AWS Application Load Balancers (ALBs) and Network Load Balancers (NLBs).
- **IngressClass** specifies which Ingress Controller should process an Ingress resource.
- A production Amazon EKS deployment commonly places the **Application Load Balancer in public subnets** and the **worker nodes and Pods in private subnets** for improved security and scalability.
