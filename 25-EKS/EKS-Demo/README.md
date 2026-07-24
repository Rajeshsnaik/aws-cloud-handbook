# Amazon EKS Practical Deployment Flow

---

# 🔵 Complete Architecture

```text
User
   │
   ▼
Route 53 (example.com)
   │
   ▼
AWS Application Load Balancer (Created Automatically)
   │
   ▼
AWS Load Balancer Controller
   │
   ▼
Ingress
   │
   ▼
Service
   │
   ▼
Pod
   │
   ▼
Application
```

---

# 🔵 Complete Deployment Workflow

Before deploying an application to Amazon EKS, it follows a complete lifecycle.

```text
Write Application Code
        │
        ▼
Build Docker Image
        │
        ▼
Push Docker Image to Amazon ECR
        │
        ▼
Create Amazon EKS Cluster
        │
        ▼
Configure kubectl
        │
        ▼
Create Fargate Profile
(or EC2 Node Group)
        │
        ▼
Install AWS Load Balancer Controller
        │
        ▼
Deploy Kubernetes Resources
(Deployment + Service + Ingress)
        │
        ▼
AWS Application Load Balancer
        │
        ▼
Route 53
        │
        ▼
Users Access the Application
```

---

# 🔵 Prerequisites

Before creating an Amazon EKS cluster, install the following tools.

---

## 1. Docker

Docker is used to build container images for your applications.

Example:

```bash
docker build -t demo-app:v1 .

docker images

docker run demo-app:v1
```

Without Docker, you cannot package your application into a container image.

---

## 2. AWS CLI

AWS CLI is used to communicate with AWS services.

Example:

```bash
aws s3 ls

aws ec2 describe-instances

aws eks list-clusters
```

After installation, configure your AWS credentials.

```bash
aws configure
```

Provide:

```text
Access Key ID

Secret Access Key

Default Region

Output Format
```

---

## 3. kubectl

kubectl is the command-line tool used to communicate with Kubernetes.

Example:

```bash
kubectl get nodes

kubectl get pods

kubectl get svc

kubectl apply -f deployment.yaml
```

Without kubectl, you cannot manage Kubernetes resources.

---

## 4. eksctl

eksctl is the official CLI for creating and managing Amazon EKS clusters.

Instead of manually creating:

- VPC
- IAM Roles
- Security Groups
- EKS Cluster
- Node Groups

eksctl automates the entire process.

Example:

```bash
eksctl create cluster
```

---

## 5. Helm

Helm is the package manager for Kubernetes.

It is commonly used to install:

- AWS Load Balancer Controller
- Metrics Server
- Prometheus
- Grafana
- ArgoCD
- NGINX Ingress Controller

Install Helm before installing the AWS Load Balancer Controller.

---

# 🔵 Step 1 - Create Amazon EKS Cluster

Although you can create an EKS cluster using the AWS Console, most DevOps engineers use **eksctl** because it automatically creates and configures the required AWS resources.

---

## Create an EKS Cluster Using AWS Fargate

```bash
eksctl create cluster \
--name demo-cluster \
--region us-east-1 \
--fargate
```

This command creates:

- Amazon EKS Cluster
- Kubernetes Control Plane
- Default Fargate Profile
- IAM Roles
- Security Groups
- VPC (if not specified)

---

## Delete the Cluster

```bash
eksctl delete cluster \
--name demo-cluster \
--region us-east-1
```

---

# 🔵 Step 2 - Configure kubectl

After creating the cluster, kubectl still doesn't know how to communicate with it.

Update your kubeconfig file.

```bash
aws eks update-kubeconfig \
--name demo-cluster \
--region us-east-1
```

Verify the connection.

```bash
kubectl get nodes
```

If you're using only AWS Fargate, you may not immediately see worker nodes because pods run directly on Fargate instead of EC2 instances.

---

# 🔵 Step 3 - Create Amazon ECR Repository

Amazon ECR stores Docker container images used by Kubernetes Deployments.

---

## Create Repository

```bash
aws ecr create-repository \
--repository-name demo-app
```

---

## Authenticate Docker to Amazon ECR

```bash
aws ecr get-login-password \
--region us-east-1 | docker login \
--username AWS \
--password-stdin <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com
```

---

## Build Docker Image

```bash
docker build -t demo-app:v1 .
```

---

## Tag Docker Image

```bash
docker tag demo-app:v1 \
<ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/demo-app:v1
```

---

## Push Image to Amazon ECR

