# 🔵 Amazon RDS Demo (MySQL)

In this demo, we'll create a **MySQL database** using Amazon RDS and connect a Node.js application to it.

The application will store user data in **Amazon RDS MySQL** instead of using a local MySQL database.

---

# 🔵 Databases Supported by Amazon RDS

Amazon RDS supports the following relational database engines:

- MySQL
- PostgreSQL
- MariaDB
- Oracle Database
- Microsoft SQL Server
- Amazon Aurora (MySQL & PostgreSQL Compatible)

---

# 🔵 Why Use Amazon RDS?

Instead of installing and managing a database server yourself, Amazon RDS automates database management.

### Benefits

- Fully Managed Database
- Automatic Backups
- Easy Scaling
- High Availability
- Security & Encryption
- Monitoring
- Automatic Patching

---

# 🔵 Step 1: Create an RDS Database

Navigate to:

```text
AWS Console → Amazon RDS → Databases → Create Database
```

---

# 🔵 Step 2: Choose Database Creation Method

Amazon RDS provides two creation methods:

- **Easy Create** – AWS automatically configures most settings.
- **Standard Create** – Configure all settings manually.

For this demo, select:

```text
Standard Create
```

---

# 🔵 Step 3: Choose Database Engine

Select the required database engine.

Example:

```text
MySQL
```

Choose the preferred **MySQL Engine Version**.

---

# 🔵 Step 4: Choose Template

Amazon RDS provides predefined templates.

- Production
- Dev/Test
- Free Tier

For learning and this demo, choose:

```text
Free Tier
```

For production workloads, choose:

```text
Production
```

---

# 🔵 Step 5: Availability & Durability (Production)

If using the **Production** template, select the appropriate deployment option:

- Multi-AZ DB Cluster
- Multi-AZ DB Instance
- Single DB Instance

Choose based on your application's availability requirements.

---

# 🔵 Step 6: Configure Database Settings

Provide the database details.

Example:

```text
DB Instance Identifier : mydb-instance

Master Username : admin

Master Password : ********
```

Choose a strong password and confirm it.

---

# 🔵 Step 7: Choose DB Instance Class

Select the instance size based on your workload.

Examples:

- db.t3.micro (Free Tier)
- db.t3.small
- db.m7g.large

Higher instance classes provide more CPU and memory.

---

# 🔵 Step 8: Configure Storage

Choose the storage type and storage size.

You can also enable:

- Storage Auto Scaling

This automatically increases storage when needed.

---

# 🔵 Step 9: Configure Connectivity

Configure how applications will access the database.

Important settings:

- VPC
- Public Access (Yes/No)
- Security Group
- Availability Zone

For this demo:

- Enable **Public Access**
- Allow MySQL port **3306** in the Security Group
- Connect from the local Node.js application

For production applications:

- Keep RDS private inside a VPC
- Connect through EC2 or application servers

---

# 🔵 Step 10: Add Tags (Optional)

Tags help organize AWS resources.

Example:

```text
Project = Demo
Environment = Development
Owner = Rajesh
```

---

# 🔵 Step 11: Review Estimated Cost

Before creating the database, review the **Estimated Monthly Cost**, especially for production deployments.

This helps avoid unexpected charges.

---

# 🔵 Step 12: Create Database

Click **Create Database**.

Wait until the database status changes from:

```text
Creating
```

to:

```text
Available
```

Once the status is **Available**, the database is ready for use.

---

# 🔵 Connect RDS MySQL Locally

After the RDS database is available, connect it from your local machine.

You can use:

- MySQL Workbench
- MySQL CLI
- DBeaver

Connection details are available in:

```text
AWS Console → RDS → Database → Connectivity & Security
```

Required details:

```text
Endpoint
Port
Username
Password
```

Example:

```text
Host:
mydb-instance.xxxxx.ap-south-1.rds.amazonaws.com

Port:
3306

Username:
admin
```

---

# 🔵 Create a Database Inside MySQL

After connecting to the RDS instance, create your application database.

Example:

```sql
CREATE DATABASE studentdb;
```

Use the database:

```sql
USE studentdb;
```

You can then create tables and start storing data.

---

# 🔵 Simple Node.js + RDS (MySQL) Project Demo

In this demo, a **Node.js + Express** application stores data in an **Amazon RDS MySQL** database.

---

# 🔵 Step 1: Clone the Project

Clone the repository.

```bash
git clone https://github.com/Rajeshsnaik/rds-mysql-user-management.git
```

Move into the project folder.

```bash
cd rds-mysql-user-management
```

Open the project in VS Code.

```bash
code .
```

Install project dependencies.

```bash
npm install
```

---

# 🔵 Step 2: Install MySQL Driver

Install the MySQL package for Node.js.

```bash
npm install mysql2
```

Install Express and environment variables package.

```bash
npm install express dotenv
```

---

# 🔵 Step 3: Configure Environment Variables

Create a `.env` file.

```env
DB_HOST=mydb-instance.xxxxx.ap-south-1.rds.amazonaws.com
DB_PORT=3306
DB_NAME=studentdb
DB_USER=admin
DB_PASSWORD=your_password
```

These values come from the RDS instance details.

---

# 🔵 Step 4: Connect the Application

Update your database connection code to use the RDS endpoint instead of a local MySQL server.

The application will connect directly to:

```text
Node.js Application
        │
        ▼
Amazon RDS MySQL Endpoint
        │
        ▼
Student Database
```

---

# 🔵 Step 5: Run the Application

Start the server.

```bash
npm start
```

or

```bash
npm run dev
```

If everything is configured correctly, the application connects successfully to Amazon RDS.

Example:

```text
Server running on port 3000
Connected to Amazon RDS MySQL
```

---

# 🔵 Step 6: Test CRUD Operations

Use **Postman**, **Thunder Client**, or your frontend application to test:

- Create User
- Get User
- Update User
- Delete User

Verify the inserted data by connecting to the RDS database using:

- MySQL Workbench
- MySQL CLI
- DBeaver

---

# 🔵 Project Flow

```text
Client / Postman
        │
        ▼
Node.js + Express API
        │
        ▼
MySQL Driver (mysql2)
        │
        ▼
Amazon RDS (MySQL)
        │
        ▼
Student Database
        │
        ▼
Tables & Records
```

---

# 🔵 Why Run Locally Instead of EC2?

For this demo, the application runs on your **local machine** instead of an Amazon EC2 instance because:

- No EC2 setup is required.
- No Docker container is needed.
- Lower AWS costs.
- Faster development and testing.
- Simpler to demonstrate RDS MySQL integration.

Once the application is production-ready, the same code can be deployed to:

- Amazon EC2
- AWS Elastic Beanstalk
- Amazon ECS
- AWS Lambda

without changing the RDS MySQL integration.

---

This demonstrates how a real-world application connects to a managed MySQL database on Amazon RDS without installing or maintaining a database server yourself.
