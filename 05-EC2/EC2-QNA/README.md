# EC2 Interview Questions and Answers

This section contains some of the most commonly asked Amazon EC2 interview questions, along with concise and practical answers. These questions cover core EC2 concepts, storage, networking, troubleshooting, security, scaling, and production best practices.

---

# 🔵 What is EC2?

Amazon EC2 (Elastic Compute Cloud) is a virtual server service provided by AWS that allows you to create, manage, and scale virtual machines in the cloud. It is commonly used to host websites, web applications, APIs, databases, and backend services without purchasing physical hardware.

---

# 🔵 What is AMI?

An **Amazon Machine Image (AMI)** is a pre-configured template that contains the operating system, software, application configurations, and storage settings required to launch an EC2 instance.

---

# 🔵 Difference Between AMI and Snapshot

| AMI                                                  | Snapshot                        |
| ---------------------------------------------------- | ------------------------------- |
| Used to launch EC2 instances                         | Used to back up EBS volumes     |
| Contains operating system and instance configuration | Contains only volume data       |
| Can create identical EC2 instances                   | Used to restore storage volumes |
| Includes one or more EBS snapshots                   | Stores block-level data only    |

---

# 🔵 What is EBS?

Amazon EBS (Elastic Block Store) is persistent block storage for EC2 instances. It behaves like a virtual hard disk and stores operating systems, applications, databases, and user data. The data remains available even if the EC2 instance is stopped.

---

# 🔵 Difference Between EBS and Instance Store

| EBS                            | Instance Store                    |
| ------------------------------ | --------------------------------- |
| Persistent storage             | Temporary storage                 |
| Supports snapshots             | Snapshot not supported            |
| Network-attached storage       | Physically attached storage       |
| Can be detached and reattached | Cannot be detached                |
| Best for production workloads  | Best for cache and temporary data |

---

# 🔵 What Happens to EBS When an EC2 Instance is Stopped?

The data stored in an EBS volume remains intact when an EC2 instance is stopped. Only compute resources are stopped, while the storage continues to exist until the volume is deleted.

---

# 🔵 What Happens to Instance Store When an EC2 Instance Stops?

Instance Store is ephemeral storage. Any data stored on it is permanently lost when the instance is stopped, terminated, or the underlying host fails.

---

# 🔵 Difference Between Stop, Reboot, Hibernate, and Terminate

| Action    | Result                                                             |
| --------- | ------------------------------------------------------------------ |
| Reboot    | Restarts the operating system without losing EBS data              |
| Stop      | Shuts down the instance while preserving EBS volumes               |
| Hibernate | Saves the contents of RAM to the root EBS volume and resumes later |
| Terminate | Permanently deletes the EC2 instance                               |

---

# 🔵 What is a Security Group?

A **Security Group** is an instance-level, stateful virtual firewall that controls inbound and outbound network traffic for an EC2 instance.

---

# 🔵 What is an Elastic IP?

An **Elastic IP (EIP)** is a static public IPv4 address provided by AWS that can be associated with an EC2 instance. Unlike a default public IP address, an Elastic IP remains the same even if the instance is stopped and started.

---

# 🔵 How Would You Troubleshoot if an EC2 Instance is Running but the Website is Not Accessible?

I would first verify that the web server (such as Nginx, Apache, or Node.js) and the application are running successfully. Next, I would check the Security Group to ensure that HTTP (80) and HTTPS (443) ports are allowed. I would also verify Network ACL rules, Route Tables, Internet Gateway configuration, Public IP assignment, DNS settings, and application logs. Finally, I would confirm that the application is listening on the expected port.

---

# 🔵 How Would You Troubleshoot if SSH Access to an EC2 Instance is Not Working?

I would verify that port **22** is allowed in the Security Group, ensure that the correct SSH key pair is being used, confirm that the instance has a Public IP or Elastic IP, inspect Route Tables and Internet Gateway configuration, review Network ACL rules, verify that the SSH service is running, and ensure that my source IP address is permitted in the Security Group.

