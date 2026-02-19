# 🌐 C. KIẾN TRÚC ỨNG DỤNG WEB

> **Mục tiêu:** Thiết kế web applications với compute, load balancing, và auto scaling

---

## 📋 Mục Lục
1. [Compute Options](#compute-options)
2. [Load Balancing Patterns](#load-balancing)
3. [Auto Scaling Strategies](#auto-scaling)
4. [Serverless Architectures](#serverless)
5. [Container Orchestration](#containers)
6. [Architecture Patterns](#architecture-patterns)

---

## 💻 Compute Options

### Decision Tree: Chọn Compute Service

```
START
│
├─ Cần server management? 
│  ├─ YES → EC2
│  └─ NO ↓
│
├─ Workload type?
│  ├─ Short-lived functions (<15 min) → Lambda
│  ├─ Containers → ECS/EKS/Fargate
│  ├─ Batch processing → AWS Batch
│  └─ Simple web app → Elastic Beanstalk
```

### EC2 (Elastic Compute Cloud)

#### **Instance Types - Naming Convention**

```
m5.2xlarge
│ │  │
│ │  └─ Size (nano, micro, small, medium, large, xlarge, 2xlarge, etc.)
│ └──── Generation (5 = 5th generation)
└────── Family (m = general purpose)
```

#### **Instance Families**

| Family | Type | Use Case | Example |
|--------|------|----------|---------|
| **T** | Burstable | Dev/test, low traffic web | t3.micro, t3.medium |
| **M** | General Purpose | Balanced compute/memory | m5.large, m6i.xlarge |
| **C** | Compute Optimized | High-performance computing | c5.2xlarge |
| **R** | Memory Optimized | In-memory databases, caching | r5.4xlarge |
| **X** | Memory Optimized | SAP HANA, in-memory DB | x1e.32xlarge |
| **I** | Storage Optimized | NoSQL databases, data warehousing | i3.8xlarge |
| **G** | GPU | Machine learning, video rendering | g4dn.xlarge |
| **P** | GPU | ML training, HPC | p3.8xlarge |

**Ví dụ trong đề thi:**
> **Q:** Application cần high CPU performance cho batch processing. Instance type nào phù hợp?
> 
> ✅ **C5 family** (Compute Optimized)

#### **Pricing Models**

| Model | Commitment | Discount | Use Case |
|-------|------------|----------|----------|
| **On-Demand** | None | 0% | Unpredictable workloads |
| **Reserved (1-3 year)** | 1-3 years | Up to 72% | Steady-state workloads |
| **Savings Plans** | 1-3 years | Up to 72% | Flexible compute (EC2, Fargate, Lambda) |
| **Spot Instances** | None | Up to 90% | Fault-tolerant, flexible workloads |
| **Dedicated Hosts** | On-Demand/Reserved | Varies | Licensing requirements, compliance |
| **Dedicated Instances** | On-Demand/Reserved | Varies | Hardware isolation |

**Reserved Instances Options:**
- **Standard RI:** Highest discount, không flexible
- **Convertible RI:** Lower discount, có thể change instance type
- **Scheduled RI:** Recurring schedule (VD: batch jobs mỗi ngày 2AM-6AM)

**Ví dụ trong đề thi:**
> **Q:** Application chạy 24/7 trong 3 năm. Cách nào MOST cost-effective?
> 
> ✅ **3-year Standard Reserved Instance** (up to 72% discount)

> **Q:** Batch processing có thể bị interrupt. Cách nào rẻ nhất?
> 
> ✅ **Spot Instances** (up to 90% discount)

#### **Placement Groups**

| Type | Use Case | Pros | Cons |
|------|----------|------|------|
| **Cluster** | HPC, low latency | Lowest latency (10 Gbps) | Single AZ, single point of failure |
| **Spread** | Critical instances | Max 7 instances/AZ, isolated hardware | Limited instances |
| **Partition** | Distributed systems (Hadoop, Kafka) | Multiple partitions/AZ | Complex setup |

**Ví dụ trong đề thi:**
> **Q:** Big data application cần low latency giữa instances. Placement group nào phù hợp?
> 
> ✅ **Cluster Placement Group**

---

## ⚖️ Load Balancing

### Elastic Load Balancer (ELB) Types

#### **Application Load Balancer (ALB)** - Layer 7 (HTTP/HTTPS)

**Features:**
- ✅ HTTP/HTTPS traffic
- ✅ Path-based routing (`/api/*`, `/images/*`)
- ✅ Host-based routing (`api.example.com`, `www.example.com`)
- ✅ Query string/header routing
- ✅ WebSocket support
- ✅ HTTP/2 support
- ✅ Target types: EC2, IP, Lambda, ECS containers

**Use Cases:**
- Microservices architectures
- Container-based applications
- Modern web applications

**Routing Example:**
```
ALB
├─ /api/* → Target Group 1 (API servers)
├─ /images/* → Target Group 2 (Image servers)
└─ / → Target Group 3 (Web servers)
```

**Ví dụ trong đề thi:**
> **Q:** Microservices application cần route traffic based on URL path. ELB nào phù hợp?
> 
> ✅ **Application Load Balancer** (Layer 7, path-based routing)

#### **Network Load Balancer (NLB)** - Layer 4 (TCP/UDP)

**Features:**
- ✅ Ultra-high performance (millions requests/second)
- ✅ Ultra-low latency (<100 microseconds)
- ✅ Static IP per AZ
- ✅ Elastic IP support
- ✅ TCP/UDP/TLS traffic
- ✅ Preserve source IP

**Use Cases:**
- Extreme performance requirements
- Static IP requirements
- Non-HTTP protocols

**Ví dụ trong đề thi:**
> **Q:** Application cần handle millions of requests/second với lowest latency. ELB nào phù hợp?
> 
> ✅ **Network Load Balancer** (ultra-high performance)

#### **Gateway Load Balancer (GWLB)** - Layer 3 (IP)

**Features:**
- ✅ Deploy, scale, manage third-party appliances
- ✅ Firewalls, IDS/IPS, deep packet inspection
- ✅ Transparent network gateway
- ✅ GENEVE protocol (port 6081)

**Use Cases:**
- Third-party security appliances
- Network monitoring tools

#### **Classic Load Balancer (CLB)** - Legacy

**Status:** ❌ Deprecated - Không nên dùng cho new applications

### ELB Features

#### **Health Checks**

**ALB/NLB Health Check:**
```
Protocol: HTTP/HTTPS/TCP
Port: 80, 443, custom
Path: /health (ALB only)
Interval: 5-300 seconds
Timeout: 2-120 seconds
Healthy threshold: 2-10 checks
Unhealthy threshold: 2-10 checks
```

**Ví dụ:**
```
Health Check: GET /health
Response: 200 OK
→ Instance marked healthy
```

#### **Cross-Zone Load Balancing**

**Enabled:**
```
AZ-A (2 instances) ← 50% traffic
AZ-B (8 instances) ← 50% traffic
→ Each instance gets 10% traffic
```

**Disabled:**
```
AZ-A (2 instances) ← 50% traffic → 25% per instance
AZ-B (8 instances) ← 50% traffic → 6.25% per instance
```

**Default:**
- ALB: Always enabled (cannot disable)
- NLB: Disabled (pay per GB if enabled)
- CLB: Disabled (free if enabled)

#### **Sticky Sessions (Session Affinity)**

**Purpose:** Route user requests tới same instance

**Types:**
- **Duration-based:** Cookie expiration (1 second - 7 days)
- **Application-based:** Custom cookie

**Khi nào dùng:**
- ✅ User session data stored locally on instance
- ❌ Không dùng nếu có: ElastiCache, DynamoDB for sessions

**Ví dụ trong đề thi:**
> **Q:** Users losing session data khi ALB routes tới different instance. Giải pháp nào?
> 
> Option 1: ✅ Enable sticky sessions
> Option 2: ✅ Store sessions trong ElastiCache (better solution)

#### **Connection Draining / Deregistration Delay**

**Purpose:** Complete in-flight requests trước khi terminate instance

**Settings:**
- Default: 300 seconds
- Range: 1-3600 seconds
- Set to 0 to disable

**Use Case:** Graceful shutdown during deployments

---

## 📈 Auto Scaling

### Auto Scaling Components

```
Auto Scaling Group (ASG)
├─ Launch Template/Configuration
├─ Scaling Policies
├─ Health Checks
└─ Notifications
```

#### **Launch Template vs Launch Configuration**

| Feature | Launch Template | Launch Configuration |
|---------|-----------------|----------------------|
| **Versioning** | ✅ Yes | ❌ No |
| **Multiple instance types** | ✅ Yes | ❌ No |
| **Spot + On-Demand** | ✅ Yes | ❌ No |
| **T2 Unlimited** | ✅ Yes | ❌ No |
| **Recommended** | ✅ Yes | ❌ Legacy |

**Best Practice:** ✅ Always use Launch Template

### Scaling Policies

#### **1. Target Tracking Scaling** ⭐ RECOMMENDED

**Concept:** Maintain metric at target value

**Examples:**
```
Target: Average CPU = 50%
→ ASG automatically adds/removes instances

Target: ALB Request Count = 1000 per target
→ ASG scales based on requests
```

**Ví dụ trong đề thi:**
> **Q:** Application cần maintain CPU utilization around 60%. Scaling policy nào simplest?
> 
> ✅ **Target Tracking** với target value = 60%

#### **2. Step Scaling**

**Concept:** Scale based on CloudWatch alarm thresholds

**Example:**
```
CPU > 80% → Add 2 instances
CPU > 90% → Add 4 instances
CPU < 40% → Remove 1 instance
```

**Use Case:** More control over scaling increments

#### **3. Simple Scaling**

**Concept:** Single scaling adjustment

**Example:**
```
CPU > 70% → Add 1 instance
Wait 300 seconds (cooldown)
```

**Limitation:** ❌ Cooldown period (không scale during cooldown)

#### **4. Scheduled Scaling**

**Concept:** Scale at specific times

**Example:**
```
Mon-Fri 8AM: Min=10, Max=50, Desired=20
Mon-Fri 6PM: Min=2, Max=10, Desired=5
```

**Use Case:** Predictable traffic patterns

#### **5. Predictive Scaling**

**Concept:** Machine learning predicts traffic và scales ahead

**Use Case:** Applications với daily/weekly patterns

### Scaling Metrics

**Common Metrics:**
- ✅ CPU Utilization
- ✅ Network In/Out
- ✅ ALB Request Count per Target
- ✅ Custom CloudWatch metrics

**Best Practice:**
```
Use Target Tracking with:
- ASGAverageCPUUtilization (CPU-bound apps)
- ALBRequestCountPerTarget (request-bound apps)
- Custom metrics (business logic)
```

### Health Checks

**Types:**
- **EC2 Status Checks:** Instance reachable
- **ELB Health Checks:** Application responding
- **Custom Health Checks:** Application-specific logic

**Grace Period:**
- Default: 300 seconds
- Purpose: Allow instance to boot và warm up
- Recommendation: Set based on application startup time

**Ví dụ trong đề thi:**
> **Q:** Instances being terminated immediately after launch. Tại sao?
> 
> ✅ Health check grace period quá ngắn - tăng lên 600 seconds

---

## ⚡ Serverless Architectures

### AWS Lambda

#### **Core Concepts**

**Limits:**
```
Memory: 128 MB - 10,240 MB (1 MB increments)
Timeout: 1 second - 15 minutes (900 seconds)
Deployment package: 50 MB (zipped), 250 MB (unzipped)
/tmp storage: 512 MB - 10,240 MB
Concurrent executions: 1000 (default, can request increase)
```

**Ví dụ trong đề thi:**
> **Q:** Function cần process video (20 minutes). Lambda có phù hợp không?
> 
> ❌ **Không** - Lambda max 15 minutes. Dùng ECS/Batch instead.

#### **Invocation Types**

| Type | Use Case | Response | Example |
|------|----------|----------|---------|
| **Synchronous** | Real-time | Wait for response | API Gateway, ALB |
| **Asynchronous** | Fire-and-forget | Immediate return | S3 events, SNS |
| **Event Source Mapping** | Poll-based | Batch processing | SQS, Kinesis, DynamoDB Streams |

#### **Lambda@Edge**

**Purpose:** Run Lambda functions at CloudFront edge locations

**Use Cases:**
- Modify requests/responses
- A/B testing
- User authentication
- Image resizing

**Triggers:**
- Viewer Request (before CloudFront cache)
- Viewer Response (before returning to user)
- Origin Request (before forwarding to origin)
- Origin Response (after receiving from origin)

#### **Pricing**

```
Charges:
1. Number of requests: $0.20 per 1M requests
2. Duration: $0.00001667 per GB-second

Example:
- 1M requests/month
- 512 MB memory
- 1 second duration each
→ Cost = $0.20 + (1M × 0.5 GB × 1 sec × $0.00001667) = $8.54/month
```

**Free Tier:**
- 1M requests/month
- 400,000 GB-seconds/month

### Lambda Best Practices

1. ✅ **Minimize deployment package size** (faster cold starts)
2. ✅ **Use environment variables** for configuration
3. ✅ **Reuse execution context** (global variables, connections)
4. ✅ **Use Lambda Layers** for shared code/dependencies
5. ✅ **Enable VPC only if needed** (adds latency)
6. ✅ **Use provisioned concurrency** for consistent performance

**Ví dụ trong đề thi:**
> **Q:** Lambda function has slow cold starts. Cách nào improve performance?
> 
> ✅ Provisioned Concurrency (keeps functions warm)

---

## 🐳 Containers

### ECS vs EKS vs Fargate

| Feature | ECS | EKS | Fargate |
|---------|-----|-----|---------|
| **Orchestration** | AWS proprietary | Kubernetes | Serverless |
| **Learning Curve** | Easy | Steep | Easiest |
| **Control** | Medium | High | Low |
| **Pricing** | Free (pay for EC2) | $0.10/hour/cluster | Pay per vCPU/memory |
| **Use Case** | AWS-native apps | Kubernetes expertise | Simplest container deployment |

#### **ECS (Elastic Container Service)**

**Components:**
```
ECS Cluster
├─ Task Definition (blueprint)
│  ├─ Container definitions
│  ├─ CPU/Memory
│  └─ IAM Role
│
├─ Service (maintains desired count)
│  ├─ Task count
│  ├─ Load balancer integration
│  └─ Auto Scaling
│
└─ Tasks (running containers)
```

**Launch Types:**
- **EC2:** You manage EC2 instances
- **Fargate:** AWS manages infrastructure

**Ví dụ trong đề thi:**
> **Q:** Company wants containers với minimal operational overhead. Giải pháp nào?
> 
> ✅ **ECS with Fargate** (serverless, no EC2 management)

#### **EKS (Elastic Kubernetes Service)**

**Khi nào dùng:**
- ✅ Already using Kubernetes
- ✅ Need Kubernetes features
- ✅ Multi-cloud strategy
- ✅ Complex orchestration requirements

**Components:**
- Control Plane (managed by AWS)
- Worker Nodes (EC2 or Fargate)

**Ví dụ trong đề thi:**
> **Q:** Company migrating from on-premises Kubernetes. AWS service nào tương thích?
> 
> ✅ **EKS** (managed Kubernetes)

---

## 🏗️ Architecture Patterns

### Pattern 1: Classic 3-Tier Web Application

```
Internet
│
└─ CloudFront (CDN)
   │
   └─ Route 53 (DNS)
      │
      └─ Application Load Balancer
         │
         ├─ Public Subnet (Multi-AZ)
         │  └─ Auto Scaling Group (Web Tier)
         │     └─ EC2 Instances
         │
         ├─ Private Subnet (Multi-AZ)
         │  └─ Auto Scaling Group (App Tier)
         │     └─ EC2 Instances
         │
         └─ Private Subnet (Multi-AZ)
            └─ RDS Multi-AZ (Database Tier)
```

**Characteristics:**
- ✅ High availability (Multi-AZ)
- ✅ Scalable (Auto Scaling)
- ✅ Secure (private subnets)
- ✅ Fast (CloudFront CDN)

### Pattern 2: Serverless Web Application

```
Internet
│
└─ CloudFront
   │
   ├─ S3 (Static content)
   │
   └─ API Gateway
      │
      └─ Lambda Functions
         │
         └─ DynamoDB
```

**Characteristics:**
- ✅ No servers to manage
- ✅ Auto-scaling
- ✅ Pay per use
- ✅ High availability built-in

**Ví dụ trong đề thi:**
> **Q:** Startup cần deploy web app với minimal operational overhead và cost. Architecture nào phù hợp?
> 
> ✅ **Serverless:** S3 + CloudFront + API Gateway + Lambda + DynamoDB

### Pattern 3: Microservices với Containers

```
Internet
│
└─ Application Load Balancer
   │
   ├─ ECS Service 1 (User Service)
   │  └─ Fargate Tasks
   │
   ├─ ECS Service 2 (Order Service)
   │  └─ Fargate Tasks
   │
   └─ ECS Service 3 (Payment Service)
      └─ Fargate Tasks
```

**Characteristics:**
- ✅ Independent scaling
- ✅ Independent deployments
- ✅ Technology diversity
- ✅ Fault isolation

---

## ✅ Checklist

- [ ] Hiểu EC2 instance families (T, M, C, R, X, I, G, P)
- [ ] Biết pricing models (On-Demand, Reserved, Spot, Savings Plans)
- [ ] So sánh ALB vs NLB vs GWLB
- [ ] Hiểu Auto Scaling policies (Target Tracking recommended)
- [ ] Biết Lambda limits (15 min timeout, 10 GB memory)
- [ ] So sánh ECS vs EKS vs Fargate
- [ ] Nhớ 3 architecture patterns chính

---

*Tiếp theo: [D. Kiến Trúc Lưu Trữ](D-kien-truc-luu-tru.md)*
