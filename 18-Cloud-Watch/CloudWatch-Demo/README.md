# 🔵 Demo: Create a CloudWatch Alarm for EC2 CPU Utilization

![AWS](./demo-cloud-watch.png)

## Demo Objective

In this demo, we will configure an **Amazon CloudWatch Alarm** to monitor the CPU utilization of an EC2 instance.

When the **CPU utilization exceeds 80%**, CloudWatch will trigger an alarm and send an **email notification through Amazon SNS**.

> **Note:** To simulate high CPU usage, we'll use a simple Python script. This is **only for demonstration purposes** and should not be used in production environments.

---

# 🔵 Architecture

```text
EC2 Instance
     │
     ▼
CloudWatch Metrics (CPU Utilization)
     │
     ▼
CloudWatch Alarm (CPU > 80%)
     │
     ▼
Amazon SNS Topic
     │
     ▼
Email Notification
```

---

# 🔵 Step 1: Launch an EC2 Instance

Create an EC2 instance.

### Recommended Configuration

- **AMI:** Amazon Linux 2023 (or Amazon Linux 2)
- **Instance Type:** t2.micro (Free Tier eligible)
- **Key Pair:** Create or use an existing key pair
- **Security Group:**
  - SSH (Port 22) → Your IP

Launch the instance.

> If you've already followed the EC2 setup from the previous chapter, you can reuse the same instance.

---

# 🔵 Step 2: Connect to the EC2 Instance

Connect using SSH.

```bash
ssh -i my-key.pem ec2-user@<Public-IP>
```

Or, if you're using Windows:

- Command Prompt
- PowerShell
- Windows Terminal
- Git Bash

Verify the connection.

---

# 🔵 Step 3: Check Current CPU Usage

Run:

```bash
top
```

or

```bash
htop
```

(Current CPU usage should be low because the instance is idle.)

Press **Q** to exit.

---

# 🔵 Step 4: Install Python (If Not Installed)

Check the Python version.

```bash
python3 --version
```

If Python isn't installed:

### Amazon Linux

```bash
sudo dnf install python3 -y
```

or

```bash
sudo yum install python3 -y
```

Verify the installation:

```bash
python3 --version
```

---

# 🔵 Step 5: Enable Detailed Monitoring (Optional but Recommended)

If you don't see CPU metrics updating frequently:

Go to:

```text
EC2
   ↓
Instances
   ↓
Select Instance
   ↓
Monitoring
   ↓
Manage Detailed Monitoring
```

Enable:

> **Detailed Monitoring**

### Why?

- Basic Monitoring → Every **5 minutes**
- Detailed Monitoring → Every **1 minute**

This provides faster metric updates for the demo.

---

# 🔵 Step 6: Create a Python Script

Create a new Python file.

Using **vi**

```bash
vi cpu_utilization_demo.py
```

or using **nano**

```bash
nano cpu_utilization_demo.py
```

---

## Add Your CPU Stress Script

```python
import time

def simulate_cpu_spike(duration=60, cpu_percent=80):
    """
    Simulate approximately the given CPU utilization
    on a single CPU core.

    duration: Total runtime in seconds
    cpu_percent: Target CPU usage (1-100)
    """

    if cpu_percent < 1 or cpu_percent > 100:
        raise ValueError("cpu_percent must be between 1 and 100")

    print(f"Simulating approximately {cpu_percent}% CPU usage for {duration} seconds...")

    interval = 0.1  # 100 ms control interval
    busy_time = interval * (cpu_percent / 100)
    idle_time = interval - busy_time

    end_time = time.time() + duration

    while time.time() < end_time:

        # Busy loop
        start = time.perf_counter()
        while (time.perf_counter() - start) < busy_time:
            pass

        # Sleep for remaining interval
        if idle_time > 0:
            time.sleep(idle_time)

    print("CPU spike simulation completed.")

if __name__ == "__main__":
    simulate_cpu_spike(duration=60, cpu_percent=80)
```

Save the file.

```
Esc → :wq → Enter
```

---

# 🔵 Step 7: Run the Script

Execute:

```bash
python3 cpu_utilization_demo.py
```

The script should generate CPU load on the EC2 instance.

---

# 🔵 Step 8: Verify CPU Usage

Open another SSH terminal.

Run:

```bash
top
```

Now you'll notice CPU utilization increasing.

---

# 🔵 Step 9: Monitor CPU in CloudWatch

