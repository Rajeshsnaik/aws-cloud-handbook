# 🔵 Complete Amazon ECS Practical Demo

Now that you've learned the theory and core components of Amazon ECS, it's time to deploy your first containerized application.

In this practical demo, you'll learn how to:

- Create an Amazon ECR Repository
- Build and push a Docker image to Amazon ECR
- Create an ECS Cluster
- Create a Task Definition
- Run an ECS Task
- Create an ECS Service
- Verify the deployment
- Scale the application
- Update the application
- Monitor the application
- Delete ECS resources

By the end of this demo, you'll understand the complete deployment workflow used in real-world AWS projects.

---

# 🔵 Complete ECS Deployment Workflow

```text
Developer
     │
     ▼
Build Docker Image
     │
     ▼
Amazon ECR
     │
     ▼
Task Definition
     │
     ▼
Amazon ECS Cluster
     │
     ▼
ECS Service
     │
     ▼
Running Containers
     │
     ▼
Application Users
```

---

# 🔵 Step 1: Sign in to the AWS Console

Log in to your **AWS Management Console**.

Navigate to:

```text
Amazon ECS
```

This is where you'll create and manage your ECS resources.

---

# 🔵 Step 2: Create an Amazon ECR Repository

Before deploying containers, you need a repository to store Docker images.

Navigate to:

```text
Amazon ECR
      │
      ▼
Repositories
      │
      ▼
Create Repository
```

Configure the repository:

- Repository Name

```text
demo-ecs-example
```

- Visibility

```text
Private
```

Leave the remaining settings as default.

Click:

```text
Create Repository
```

After the repository is created, copy the **Repository URI**.

Example:

```text
123456789012.dkr.ecr.ap-south-1.amazonaws.com/demo-ecs-example
```

This URI will be used to push your Docker image.

---

# 🔵 Step 3: Create an ECS Cluster

Navigate to:

```text
Amazon ECS
      │
      ▼
Clusters
      │
      ▼
Create Cluster
```

Provide:

```text
Cluster Name

demo-ecs-cluster
```

Choose one of the infrastructure options:

- AWS Fargate
- Amazon EC2

For this demo, select:

```text
AWS Fargate
```

Click:

```text
Create
```

Your ECS Cluster is now ready.

---

# 🔵 Step 4: Build and Push the Docker Image to Amazon ECR

Authenticate Docker with Amazon ECR.

```bash
aws ecr get-login-password \
--region ap-south-1 | docker login \
--username AWS \
--password-stdin <ecr-registry>
```

Build the Docker image.

```bash
docker build -t demo-ecs-example .
```

Tag the Docker image.

```bash
docker tag demo-ecs-example:latest \
<ecr-registry>:latest
```

Push the image.

```bash
docker push <ecr-registry>:latest
```

Example workflow:

```text
Dockerfile
      │
      ▼
docker build
      │
      ▼
Local Docker Image
      │
      ▼
docker tag
      │
      ▼
docker push
      │
      ▼
Amazon ECR
```

Verify that the image appears in the Amazon ECR repository.

---

# 🔵 Step 5: Create a Task Definition

Navigate to:

```text
Amazon ECS
      │
      ▼
Task Definitions
      │
      ▼
Create Task Definition
```

Configure the task.

### General Configuration

```text
Family

demo-ecs-example
```

Launch Type:

```text
AWS Fargate
```

Operating System:

```text
Linux
```

CPU:

```text
0.25 vCPU
```

Memory:

```text
0.5 GB
```

---

### Add Container

Container Name

```text
demo-container
```

Image URI

```text
<ecr-registry>:latest
```

Container Port

```text
3000
```

(Optional)

- Environment Variables
- CloudWatch Logs
- Health Check
- Task Role

Click:

```text
Create
```

The Task Definition acts as a blueprint describing how ECS should run your container.

---

# 🔵 Step 6: Run the ECS Task

Navigate to:

```text
Amazon ECS
      │
      ▼
Task Definitions
      │
      ▼
Select ACTIVE Task Definition
      │
      ▼
Run Task
```

Configure:

Cluster

```text
demo-ecs-cluster
```

Launch Type

```text
AWS Fargate
```

Number of Tasks

```text
1
```

Network

- VPC
- Subnet
- Security Group

Assign Public IP

```text
Enabled
```

Click:

```text
Run Task
```

Amazon ECS now:

- Pulls the Docker image from Amazon ECR
- Creates the container
- Starts the application

---

# 🔵 Step 7: Verify the Deployment

