# Amazon CloudWatch

![AWS](./cloud-watch.png)

**Amazon CloudWatch** is a **monitoring and observability service** provided by AWS that helps you monitor your AWS resources, applications, and infrastructure in real time.

It collects **metrics, logs, events, traces, and performance data** from AWS services and custom applications, allowing you to understand what's happening inside your cloud environment.

> **Simple Definition:**
>
> **CloudWatch is like the health monitor of your AWS infrastructure.**
>
> It continuously tracks your resources and tells you **what is happening, what went wrong, and when you need to take action.**

---

# 🔵 What Does CloudWatch Do?

CloudWatch continuously monitors your AWS resources and applications.

It helps you:

- Monitor resource health
- Track application performance
- Collect application & system logs
- Send alerts when something goes wrong
- Automatically take actions
- Visualize data using dashboards
- Troubleshoot issues quickly
- Monitor custom applications
- Analyze historical performance trends

---

# 🔵 In Simple Terms

Imagine you're managing hundreds of AWS resources.

Without CloudWatch:

- You don't know if CPU reaches 100%
- You don't know if a server crashes
- You don't know if storage becomes full
- You don't know why your application becomes slow

CloudWatch continuously **tracks every important resource** and immediately tells you:

- Is everything healthy?
- Is CPU too high?
- Is memory full?
- Is network traffic increasing?
- Is an application failing?
- Are users getting errors?

Think of it as:

> **CloudWatch = CCTV + Doctor + Security Guard + Performance Analyzer for your AWS Cloud**

It continuously watches your cloud infrastructure.

---

# 🔵 Why Do We Need CloudWatch?

As applications grow,

- More users
- More servers
- More databases
- More containers
- More microservices

It becomes impossible to manually check every resource.

CloudWatch automatically monitors everything for you.

This helps teams:

- Detect problems early
- Reduce downtime
- Improve application performance
- Save operational effort

---

# 🔵 Main Features of CloudWatch

## 1. Metrics Monitoring

CloudWatch collects numerical performance data called **Metrics**.

### Examples

- CPU Utilization
- Memory Usage (custom metric)
- Disk Usage
- Network In/Out
- Request Count
- Error Count
- Latency
- Read/Write Operations

These metrics help understand how resources are performing.

---

## 2. Log Monitoring (CloudWatch Logs)

CloudWatch collects logs from:

- EC2
- Lambda
- ECS
- EKS
- Applications
- Operating Systems
- Custom applications

Developers can search logs to find:

- Errors
- Exceptions
- Failed requests
- Debug information

Instead of logging into multiple servers, all logs are available in one centralized place.

---

## 3. CloudWatch Alarms

CloudWatch can automatically trigger alerts when a metric crosses a threshold.

### Example

If:

```text
CPU > 80%
```

Then:

- Send Email
- Send SMS
- Trigger Lambda
- Auto Scale EC2
- Create Incident
- Notify DevOps Team

This enables proactive monitoring.

### Demo Example

In the demo, I covered the EC2 instance reaching **80% CPU utilization**.

When the CPU utilization exceeded **80%**, a CloudWatch alarm sent an alert message using **Amazon SNS** to the main email ID, and then the appropriate action was taken.

---

## 4. Dashboards

CloudWatch Dashboards display multiple metrics on a single screen.

### Examples

- CPU graphs
- Memory graphs
- Error rates
- Request count
- Database usage
- Network traffic

Operations teams can quickly understand overall system health.

---

## 5. Events / EventBridge Integration

CloudWatch Events (now integrated with Amazon EventBridge) detects AWS events and triggers automated actions.

### Examples

- EC2 Instance Started
- EC2 Stopped
- New File Uploaded to S3
- IAM User Created
- Auto Scaling Event
- Lambda Execution

This enables event-driven automation.

---

## 6. Log Insights

CloudWatch Logs Insights lets you query and analyze log data using a powerful query language.

You can quickly answer questions like:

- Which API is failing the most?
- Which user generated the most errors?
- What happened in the last hour?
- Which server produced the highest number of exceptions?

---

## 7. Application Insights

Automatically monitors applications and detects:

- Slow applications
- Failed services
- Performance bottlenecks
- Infrastructure issues

It also provides recommendations to resolve problems.

---

## 8. Container Insights

Monitors containerized workloads running on:

- Amazon EKS
- Amazon ECS
- Kubernetes

Tracks:

- CPU
- Memory
- Pods
- Nodes
- Containers
- Cluster health

---

## 9. Lambda Insights

Provides deep monitoring for AWS Lambda functions.

Shows:

- Invocation count
- Duration
- Errors
- Cold starts
- Memory usage

---

## 10. Custom Metrics

Applications can publish their own metrics to CloudWatch.

### Example (E-commerce Application)

- Active Users
- Orders per Minute
- Payment Success Rate
- Checkout Failures

These business metrics can also trigger alarms.

---

# 🔵 CloudWatch Workflow

```text
AWS Resources / Applications
            │
            ▼
Collect Metrics, Logs & Events
            │
            ▼
      Amazon CloudWatch
            │
 ┌──────────┼──────────┐
 │          │          │
 ▼          ▼          ▼
Dashboards  Alarms   Log Analysis
 │          │          │
 ▼          ▼          ▼
Visualization Alerts Troubleshooting
            │
            ▼
 Automated Actions (SNS, Lambda,
 Auto Scaling, EventBridge)
```

---

# 🔵 Why is CloudWatch Important in Cloud & DevOps?

One of the biggest reasons organizations move from **On-Premises** infrastructure to the **Cloud** is to gain:

- Scalability
- High availability
- Cost optimization
- Automation
- Reduced operational overhead

However, cloud resources are dynamic—they can be created, scaled, or removed automatically based on demand. Manually monitoring them is impractical.

CloudWatch helps by:

- Monitoring resource utilization in real time
- Tracking application health and performance
- Detecting abnormal behavior early
- Sending alerts before issues become critical
- Triggering automated actions (such as Auto Scaling or Lambda functions)
- Providing visibility into infrastructure and application performance

### Example

Suppose your web application normally handles **1,000 users**, but suddenly traffic increases to **10,000 users**.

CloudWatch detects:

- CPU usage increasing
- Memory utilization rising
- Request latency growing

Based on predefined alarms, it can automatically trigger an **Auto Scaling Group** to launch additional EC2 instances.

When traffic decreases, the extra instances can be terminated, helping maintain performance while optimizing costs.

This enables organizations to **use only the resources they need, when they need them**, improving both reliability and cost efficiency.

---

# 🔵 In Short

> **Amazon CloudWatch is the central monitoring and observability service in AWS. It continuously collects metrics, logs, and events from your infrastructure and applications, helping you monitor resource health, detect issues early, visualize performance, automate responses, and ensure your cloud environment remains reliable, scalable, and cost-efficient.**
