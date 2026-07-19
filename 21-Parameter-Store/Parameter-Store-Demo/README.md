# 🔵 Using AWS Parameter Store in a Next.js Project

Here, we are creating a simple Next.js project to demonstrate how we can use **AWS Parameter Store** instead of a `.env` file to manage configuration values and run the application.

I have included only the main files from the **lib** folder, such as the **SSM Utility**, **Config Loader**, **MongoDB Connection**, and **JWT Utility** files.

For the remaining files (such as API routes, login pages, `page.js`, etc.), create them as needed and follow the configuration shown below to integrate **AWS Parameter Store** into your project.

---

# 🔵 Project Structure

```text
nextjs-app/
│
├── app/
│   │
│   ├── api/
│   │   ├── auth/
│   │   │   └── route.js
│   │   └── users/
│   │       └── route.js
│   │
│   ├── login/
│   ├── dashboard/
│   └── page.js
│
├── components/
│
├── lib/
│   │
│   ├── ssm.js
│   ├── config.js
│   ├── mongodb.js
│   └── jwt.js
│
├── public/
│
├── package.json
│
└── next.config.js
```

---

# 🔵 AWS Parameter Store

```text
Parameter Store

nextjs
   │
   └── prod
        │
        ├── mongodb-uri
        ├── jwt-secret
```

Actual names:

```text
/nextjs/prod/mongodb-uri
/nextjs/prod/jwt-secret
```

---

# 🔵 Create Systems Manager (SSM) Utility

Create an SSM utility that connects to **AWS Systems Manager Parameter Store** and retrieves encrypted parameters.

```javascript
// lib/ssm.js

import { SSMClient, GetParameterCommand } from "@aws-sdk/client-ssm";

const client = new SSMClient({
  region: "ap-south-1",
});

export async function getParameter(name) {
  const command = new GetParameterCommand({
    Name: name,
    WithDecryption: true,
  });

  const response = await client.send(command);

  return response.Parameter?.Value;
}
```

---

# 🔵 Create Config Loader

This file loads the required parameters only once and makes them available throughout the application.

```javascript
// lib/config.js
// This loads secrets only once

import { getParameter } from "./ssm";

const config = {
  mongoUri: await getParameter("/nextjs/prod/mongodb-uri"),

  jwtSecret: await getParameter("/nextjs/prod/jwt-secret"),

  razorpayKey: await getParameter("/nextjs/prod/razorpay-key"),

  razorpaySecret: await getParameter("/nextjs/prod/razorpay-secret"),
};

export default config;
```

---

# 🔵 MongoDB Connection

Use the MongoDB URI retrieved from **AWS Parameter Store** to establish the database connection.

```javascript
// lib/mongodb.js

import mongoose from "mongoose";
import config from "./config";

export async function connectDB() {
  if (mongoose.connection.readyState) return;

  await mongoose.connect(config.mongoUri);
}
```

---

# 🔵 JWT Utility

Generate JWT tokens using the secret stored securely in **AWS Parameter Store**.

```javascript
// lib/jwt.js

import jwt from "jsonwebtoken";
import config from "./config";

export function generateToken(payload) {
  return jwt.sign(payload, config.jwtSecret);
}
```

---

# 🔵 How It Works

```text
Next.js Application
        │
        ▼
Config Loader
        │
        ▼
SSM Utility
        │
        ▼
AWS Systems Manager Parameter Store
        │
        ▼
Decrypt Using AWS KMS
        │
        ▼
Return Configuration Values
        │
        ▼
MongoDB • JWT • Razorpay • APIs
```

---

# 🔵 Benefits

- No `.env` file required for sensitive configuration values.
- Centralized configuration management.
- Parameters are securely encrypted using AWS KMS.
- IAM controls who can access parameters.
- Supports multiple environments (Development, QA, Production).
- Configuration can be updated without modifying application code.
- Improves security and follows AWS best practices for secret management.