---

# 🔵 How Would You Troubleshoot if a Private EC2 Instance Cannot Access the Internet?

I would verify that a NAT Gateway or NAT Instance is deployed in a public subnet. Next, I would ensure that the private subnet's Route Table contains a default route (`0.0.0.0/0`) pointing to the NAT device. I would also verify outbound rules in the Security Group and Network ACLs, and confirm that the NAT Gateway is healthy.

---

# 🔵 How Would You Troubleshoot if an EC2 Instance Suddenly Reaches 100% CPU Utilization?

I would use Linux tools such as **top**, **htop**, or **ps** along with Amazon CloudWatch metrics to identify the process consuming CPU resources. I would then analyze application logs, review recent deployments, inspect database queries, and investigate possible traffic spikes, memory leaks, infinite loops, or background jobs causing excessive CPU usage.

---

# 🔵 How Would You Handle a Production EC2 Instance Whose Root EBS Volume is Full?

I would first create an EBS Snapshot as a backup. Then I would increase the EBS volume size from the AWS Console, extend the operating system's filesystem, and verify that the newly allocated storage is available. In most cases, this operation can be completed without stopping the EC2 instance.

---

# 🔵 How Would You Troubleshoot if an EC2 Instance Becomes Unreachable After a Security Group Modification?

I would review all inbound and outbound Security Group rules, verify that my source IP address is allowed, inspect Network ACLs and Route Tables, and ensure that required ports such as SSH (22), HTTP (80), and HTTPS (443) have not been accidentally removed.

---

# 🔵 What Happens When an Instance Becomes Unhealthy in an Auto Scaling Group?

Auto Scaling continuously monitors the health of EC2 instances. If an instance fails the configured health checks, Auto Scaling automatically terminates the unhealthy instance and launches a replacement instance to maintain the desired capacity.

---

# 🔵 How Would You Troubleshoot if Auto Scaling is Not Launching Additional Instances?

I would verify CloudWatch alarms, scaling policies, Launch Template configuration, minimum, maximum, and desired capacity settings, subnet availability, AWS service quotas, and Auto Scaling activity logs to identify any errors preventing scaling.

---

# 🔵 How Would You Troubleshoot if a Load Balancer Reports Unhealthy Targets?

I would verify that the application is running, ensure that it is listening on the correct port, review Security Group rules, confirm that the health check path is valid, inspect Target Group configuration, and analyze application logs for errors.

---

# 🔵 How Would You Troubleshoot if an EC2 Instance is Running but the Application is Down?

A running EC2 instance only confirms that the virtual machine is operational. I would verify that the application service is running, inspect application logs, review process managers such as PM2, Docker, or Systemd, and ensure that the application is listening on the correct port.

---

# 🔵 How Would You Recover After Accidentally Terminating a Production EC2 Instance?

Recovery depends on available backups. If EBS Snapshots exist, I would create new EBS volumes from those snapshots and attach them to a newly launched EC2 instance. If a custom AMI is available, I would launch a new instance from the AMI and restore any required application data.

---

# 🔵 How Would You Deploy 100 Identical EC2 Instances with the Same Configuration?

I would create a Golden AMI containing all required software and configurations, create a Launch Template based on that AMI, and use an Auto Scaling Group to launch and manage the required number of identical EC2 instances.

---

# 🔵 How Would You Automatically Deploy Software When a New EC2 Instance Launches?

I would use EC2 User Data scripts with Cloud-Init to automatically install software packages, configure services, download application code, and complete all required server initialization tasks during instance startup.

---

# 🔵 How Would You Securely Allow an EC2 Instance to Access Amazon S3?

The recommended approach is to attach an IAM Role to the EC2 instance. IAM Roles provide temporary AWS credentials automatically and eliminate the need to store long-term AWS access keys on the server.

---

# 🔵 Can a Single EBS Volume Be Attached to Multiple EC2 Instances?