Open the AWS Console.

Go to:

```text
CloudWatch
    ↓
Metrics
    ↓
EC2
    ↓
Per-Instance Metrics
```

Select your EC2 instance.

Choose:

```text
CPUUtilization
```

Configure the graph:

- Statistic → **Average**
- Period → **5 Minutes** (or **1 Minute** if using Detailed Monitoring)

You'll observe CPU utilization increasing as the Python script runs.

---

# 🔵 Step 10: Create a CloudWatch Alarm

Navigate to:

```text
CloudWatch
    ↓
Alarms
    ↓
Create Alarm
```

Click:

```text
Select Metric
```

Choose:

```text
EC2
    ↓
Per-Instance Metrics
    ↓
CPUUtilization
```

Select your EC2 instance.

Click:

```text
Select Metric
```

---

# 🔵 Step 11: Configure Alarm Conditions

Configure the alarm as follows.

### Metric

```text
CPUUtilization
```

### Statistic

```text
Average
```

### Period

```text
5 Minutes
```

### Threshold Type

```text
Static
```

### Condition

```text
Greater than
```

### Threshold Value

```text
80%
```

This means:

> If CPU utilization remains above **80%**, CloudWatch will trigger the alarm.

Click **Next**.

---

# 🔵 Step 12: Configure Notifications (SNS)

CloudWatch uses **Amazon SNS (Simple Notification Service)** to send alerts.

### If an SNS Topic Already Exists

Select:

```text
Use Existing Topic
```

Choose your SNS topic.

---

### If No SNS Topic Exists

Create a new topic.

Enter:

```text
Topic Name:
EC2-CPU-Alerts
```

Add the notification endpoint:

```text
Protocol:
Email

Endpoint:
your-email@example.com
```

Click:

```text
Create Topic
```

Then click **Next**.

---

# 🔵 Step 13: Configure Alarm Details

Provide meaningful information.

### Alarm Name

```text
EC2-CPU-High-Usage
```

### Description

```text
Triggers an email notification when EC2 CPU utilization exceeds 80%.
```

Click:

```text
Create Alarm
```

---

# 🔵 Step 14: Confirm the SNS Subscription

Check your inbox (or Spam folder).

You'll receive an email similar to:

```text
AWS Notification - Subscription Confirmation
```

Click:

```text
Confirm Subscription
```

Without confirming, SNS cannot send notifications.

---

# 🔵 Step 15: Trigger the Alarm

Run the CPU stress script:

```bash
python3 cpu_utilization_demo.py
```

Wait a few minutes.

CloudWatch continuously collects CPU metrics.

Once CPU utilization exceeds **80%**, the alarm state changes:

```text
OK
        ↓
ALARM
```

---

# 🔵 Step 16: Receive the Email Alert

After the alarm enters the **ALARM** state, Amazon SNS sends an email notification.

Example:

```text
Subject:

ALARM: "EC2-CPU-High-Usage"

Message:

CloudWatch has detected that CPU utilization
for EC2 instance i-xxxxxxxxxxxx
has exceeded 80%.

Current State:
ALARM
```

The email is sent to the subscribed address.

---

# 🔵 Verify in CloudWatch

You can also verify the alarm status in:

```text
CloudWatch
    ↓
Alarms
```

Possible states:

- OK
- ALARM
- Insufficient Data

---

# 🔵 Demo Flow Summary

```text
Launch EC2
      │
      ▼
Connect via SSH
      │
      ▼
Check CPU (top)
      │
      ▼
Install Python (if required)
      │
      ▼
Create CPU Stress Script
      │
      ▼
Run Python Script
      │
      ▼
CPU Utilization Increases
      │
      ▼
CloudWatch Collects Metrics
      │
      ▼
CPU > 80%
      │
      ▼
CloudWatch Alarm Triggered
      │
      ▼
Amazon SNS
      │
      ▼
Email Notification Sent
```

---

# 🔵 Key Takeaway

This demo shows how **Amazon CloudWatch** continuously monitors an EC2 instance's CPU utilization. When the average CPU usage exceeds the configured threshold (**80%**), **CloudWatch automatically changes the alarm state and uses Amazon SNS to send an email notification**.

In real-world DevOps environments, this same mechanism can also trigger automated actions such as **EC2 Auto Scaling, AWS Lambda functions, Systems Manager Automation, or incident management workflows**, helping teams detect issues early and respond quickly.
