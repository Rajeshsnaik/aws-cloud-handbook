# 🔵 Simple Node.js + DynamoDB Project Demo

In this demo, we'll build a simple **User Management API** using **Node.js**, **Express.js**, and **Amazon DynamoDB**.

Instead of storing user data in MongoDB, all records will be stored directly in a DynamoDB table.

---

# 🔵 Step 1: Clone the Project

Clone the project repository.

```bash
git clone (https://github.com/Rajeshsnaik/dynamodb-user-management.git)
```

Move into the project folder.

```bash
cd dynamodb-user-management
```

Open it in VS Code.

```bash
code .
```

---

# 🔵 Step 2: Install Dependencies

Install all project dependencies.

```bash
npm install
```

If you're creating the project from scratch, install the required packages.

```bash
npm install express dotenv
```

Install the AWS SDK.

```bash
npm install @aws-sdk/client-dynamodb @aws-sdk/lib-dynamodb
```

---

# 🔵 Step 3: Install AWS CLI

If AWS CLI is not installed, install it on your local machine.

Verify the installation.

```bash
aws --version
```

---

# 🔵 Step 4: Configure AWS CLI

Run:

```bash
aws configure
```

Enter the following details:

```text
AWS Access Key ID:
AWS Secret Access Key:
Default Region Name:
Default Output Format:
```

Example:

```text
AWS Access Key ID: AKIA****************
AWS Secret Access Key: ************************
Default Region Name: ap-south-1
Default Output Format: json
```

This securely stores your AWS credentials locally, allowing both the AWS CLI and the AWS SDK to authenticate with AWS services.

---

# 🔵 Step 5: Create the DynamoDB Table

Before running the project, create a DynamoDB table from the AWS Console.

Example:

```text
Table Name : Users

Partition Key : UserId
```

Wait until the table status becomes **Active**.

---

# 🔵 Step 6: Configure Environment Variables

Create a `.env` file in the project's root directory.

```env
AWS_REGION=ap-south-1
TABLE_NAME=Users
```

If your project uses environment variables for credentials, you can also include:

```env
AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY
AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
```

> **Note:** If you've already configured the AWS CLI using `aws configure`, you typically don't need to store your AWS access keys in the `.env` file. The AWS SDK automatically uses the credentials configured by the CLI.

---

# 🔵 Step 7: Verify the Configuration

Before starting the application, ensure:

- AWS CLI is installed.
- AWS CLI is configured successfully.
- DynamoDB table is created.
- `.env` file contains the correct Region and Table Name.
- Internet connection is available.

---

# 🔵 Step 8: Run the Project

Start the application.

For a production build:

```bash
npm start
```

or for development:

```bash
npm run dev
```

If everything is configured correctly, the server starts successfully and connects to Amazon DynamoDB.

Example:

```text
Server running on port 3000
Connected to Amazon DynamoDB
```

---

# 🔵 Step 9: Test the Application

Use **Postman**, **Thunder Client**, or your frontend application to test the API.

Example operations:

- Create User
- Get User
- Update User
- Delete User

Each request stores or retrieves data directly from the **Users** table in DynamoDB.

You can verify the changes by opening:

**AWS Console → DynamoDB → Users Table → Explore Table Items**

Every CRUD operation performed by the application will be reflected in the table.

---

# 🔵 Project Flow

```text
Client / Postman
        │
        ▼
Node.js + Express API
        │
        ▼
AWS SDK for JavaScript
        │
        ▼
Amazon DynamoDB
        │
        ▼
Users Table
```

---

# 🔵 Why Run Locally Instead of EC2?

For this demo, the application runs on your **local machine** instead of an Amazon EC2 instance because:

- No EC2 setup is required.
- No Docker container is needed.
- Lower AWS costs.
- Faster development and testing.
- Simpler to demonstrate DynamoDB integration.

Once the application is production-ready, the same code can be deployed to **Amazon EC2**, **AWS Elastic Beanstalk**, **Amazon ECS**, or **AWS Lambda** without changing the DynamoDB integration.