Normally, an EBS volume can only be attached to one EC2 instance at a time. However, **EBS Multi-Attach** supports attaching specific **io1** and **io2** volumes to multiple EC2 instances within the same Availability Zone.

---

# 🔵 Can an EBS Volume Be Attached to an EC2 Instance in a Different Availability Zone?

No. EBS volumes are specific to a single Availability Zone. To use the same data in another Availability Zone, you must create a snapshot and then create a new EBS volume from that snapshot in the target Availability Zone.

---

# 🔵 How Would You Migrate an EC2 Instance from One AWS Region to Another?

I would create an AMI of the source EC2 instance, copy the AMI to the destination AWS Region, and launch a new EC2 instance from the copied AMI. Additional application data can be migrated using EBS Snapshots, Amazon S3, or database replication.

---

# 🔵 How Would You Reduce EC2 Infrastructure Costs Significantly?

I would analyze utilization patterns and implement right-sizing, Reserved Instances, Savings Plans, Spot Instances for fault-tolerant workloads, Auto Scaling, and automated shutdown schedules for non-production environments.

---

# 🔵 How Would You Design a Highly Available EC2 Architecture?

I would deploy EC2 instances across multiple Availability Zones, place them behind an Application Load Balancer, configure Auto Scaling Groups for automatic recovery and scaling, and store application data on durable AWS services such as Amazon EBS, Amazon RDS, or Amazon S3.

---

# 🔵 How Would You Troubleshoot if Security Groups Allow Traffic but Connectivity Still Fails?

I would investigate Network ACL rules, operating system firewalls such as **iptables** or **firewalld**, verify that the application is running, inspect routing configuration, confirm DNS resolution, and review Application Load Balancer health checks.

---

# 🔵 How Would You Troubleshoot if User Data Scripts Did Not Execute During Instance Launch?

I would review Cloud-Init logs, verify User Data syntax, ensure the script is correctly formatted, confirm IAM permissions if AWS services are accessed, and inspect operating system startup logs for execution errors.

---

# 🔵 Why Does an EC2 Instance Receive a Different Public IP After Being Stopped and Started?

By default, AWS assigns dynamic public IP addresses to EC2 instances. When an instance is stopped and started, AWS may assign a different public IP address. To maintain a consistent public IP, an Elastic IP should be associated with the instance.

---

# 🔵 When Would You Choose EC2 Hibernation Instead of Stop?

I would use EC2 Hibernation when I need to preserve the contents of RAM and resume applications exactly where they left off. It is particularly useful for development environments, long-running applications, and workloads with lengthy startup times.

---

# 🔵 Why are Launch Templates Preferred Over Launch Configurations?

Launch Templates support versioning, Spot Instances, advanced networking features, multiple instance types, and newer AWS capabilities. Because of their flexibility and continued AWS support, they are the recommended option for modern EC2 deployments.

---

# 🔵 How Would You Troubleshoot Intermittent Application Failures on EC2?

I would analyze Amazon CloudWatch metrics, application logs, operating system logs, network performance, load balancer logs, CPU utilization, memory usage, storage performance, and database response times to identify recurring patterns causing the failures.

---

# 🔵 Why Might an Application Load Balancer Mark an EC2 Instance Unhealthy Even When the Instance is Running?

Common reasons include an incorrect health check path, the application not listening on the expected port, firewall restrictions, Security Group misconfigurations, application crashes, or incorrect Target Group settings.

---

# 🔵 Can EBS Volumes Be Resized Without Stopping the EC2 Instance?

Yes. AWS supports online EBS volume modification. After increasing the volume size, the operating system filesystem must also be expanded to utilize the additional storage capacity.

---

# 🔵 What is the Most Secure Method to Access Production EC2 Instances?

The recommended approach is **AWS Systems Manager Session Manager**. It eliminates the need for SSH access, open port 22, bastion hosts, and long-term SSH key management while providing centralized auditing, secure access, and fine-grained IAM-based permissions.