Navigate to:

```text
Amazon ECS
      │
      ▼
Clusters
      │
      ▼
demo-ecs-cluster
      │
      ▼
Tasks
```

Verify:

- Last Status

```text
RUNNING
```

- Health Status

```text
Healthy
```

If CloudWatch Logs are enabled, review the application logs for any startup issues.

---

# 🔵 Step 8: Create an ECS Service

Running individual tasks is useful for temporary workloads.

For production applications, create an **ECS Service**.

Navigate to:

```text
Amazon ECS
      │
      ▼
Clusters
      │
      ▼
demo-ecs-cluster
      │
      ▼
Services
      │
      ▼
Create
```

Configure:

Service Name

```text
demo-service
```

Desired Tasks

```text
2
```

Deployment Type

```text
Rolling Update
```

(Optional)

- Application Load Balancer
- Auto Scaling
- Service Discovery

Click:

```text
Create
```

An ECS Service automatically:

- Maintains the desired number of tasks
- Replaces failed containers
- Supports rolling deployments
- Integrates with Load Balancers
- Supports Auto Scaling

---

# 🔵 Complete Deployment Flow

```text
Developer
      │
      ▼
Create Amazon ECR Repository
      │
      ▼
Build Docker Image
      │
      ▼
Tag Docker Image
      │
      ▼
Push Image to Amazon ECR
      │
      ▼
Create ECS Cluster
      │
      ▼
Create Task Definition
      │
      ▼
Create ECS Service
      │
      ▼
Amazon ECS Pulls Image
      │
      ▼
Launch Containers
      │
      ▼
Application Running
```

---

# 🔵 Scale the Application

Amazon ECS makes horizontal scaling simple.

Navigate to:

```text
Amazon ECS
      │
      ▼
Cluster
      │
      ▼
Service
      │
      ▼
Update
```

Modify:

```text
Desired Count
```

Example:

```text
2 Tasks

↓

5 Tasks

↓

10 Tasks
```

Amazon ECS automatically launches additional tasks.

Similarly, reducing the desired count removes unnecessary tasks.

---

# 🔵 Update the Application

Deploying a new version involves only a few steps.

```text
Modify Source Code
      │
      ▼
Build New Docker Image
      │
      ▼
Push Image to Amazon ECR
      │
      ▼
Create New Task Definition Revision
      │
      ▼
Update ECS Service
      │
      ▼
Rolling Deployment Starts
```

Amazon ECS gradually replaces old containers with the new version while minimizing downtime.

---

# 🔵 Monitor the Application

Amazon ECS integrates with **Amazon CloudWatch**.

Monitor:

- CPU Utilization
- Memory Utilization
- Running Tasks
- Stopped Tasks
- Application Logs
- Container Health

CloudWatch helps troubleshoot issues and monitor application performance.

---

# 🔵 Delete ECS Resources

After completing the demo, delete unused resources to avoid unnecessary AWS charges.

Delete resources in the following order:

```text
Delete ECS Service
        │
        ▼
Stop Running Tasks
        │
        ▼
Delete ECS Cluster
        │
        ▼
Delete Task Definition (Optional)
        │
        ▼
Delete Amazon ECR Repository (Optional)
```

---

# 🔵 Common ECS CLI Commands

List all ECS clusters.

```bash
aws ecs list-clusters
```

Describe a cluster.

```bash
aws ecs describe-clusters \
--clusters my-cluster
```

List running tasks.

```bash
aws ecs list-tasks \
--cluster my-cluster
```

Describe running tasks.

```bash
aws ecs describe-tasks \
--cluster my-cluster \
--tasks <task-id>
```

Update the desired task count.

```bash
aws ecs update-service \
--cluster my-cluster \
--service my-service \
--desired-count 4
```

Run a new task.

```bash
aws ecs run-task
```

Stop a running task.

```bash
aws ecs stop-task
```

Delete an ECS service.

```bash
aws ecs delete-service
```

Delete an ECS cluster.

```bash
aws ecs delete-cluster
```

---

# 🔵 Demo Summary

In this demo, you learned how to:

- Create an Amazon ECR Repository
- Build and push a Docker image
- Create an Amazon ECS Cluster
- Create a Task Definition
- Run an ECS Task
- Create an ECS Service
- Verify the deployment
- Scale the application
- Update the application
- Monitor the application using CloudWatch
- Delete ECS resources

This end-to-end workflow represents the standard process used by organizations to deploy and manage containerized applications on AWS using Amazon ECS.
