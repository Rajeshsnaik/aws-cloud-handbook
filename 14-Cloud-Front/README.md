# 🔵 Amazon CloudFront (CDN)

![Cloud-Front](./cloud-front.png)

**Amazon CloudFront** is AWS's **Content Delivery Network (CDN)** that delivers websites, APIs, videos, images, and other web content to users with **low latency** and **high transfer speeds**.

Instead of serving content from a single server, CloudFront caches content at locations around the world and serves users from the nearest location.

---

# 🔵 What is a CDN?

A **Content Delivery Network (CDN)** is a network of servers distributed across multiple geographic locations.

Its primary purpose is to deliver content to users from the **nearest location**, reducing latency and improving website performance.

Without a CDN:

- Every request travels to the origin server.
- Users far away experience higher latency.
- Website loading becomes slower.

With a CDN:

- Frequently accessed content is cached closer to users.
- Requests travel a much shorter distance.
- Pages load much faster.

---

# 🔵 Why Do We Need CloudFront?

Suppose your website is hosted on an **Amazon EC2 instance** in the **Mumbai (ap-south-1)** Region.

Users access the same website from:

- India
- United States
- Europe
- Australia
- Japan

All users must connect to the **same EC2 server in Mumbai**.

As the distance increases:

- Network latency increases.
- Page loading time increases.
- User experience becomes slower.

Example:

```text
Website Hosted

Mumbai EC2
      │
      ├──────── India User (Fast)
      │
      ├──────────────────────── USA User (Slower)
      │
      ├──────────────────────────── Europe User (Slower)
      │
      └──────────────────────────────── Australia User (Slowest)
```

Although everyone accesses the same website, users experience different loading speeds because they are physically farther from the server.

---

# 🔵 Global Website with Low Latency

Modern applications are accessed by users from all over the world.

Examples include:

- E-commerce websites
- Social media platforms
- Video streaming platforms
- SaaS applications
- Online learning platforms

These applications require:

- Fast loading
- Low latency
- High availability

CloudFront helps provide a consistent experience regardless of user location.

---

# 🔵 Using Route 53 with CloudFront

A common AWS architecture is:

```text
User
   │
   ▼
Route 53
   │
   ▼
CloudFront
   │
   ▼
Origin (EC2 / S3 / Load Balancer / API)
```

### Flow

1. User enters your domain name.
2. Route 53 resolves the domain.
3. Traffic is directed to CloudFront.
4. CloudFront checks its cache.
5. If cached, content is returned immediately.
6. Otherwise, CloudFront retrieves the content from the origin.

---

# 🔵 Actual Use Case

Suppose a company hosts its website on an EC2 instance in Mumbai.

Customers visit the website from:

- India
- Germany
- USA
- Australia

Without CloudFront:

Every user connects directly to Mumbai.

This causes higher latency for users located far away.

With CloudFront:

CloudFront stores cached content in edge locations around the world.

Users automatically receive content from the nearest edge location.

Result:

- Faster website loading
- Lower latency
- Better user experience

---

# 🔵 How CloudFront Solves the Problem

Instead of every request reaching the origin server:

```text
User
    │
Nearest Edge Location
    │
    ▼
Cached Content
```

Only when the requested content is unavailable in the cache does CloudFront contact the origin server.

This significantly reduces:

- Response time
- Origin server load
- Network traffic

---

# 🔵 How CloudFront Works

CloudFront works as follows:

```text
User
     │
     ▼
Nearest Edge Location
     │
     ├── Cache Hit
     │       │
     │       ▼
     │   Return Cached Content
     │
     └── Cache Miss
             │
             ▼
        Origin Server
             │
             ▼
      Cache at Edge
             │
             ▼
         Return Response
```

The next request for the same content is served directly from the edge location.

---

# 🔵 How CloudFront Improves Performance

CloudFront improves performance by:

- Serving content from the nearest edge location.
- Reducing network latency.
- Reducing requests to the origin server.
- Caching frequently accessed files.
- Improving website responsiveness.
- Supporting high traffic with ease.

---

# 🔵 What Gets Cached?

CloudFront mainly caches **static content**.

Examples:

- HTML files
- CSS files
- JavaScript files
- Images
- Videos
- PDF files
- Fonts

These files rarely change and can be safely cached.

---

# 🔵 What Is Not Cached?

Dynamic content is generally fetched from the origin server.

Examples:

- Login requests
- User profile information
- Payment transactions
- Shopping cart data
- Database queries
- Personalized dashboards

These responses are usually unique for each user.

---

# 🔵 AWS Region vs Edge Location vs Regional Edge Cache

## AWS Region

An AWS Region is where AWS services such as EC2, RDS, Lambda, and S3 are deployed.

Examples:

- Mumbai
- Singapore
- London
- Virginia

Regions contain multiple Availability Zones.

---

## Edge Location

Edge Locations are global locations where CloudFront caches content close to users.

Purpose:

- Deliver cached content quickly.
- Reduce latency.
- Improve performance.

Users connect to the nearest Edge Location instead of the origin.

---

## Regional Edge Cache

Regional Edge Caches sit between Edge Locations and the origin server.

```text
Origin Server
       │
       ▼
Regional Edge Cache
       │
       ▼
Edge Location
       │
       ▼
User
```

Benefits:

- Larger cache capacity.
- Reduces requests to the origin.
- Improves cache efficiency.

---

# 🔵 Demo Website

Before creating CloudFront, deploy a website.

You can use:

- Static website hosted on Amazon S3
- Website hosted on Amazon EC2

This website becomes the **Origin** for CloudFront.

---

# 🔵 Create a CloudFront Distribution

Navigate to:

```text
AWS Console → CloudFront → Create Distribution
```

---

# 🔵 Configure the Origin

Select the origin that CloudFront should serve.

Supported origins include:

- Amazon S3 Bucket
- EC2 Instance
- Application Load Balancer
- API Gateway
- Custom Origin

Specify the origin domain name.

---

# 🔵 Configure Distribution Settings

During distribution creation, configure options such as:

- Origin
- Viewer Protocol Policy
- Allowed HTTP Methods
- Cache Policy
- Compress Objects Automatically
- Price Class
- Alternate Domain Name (Optional)
- SSL Certificate (Optional)
- Default Root Object

Review the configuration and create the distribution.

---

# 🔵 Wait for Distribution Deployment

After creating the distribution, CloudFront starts deploying it.

Status changes from:

```text
Deploying
```

to

```text
Deployed
```

Deployment usually takes several minutes.

---

# 🔵 Check Last Modified Time

Open the CloudFront distribution.

You can view details such as:

- Distribution ID
- Status
- Domain Name
- Last Modified

The **Last Modified** field helps verify when the distribution configuration was last updated.

---

# 🔵 Access the CloudFront Domain

Every distribution receives a unique CloudFront domain.

Example:

```text
https://dxxxxxxxxxxxxx.cloudfront.net
```

Open this domain in your browser.

The website should now load through CloudFront instead of directly from the origin server.

---

# 🔵 Reports & Analytics

Open the **Reports & Analytics** tab for the distribution.

Here you can monitor:

- Cache statistics
- Cache hit ratio
- Requests
- Data transferred
- Traffic trends
- Geographic usage
- Popular objects

These metrics help evaluate CloudFront performance and cache efficiency.
