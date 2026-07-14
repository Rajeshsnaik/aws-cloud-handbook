# AWS Amplify

![Amplify](./amplify.png)

## 🔵 What is AWS Amplify?

**AWS Amplify** is a fully managed AWS platform that simplifies **building, deploying, hosting, and managing full-stack web and mobile applications**.

It provides everything needed to develop modern applications, including frontend hosting, backend services, authentication, storage, APIs, CI/CD, monitoring, and custom domains.

Amplify integrates directly with Git providers like GitHub, GitLab, Bitbucket, and CodeCommit, enabling automatic deployments whenever code changes are pushed.

---

## 🔵 Why Use AWS Amplify?

AWS Amplify helps developers:

- Build modern web and mobile applications
- Deploy applications with just a few clicks
- Host applications globally
- Automatically build and deploy from Git repositories
- Configure backend services without managing infrastructure
- Enable Continuous Integration and Continuous Deployment (CI/CD)
- Connect custom domains
- Manage SSL certificates automatically
- Scale applications without server management

---

## 🔵 Key Features

- Fully managed hosting
- Automatic CI/CD pipeline
- GitHub, GitLab, Bitbucket integration
- Custom domains
- Free SSL certificates
- Backend integration
- Authentication
- Storage
- APIs
- Functions (Lambda)
- Data Manager
- Preview deployments
- Deployment history
- Rollback support
- Global CDN
- Environment management
- Monitoring and logs

---

## 🔵 AWS Services Used by Amplify

Depending on your application, Amplify automatically uses several AWS services behind the scenes.

Common services include:

- Amazon S3
- Amazon CloudFront
- AWS IAM
- AWS Lambda
- Amazon API Gateway
- Amazon Cognito
- AWS AppSync
- Amazon DynamoDB
- AWS Route 53
- AWS Certificate Manager (ACM)
- Amazon CloudWatch

---

## 🔵 How AWS Amplify Works

```text
Developer

↓

GitHub Repository

↓

AWS Amplify

↓

Build

↓

Deploy

↓

CloudFront CDN

↓

Users
```

Whenever code is pushed to GitHub, Amplify automatically builds and deploys the latest version.

---

# Practical Demo 1 - Deploy a React Application

## 🔵 Prerequisites

Before starting, ensure you have:

- AWS Account
- GitHub Account
- React Application
- Node.js installed
- Git installed

Push your React project to GitHub before connecting it to Amplify.

---

## 🔵 Step 1: Open AWS Amplify

Go to:

```text
AWS Console

↓

AWS Amplify

↓

Create New App

↓

Host Web App
```

---

## 🔵 Step 2: Choose Source Code Provider

Select your Git provider.

Example:

```text
GitHub
```

Authorize AWS Amplify to access your GitHub account if prompted.

---

## 🔵 Step 3: Select Repository

Choose:

```text
Repository

↓

Branch
```

Example:

```text
Repository:
react-todo-app

Branch:
main
```

Click:

```text
Next
```

---

## 🔵 Step 4: Configure Build Settings

Provide:

```text
App Name:
React-Todo-App
```

Amplify automatically detects the framework.

Example build settings:

```yaml
version: 1

frontend:
  phases:
    preBuild:
      commands:
        - npm install

    build:
      commands:
        - npm run build

  artifacts:
    baseDirectory: build

    files:
      - "**/*"

  cache:
    paths:
      - node_modules/**/*
```

Review the configuration.

Click:

```text
Save and Deploy
```

---

## 🔵 Step 5: Deployment

Amplify automatically performs:

```text
Clone Repository

↓

Install Dependencies

↓

Build Project

↓

Deploy

↓

Host Application
```

After deployment completes, Amplify provides a domain similar to:

```text
https://main.xxxxxxxxx.amplifyapp.com
```

Open the URL in your browser.

Your React application should now be live.

---

# Practical Demo 2 - Automatic CI/CD Deployment

## 🔵 What is CI/CD?

**Continuous Integration (CI)** automatically builds and tests your application whenever code changes are pushed.

**Continuous Deployment (CD)** automatically deploys the latest version without manual intervention.

Amplify provides a built-in CI/CD pipeline.

---

## 🔵 Test Automatic Deployment

Open your React project locally.

Make any changes.

Example:

```text
Change the page title

OR

Update some UI text
```

Commit the changes.

```bash
git add .

git commit -m "Updated homepage"

git push origin main
```

After pushing the code:

```text
GitHub

↓

AWS Amplify detects changes

↓

Automatic Build

↓

Automatic Deployment

↓

Website Updated
```

No manual deployment is required.

Refresh the same Amplify URL.

The latest changes should appear automatically.

---

## 🔵 View Deployment History

Go to:

```text
AWS Amplify

↓

Your Application

↓

Hosting

↓

Deployment History
```