```bash
docker push \
<ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/demo-app:v1
```

Later, your Kubernetes Deployment will pull this image from Amazon ECR.

---

# 🔵 Step 4 - Create a Fargate Profile

A Fargate Profile tells Amazon EKS which Kubernetes namespaces should run on AWS Fargate.

Example:

```bash
eksctl create fargateprofile \
--cluster demo-cluster \
--region us-east-1 \
--name alb-sample-app \
--namespace game-2048
```

Now every Pod created inside the following namespace will run on AWS Fargate.

```text
game-2048
```

---

# 🔵 Step 5 - Deploy the Application

Deploy the Kubernetes resources.

- Deployment
- Service
- Ingress

Example:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml
```

This creates:

```text
Deployment
      │
      ▼
ReplicaSet
      │
      ▼
Pods
      │
      ▼
Service
      │
      ▼
Ingress
```

---

## Verify the Resources

```bash
kubectl get deployments

kubectl get pods

kubectl get svc

kubectl get ingress
```

---

## Can Users Access the Application?

No.

Although the Ingress resource exists, there is no Ingress Controller running yet.

The Ingress simply stores routing rules inside Kubernetes.

```text
Ingress
      │
      ▼
Routing Rules Stored
      │
      ▼
No AWS ALB Created
      │
      ▼
No External Access
```

---

# 🔵 Step 6 - Install AWS Load Balancer Controller

The AWS Load Balancer Controller continuously watches Kubernetes Ingress resources.

Whenever it detects a valid Ingress resource, it automatically creates an AWS Application Load Balancer (ALB).

---

## Check the OIDC Provider

```bash
aws iam list-open-id-connect-providers
```

If an OIDC provider doesn't exist, associate one with your cluster.

```bash
eksctl utils associate-iam-oidc-provider \
--cluster demo-cluster \
--approve
```

The OIDC provider enables **IAM Roles for Service Accounts (IRSA)**, allowing Kubernetes Pods to securely assume IAM roles without storing AWS credentials.

---

## Download the IAM Policy

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
```

---

## Create the IAM Policy

```bash
aws iam create-policy \
--policy-name AWSLoadBalancerControllerIAMPolicy \
--policy-document file://iam_policy.json
```

This IAM policy grants the AWS Load Balancer Controller permission to:

- Create Application Load Balancers
- Create Target Groups
- Register Targets
- Create Listeners
- Modify Listener Rules
- Delete AWS Load Balancer resources when no longer needed

---

# 🔵 Step 7 - Create IAM Service Account

The AWS Load Balancer Controller runs inside your Kubernetes cluster.

To create and manage AWS resources securely, it requires an IAM Role.

Instead of storing AWS credentials inside Pods, Amazon EKS uses **IAM Roles for Service Accounts (IRSA)**.

Create the IAM Service Account.

```bash
eksctl create iamserviceaccount \
--cluster demo-cluster \
--namespace kube-system \
--name aws-load-balancer-controller \
--role-name AmazonEKSLoadBalancerControllerRole \
--attach-policy-arn arn:aws:iam::<ACCOUNT-ID>:policy/AWSLoadBalancerControllerIAMPolicy \
--approve
```

This creates:

```text
IAM Role
      │
      ▼
Kubernetes Service Account
      │
      ▼
AWS Load Balancer Controller
      │
      ▼
Permissions to Create AWS Resources
```

Now the controller can securely create and manage AWS Application Load Balancers.

---

# 🔵 Step 8 - Install AWS Load Balancer Controller

Add the official EKS Helm repository.

```bash
helm repo add eks https://aws.github.io/eks-charts
```

Update the repository.

```bash
helm repo update
```

Install the AWS Load Balancer Controller.

```bash
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
-n kube-system \
--set clusterName=demo-cluster \
--set serviceAccount.create=false \
--set serviceAccount.name=aws-load-balancer-controller \
--set region=us-east-1 \
--set vpcId=<YOUR-VPC-ID>
```

---

## Verify Installation

```bash
kubectl get deployment -n kube-system aws-load-balancer-controller
```

Expected output:

```text
NAME                             READY

aws-load-balancer-controller      2/2
```

The controller is now running and continuously watching Kubernetes Ingress resources.

---

# 🔵 Step 9 - Automatic ALB Creation

Once the AWS Load Balancer Controller is running, it watches every Ingress resource.

Whenever it finds an Ingress with the **ALB Ingress Class**, it automatically creates AWS networking resources.

Process:

```text
Ingress
      │
      ▼
Reads Ingress Rules
      │
      ▼
Creates Application Load Balancer
      │
      ▼
Creates Target Groups
      │
      ▼
Registers Pod IPs
      │
      ▼
Creates Listener Rules
      │
      ▼
Returns ALB DNS Name
```

---

## Verify the Ingress

```bash
kubectl get ingress
```

Example:

```text
NAME       CLASS   HOSTS   ADDRESS

2048-ing   alb     *       k8s-demo-123456.us-east-1.elb.amazonaws.com
```

You can also verify the ALB from:

```text
AWS Console

↓

EC2

↓

Load Balancers
```

---

# 🔵 Step 10 - Access the Application

Your application is now publicly accessible using the ALB DNS.

Example:

```text
http://k8s-demo-123456.us-east-1.elb.amazonaws.com
```

Although functional, this URL is not user-friendly.

In production, a custom domain is preferred.

---

# 🔵 Step 11 - Configure Route 53

Suppose you own the following domain.

```text
example.com
```

Instead of using the ALB DNS name, users should access:

```text
https://example.com
```

---

## Open Route 53

Navigate to:

```text
AWS Console

↓

Route 53

↓

Hosted Zones

↓

example.com
```

---

## Create an Alias Record

Configure the record as follows.

```text
Record Type

↓

A
```

```text
Alias

↓

Yes
```

```text
Alias Target

↓

Select Your Application Load Balancer
```

Save the record.

---

## Request Flow

```text
example.com
      │
      ▼
Route 53
      │
      ▼
Application Load Balancer
      │
      ▼
Ingress Controller
      │
      ▼
Ingress Rules
      │
      ▼
Service
      │
      ▼
Pods
```

Users can now access the application using:

```text
https://example.com
```

---

# 🔵 Step 12 - Enable HTTPS (Optional)

To secure your application, use **AWS Certificate Manager (ACM)**.

---

## Request an SSL Certificate

Navigate to:

```text
AWS Console

↓

Certificate Manager (ACM)

↓

Request Certificate
```

Enter your domain.

```text
example.com
```

Validate the certificate using DNS validation.

---

## Update the Ingress

Add the following annotations.

```yaml
alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:...
alb.ingress.kubernetes.io/listen-ports: '[{"HTTPS":443}]'
alb.ingress.kubernetes.io/ssl-redirect: "443"
```

Apply the updated Ingress.

```bash
kubectl apply -f ingress.yaml
```

The AWS Load Balancer Controller automatically updates the Application Load Balancer with an HTTPS listener.

---

# 🔵 Final Production Flow

```text
Developer
      │
      ▼
Write Application Code
      │
      ▼
Build Docker Image
      │
      ▼
Push Docker Image
to Amazon ECR
      │
      ▼
Create Amazon EKS Cluster
      │
      ▼
Configure kubectl
      │
      ▼
Create Fargate Profile
(or EC2 Node Group)
      │
      ▼
Install AWS Load Balancer Controller
      │
      ▼
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
      │
      ▼
Deployment
      │
      ▼
ReplicaSet
      │
      ▼
Pods
      │
      ▼
Service (ClusterIP)
      │
      ▼
Ingress
      │
      ▼
AWS Load Balancer Controller
      │
      ▼
Creates Application Load Balancer
      │
      ▼
Target Groups
      │
      ▼
Registers Pod IPs
      │
      ▼
Route 53 Alias Record
      │
      ▼
example.com
      │
      ▼
Internet Users
```

---

# 🔵 Summary

- Amazon EKS provides a managed Kubernetes Control Plane.
- Docker is used to build container images.
- Amazon ECR stores Docker images.
- eksctl simplifies Amazon EKS cluster creation.
- kubectl manages Kubernetes resources.
- Helm installs Kubernetes applications such as the AWS Load Balancer Controller.
- Fargate Profiles determine which namespaces run Pods on AWS Fargate.
- Deployments create ReplicaSets, which manage Pods.
- Services provide a stable network endpoint for Pods.
- Ingress defines HTTP/HTTPS routing rules.
- The AWS Load Balancer Controller watches Ingress resources and automatically creates AWS Application Load Balancers.
- Route 53 maps your custom domain to the Application Load Balancer.
- AWS Certificate Manager (ACM) provides SSL/TLS certificates for HTTPS.
- This architecture is a common production deployment pattern for applications running on Amazon EKS.