Here you can view:

- Build logs
- Deployment logs
- Deployment status
- Previous deployments
- Rollback history

---

# Practical Demo 3 - Connect a Custom Domain

## 🔵 Step 1: Purchase a Domain

If you do not already own a domain, purchase one from:

- Amazon Route 53
- Hostinger
- GoDaddy
- Namecheap

If using an external provider:

- Create a Hosted Zone in Route 53 (optional if managing DNS with AWS)
- Update the domain's Name Servers with those provided by Route 53
- Wait for DNS propagation

---

## 🔵 Step 2: Add Custom Domain

Go to:

```text
AWS Amplify

↓

Hosting

↓

Custom Domains

↓

Add Domain
```

Enter:

```text
yourdomain.com
```

Click:

```text
Next
```

---

## 🔵 Step 3: Configure Subdomains

Example:

```text
www

↓

main Branch
```

or

```text
app

↓

main Branch
```

---

## 🔵 Step 4: Configure SSL Certificate

Choose one of the following:

- Amplify Managed SSL Certificate (Recommended)
- Custom SSL Certificate

The Amplify-managed certificate is free and automatically renewed.

---

## 🔵 Step 5: Verify Domain

After configuration:

```text
Domain Verification

↓

SSL Certificate Issued

↓

DNS Propagation

↓

Website Available
```

This process may take several minutes.

Finally, open:

```text
https://yourdomain.com
```

Your Amplify application should now be accessible using your custom domain.

---

# Practical Demo 4 - Deploy a Sample React Application

AWS Amplify provides ready-to-use sample applications.

Go to:

```text
AWS Amplify Documentation

↓

Quick Start

↓

Create Repository From Template
```

Choose:

- GitHub Repository
- Repository Name
- Public or Private

Click:

```text
Create Repository
```

A sample React application is automatically created in GitHub.

Now return to Amplify.

Create a new application.

Choose only the newly created repository.

Follow the same deployment steps explained earlier.

Amplify builds and deploys the application automatically.

---

# Practical Demo 5 - Review Backend Resources

After deployment:

Go to:

```text
AWS Amplify

↓

Your Application

↓

Data

↓

Data Manager
```

Here you can view:

- Backend resources
- Connected services
- Application data
- Backend configuration

You can also explore the generated project files and backend resources associated with your deployment.

---

# Practical Demo 6 - Clone the Amplify Project Locally

To work with the same project locally:

Ensure the following are installed:

- Node.js
- Git

Clone the repository:

```bash
git clone <repository_url>
```

Move into the project:

```bash
cd project-folder
```

Install dependencies:

```bash
npm install
```

Go to:

```text
AWS Amplify

↓

Your Application

↓

Backend Resources

↓

Download amplify_outputs.json
```

Copy the downloaded file into the project's root directory.

Now start the application:

```bash
npm run dev
```

Your local project is now connected to the deployed Amplify backend resources.

You can continue development locally while using the same cloud backend.

---

## 🔵 Add More Features

After your application is connected, Amplify provides guided workflows to add additional services such as:

- Authentication
- APIs
- Database
- Storage
- File Uploads
- Functions
- Notifications
- Analytics

Follow the Amplify documentation to enable these features as needed.

---

## 🔵 Update the Application

After making changes locally:

```bash
git add .

git commit -m "Added new feature"

git push origin main
```

Amplify automatically:

```text
Detects Changes

↓

Builds Project

↓

Deploys Latest Version

↓

Updates Live Website
```

No manual deployment is required.

---

## 🔵 Best Practices

- Connect Amplify with a Git repository.
- Use separate branches for development and production.
- Enable automatic deployments.
- Use Amplify-managed SSL certificates whenever possible.
- Connect a custom domain for production applications.
- Monitor build logs and deployment history regularly.
- Keep `amplify_outputs.json` synchronized with your local project.
- Protect production branches using Git branch protection rules.

---

## 🔵 Interview Questions

### Q1. What is AWS Amplify?

**Answer:** AWS Amplify is a fully managed platform for building, deploying, hosting, and managing full-stack web and mobile applications with built-in CI/CD and backend integrations.

### Q2. Does Amplify provide CI/CD?

**Answer:** Yes. Amplify automatically detects code changes in connected Git repositories, builds the application, and deploys the latest version without manual intervention.

### Q3. Which Git providers are supported by Amplify?

**Answer:** Amplify supports GitHub, GitLab, Bitbucket, and AWS CodeCommit.

### Q4. Can I use my own domain with Amplify?

**Answer:** Yes. Amplify supports custom domains and provides free SSL certificates through AWS Certificate Manager (ACM).

### Q5. What happens when code is pushed to the main branch?

**Answer:** Amplify automatically detects the commit, starts a new build, deploys the updated application, and makes the latest version available using the same application URL or custom domain.
