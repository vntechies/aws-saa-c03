# 🖥️ COMPUTE SERVICES - AWS SAA-C03

## 📌 Tổng Quan

Compute services là **TIM MẠCH** của AWS và chiếm **20-25% câu hỏi** trong kỳ thi. Phải nắm vững 100% nội dung này.

---

## 1️⃣ AMAZON EC2 (Elastic Compute Cloud)

### 🎯 Khái Niệm Cơ Bản

**EC2** = Virtual servers in the cloud. Cung cấp computing capacity có thể scale.

### 📊 EC2 Instance Types

#### Cách Đặt Tên: `m5.2xlarge`
- **m** = Instance family
- **5** = Generation
- **2xlarge** = Size

#### Instance Families (Phải Thuộc)

| Family | Tên | Use Case | Mnemonic |
|--------|-----|----------|----------|
| **T3/T4g** | Burstable | Web servers, dev/test | **T**hink **T**iny |
| **M5/M6** | General Purpose | Balanced workloads | **M**ost scenarios |
| **C5/C6** | Compute Optimized | CPU-intensive | **C**ompute |
| **R5/R6** | Memory Optimized | In-memory databases | **R**AM |
| **X1/X2** | Memory Optimized | SAP HANA, big data | E**x**treme memory |
| **P3/P4** | Accelerated Computing | ML training, HPC | **P**ower (GPU) |
| **G4/G5** | Accelerated Computing | Graphics, ML inference | **G**raphics |
| **I3/I4** | Storage Optimized | NoSQL, data warehousing | **I**OPS |
| **D2/D3** | Storage Optimized | MapReduce, HDFS | **D**ense storage |

#### 🎯 Chọn Instance Type Theo Scenario

```
Scenario → Instance Type

High CPU usage → C5/C6
High memory usage → R5/R6
Balanced workload → M5/M6
ML training → P3/P4
Graphics rendering → G4/G5
High IOPS → I3/I4
Big data storage → D2/D3
Variable workload → T3/T4g (burstable)
```

### 💰 EC2 Pricing Models

#### 1. **On-Demand** 💵
- **Giá**: Cao nhất, pay per second (Linux) or per hour (Windows)
- **Use case**: Short-term, unpredictable workloads
- **Ưu điểm**: No commitment, flexibility
- **Nhược điểm**: Đắt nhất

#### 2. **Reserved Instances (RI)** 💎
- **Discount**: 40-60% so với On-Demand
- **Commitment**: 1 hoặc 3 năm
- **Types**:
  - **Standard RI**: Discount cao nhất, không đổi được instance type
  - **Convertible RI**: Discount thấp hơn, có thể đổi instance type
  - **Scheduled RI**: Cho workloads chạy theo lịch cố định

**Payment Options**:
- **All Upfront**: Trả hết trước → Discount cao nhất
- **Partial Upfront**: Trả một phần → Discount trung bình
- **No Upfront**: Không trả trước → Discount thấp nhất

**🎯 Exam Tip**: "Predictable workload" + "1-3 years" = Reserved Instances

#### 3. **Spot Instances** 🎰
- **Discount**: 50-90% so với On-Demand
- **Risk**: AWS có thể terminate với 2-minute warning
- **Use case**: Fault-tolerant, flexible workloads
- **Ví dụ**: Batch processing, big data, CI/CD

**Spot Fleet**: Tập hợp Spot + On-Demand instances
- Strategies: `lowestPrice`, `diversified`, `capacityOptimized`

**🎯 Exam Tip**: "Cost-effective" + "fault-tolerant" = Spot Instances

#### 4. **Dedicated Hosts** 🏢
- **Giá**: Đắt nhất
- **Use case**: Licensing requirements, compliance
- **Control**: Full control over instance placement

#### 5. **Dedicated Instances** 🔒
- Hardware dedicated nhưng không control được placement
- Rẻ hơn Dedicated Hosts

#### 6. **Savings Plans** 💰
- **Discount**: 40-66%
- **Commitment**: 1 hoặc 3 năm, commit usage ($/hour)
- **Types**:
  - **Compute Savings Plans**: Flexible (instance family, region, OS)
  - **EC2 Instance Savings Plans**: Less flexible, higher discount

### 🔄 Pricing Comparison Table

| Model | Discount | Commitment | Flexibility | Use Case |
|-------|----------|------------|-------------|----------|
| On-Demand | 0% | None | Highest | Short-term, unpredictable |
| Reserved | 40-60% | 1-3 years | Low | Steady-state |
| Spot | 50-90% | None | Medium | Fault-tolerant |
| Savings Plans | 40-66% | 1-3 years | High | Flexible commitment |
| Dedicated | 0% | None | Low | Compliance |

---

## 2️⃣ EC2 STORAGE OPTIONS

### 📦 EBS (Elastic Block Store) - Chi tiết ở file 02

**Types**:
1. **gp3/gp2** (General Purpose SSD) - Balanced
2. **io2/io1** (Provisioned IOPS SSD) - High performance
3. **st1** (Throughput Optimized HDD) - Big data
4. **sc1** (Cold HDD) - Infrequent access

### 💾 Instance Store
- **Physical disk** attached to host
- **Ephemeral**: Data mất khi instance stop/terminate
- **Use case**: Cache, buffers, temporary data
- **Performance**: Very high IOPS

**🎯 Exam Tip**: "Temporary data" + "high IOPS" = Instance Store

---

## 3️⃣ EC2 NETWORKING

### 🌐 Elastic Network Interface (ENI)
- Virtual network card
- Có thể attach/detach
- **Use case**: Management network, dual-homed instances

### 🚀 Enhanced Networking
- **SR-IOV**: Single Root I/O Virtualization
- **Higher bandwidth, lower latency**
- **Types**:
  - **ENA** (Elastic Network Adapter): Up to 100 Gbps
  - **Intel 82599 VF**: Up to 10 Gbps (legacy)

### ⚡ Elastic Fabric Adapter (EFA)
- For **HPC** (High Performance Computing)
- OS-bypass, lower latency
- **Use case**: MPI applications, ML training

---

## 4️⃣ EC2 PLACEMENT GROUPS

### 📍 Cluster Placement Group
- **Đặc điểm**: Instances trong cùng 1 AZ, gần nhau
- **Ưu điểm**: Low latency, high network throughput (10 Gbps)
- **Nhược điểm**: Single AZ (not HA)
- **Use case**: HPC, big data

### 📍 Spread Placement Group
- **Đặc điểm**: Instances trên different hardware
- **Giới hạn**: Max 7 instances per AZ
- **Ưu điểm**: High availability
- **Use case**: Critical applications

### 📍 Partition Placement Group
- **Đặc điểm**: Instances trong partitions (racks) khác nhau
- **Giới hạn**: Max 7 partitions per AZ
- **Use case**: Hadoop, Cassandra, Kafka

```
Cluster    → Low latency, same AZ
Spread     → High availability, max 7/AZ
Partition  → Big data, max 7 partitions/AZ
```

---

## 5️⃣ EC2 AUTO SCALING

### 🔄 Auto Scaling Components

#### 1. **Launch Template / Launch Configuration**
- **Launch Template** (newer, recommended)
  - Versioning
  - Multiple instance types
  - Spot + On-Demand mix
- **Launch Configuration** (legacy)
  - No versioning
  - Single instance type

#### 2. **Auto Scaling Group (ASG)**
- **Min, Max, Desired Capacity**
- **Health checks**: EC2, ELB
- **Scaling Policies**

### 📈 Scaling Policies

#### 1. **Target Tracking Scaling**
- Maintain metric at target value
- **Example**: Keep CPU at 50%
- **Easiest to setup**

#### 2. **Step Scaling**
- Scale based on CloudWatch alarms
- Different steps for different thresholds

#### 3. **Simple Scaling**
- Single scaling adjustment
- Cooldown period

#### 4. **Scheduled Scaling**
- Scale at specific times
- **Example**: Scale up at 9 AM, down at 6 PM

#### 5. **Predictive Scaling**
- ML-based, forecast traffic
- Proactive scaling

### 🎯 Scaling Cooldown
- **Default**: 300 seconds
- Prevents rapid scaling
- **Tip**: Use pre-configured AMI để giảm cooldown

### 🏥 Health Checks
- **EC2 Health Check**: Instance status
- **ELB Health Check**: Application health (recommended)

**🎯 Exam Tip**: "Automatically scale" = Auto Scaling Group

---

## 6️⃣ AWS LAMBDA

### ⚡ Khái Niệm

**Lambda** = Serverless compute, chạy code không cần quản lý servers.

### 🎯 Key Features

- **Languages**: Python, Node.js, Java, Go, Ruby, C#, PowerShell
- **Max execution time**: 15 minutes
- **Memory**: 128 MB - 10 GB
- **Pricing**: Pay per request + compute time
- **Free tier**: 1M requests/month + 400,000 GB-seconds

### 🔄 Invocation Types

#### 1. **Synchronous**
- Wait for response
- **Example**: API Gateway, ALB

#### 2. **Asynchronous**
- Lambda queues request
- **Example**: S3, SNS, EventBridge
- Retries: 2 times

#### 3. **Event Source Mapping**
- Lambda polls source
- **Example**: SQS, Kinesis, DynamoDB Streams

### 📦 Lambda Layers
- Reusable code/libraries
- Max 5 layers per function
- Max 250 MB total

### 🌐 Lambda@Edge
- Run Lambda at CloudFront edge locations
- **Use case**: A/B testing, authentication, redirects

### 💾 Lambda Storage

- **/tmp**: 512 MB - 10 GB (ephemeral)
- **EFS**: Persistent storage
- **Environment variables**: 4 KB limit

### ⚙️ Lambda Configuration

#### Concurrency
- **Reserved Concurrency**: Guarantee capacity
- **Provisioned Concurrency**: Pre-warmed instances (no cold start)
- **Default limit**: 1,000 concurrent executions per region

#### VPC Integration
- Lambda có thể access VPC resources
- Cần **VPC endpoints** cho AWS services

### 🎯 Lambda Use Cases

```
Real-time file processing → S3 + Lambda
Real-time stream processing → Kinesis + Lambda
API backend → API Gateway + Lambda
Scheduled tasks → EventBridge + Lambda
Webhooks → ALB + Lambda
```

**🎯 Exam Tip**: 
- "Serverless" = Lambda
- "No infrastructure management" = Lambda
- "Event-driven" = Lambda

---

## 7️⃣ AWS ELASTIC BEANSTALK

### 🌱 Khái Niệm

**Elastic Beanstalk** = PaaS, deploy applications without managing infrastructure.

### 🎯 Key Features

- **Supported platforms**: Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker
- **Components**:
  - Application
  - Application Version
  - Environment (Web Server / Worker)

### 🔄 Deployment Options

#### 1. **All at Once**
- Deploy to all instances simultaneously
- **Downtime**: Yes
- **Fastest**

#### 2. **Rolling**
- Deploy in batches
- **Downtime**: No
- Reduced capacity during deployment

#### 3. **Rolling with Additional Batch**
- Add new instances first
- **Downtime**: No
- Full capacity maintained

#### 4. **Immutable**
- New instances in new ASG
- **Downtime**: No
- **Safest**, easy rollback

#### 5. **Blue/Green**
- New environment, swap URLs
- **Downtime**: Minimal
- **Zero downtime**

#### 6. **Traffic Splitting**
- Canary testing
- Send % of traffic to new version

### 🎯 Exam Tips

```
Fast deployment → All at Once
No downtime, cost-effective → Rolling
Full capacity + no downtime → Rolling with Additional Batch
Safest, easy rollback → Immutable
Zero downtime → Blue/Green
Testing new version → Traffic Splitting
```

---

## 8️⃣ AMAZON ECS (Elastic Container Service)

### 🐳 Khái Niệm

**ECS** = Managed container orchestration service (Docker).

### 🎯 Launch Types

#### 1. **EC2 Launch Type**
- You manage EC2 instances
- More control
- **Use case**: Cost optimization, specific instance requirements

#### 2. **Fargate Launch Type**
- AWS manages infrastructure
- **Serverless**
- **Use case**: Simplicity, no infrastructure management

### 📦 ECS Components

- **Task Definition**: Blueprint (Docker image, CPU, memory, ports)
- **Task**: Running instance of task definition
- **Service**: Maintains desired number of tasks
- **Cluster**: Logical grouping of tasks/services

### 🔄 ECS vs EKS vs Fargate

| Feature | ECS | EKS | Fargate |
|---------|-----|-----|---------|
| Orchestration | AWS proprietary | Kubernetes | Serverless |
| Complexity | Simple | Complex | Simplest |
| Portability | AWS only | Multi-cloud | AWS only |
| Cost | Low | Higher | Pay per task |

### 🎯 Exam Tips

```
Docker on AWS → ECS
Kubernetes on AWS → EKS
Serverless containers → Fargate
No infrastructure management → Fargate
Full control → ECS on EC2
```

---

## 9️⃣ AMAZON EKS (Elastic Kubernetes Service)

### ☸️ Khái Niệm

**EKS** = Managed Kubernetes service.

### 🎯 Key Features

- **Kubernetes-compatible**: Use standard K8s tools
- **Multi-AZ control plane**: High availability
- **Node types**:
  - **Managed Node Groups**: AWS manages EC2 instances
  - **Self-managed Nodes**: You manage EC2 instances
  - **Fargate**: Serverless

### 🔄 EKS vs ECS

```
Use ECS when:
- AWS-native solution
- Simpler setup
- Lower cost

Use EKS when:
- Already using Kubernetes
- Multi-cloud portability
- Advanced K8s features needed
```

---

## 🔟 AWS BATCH

### 📊 Khái Niệm

**AWS Batch** = Managed batch processing at any scale.

### 🎯 Key Features

- **Components**:
  - **Jobs**: Units of work
  - **Job Definitions**: How to run jobs
  - **Job Queues**: Queue jobs
  - **Compute Environments**: EC2 or Fargate

- **Scheduling**: Automatically schedules jobs
- **Scaling**: Dynamically provisions resources

### 🔄 Batch vs Lambda

```
AWS Batch:
- Long-running jobs (>15 min)
- Docker containers
- Batch processing

Lambda:
- Short tasks (<15 min)
- Event-driven
- Serverless
```

---

## 1️⃣1️⃣ AMAZON LIGHTSAIL

### 💡 Khái Niệm

**Lightsail** = Simplified VPS (Virtual Private Server), easy to use.

### 🎯 Key Features

- **Pre-configured**: WordPress, LAMP, Node.js
- **Fixed pricing**: Predictable monthly cost
- **Includes**: Compute, storage, networking, DNS

### 🔄 Lightsail vs EC2

```
Lightsail:
- Beginners
- Simple applications
- Predictable pricing

EC2:
- Advanced users
- Complex architectures
- Pay-as-you-go
```

---

## 📋 COMPUTE SERVICES COMPARISON

| Service | Type | Use Case | Management |
|---------|------|----------|------------|
| **EC2** | IaaS | General purpose VMs | You manage |
| **Lambda** | FaaS | Event-driven, serverless | AWS manages |
| **Elastic Beanstalk** | PaaS | Deploy apps easily | AWS manages infrastructure |
| **ECS** | Container | Docker orchestration | Flexible |
| **EKS** | Container | Kubernetes | Complex |
| **Fargate** | Serverless | Serverless containers | AWS manages |
| **Batch** | Batch | Batch processing | AWS manages |
| **Lightsail** | VPS | Simple websites | Simplified |

---

## 🎯 EXAM SCENARIOS - COMPUTE

### Scenario 1: Cost Optimization
**Question**: "Most cost-effective solution for predictable workload running 24/7 for 3 years?"

**Answer**: Reserved Instances (3-year, All Upfront)

---

### Scenario 2: High Availability
**Question**: "Deploy application across multiple AZs with automatic failover?"

**Answer**: Auto Scaling Group + Multi-AZ ELB

---

### Scenario 3: Serverless
**Question**: "Process images uploaded to S3 without managing servers?"

**Answer**: S3 + Lambda

---

### Scenario 4: Batch Processing
**Question**: "Run nightly batch jobs, cost-effective, can tolerate interruptions?"

**Answer**: Spot Instances or AWS Batch with Spot

---

### Scenario 5: Containers
**Question**: "Run Docker containers without managing infrastructure?"

**Answer**: ECS on Fargate

---

### Scenario 6: HPC
**Question**: "Low latency, high network throughput for HPC workload?"

**Answer**: Cluster Placement Group + Enhanced Networking (ENA) + EFA

---

## ⚠️ COMMON MISTAKES

1. ❌ **Chọn On-Demand cho steady-state workload** → Nên dùng Reserved Instances
2. ❌ **Dùng Lambda cho long-running tasks** → Max 15 minutes, dùng ECS/Batch
3. ❌ **Quên Spot Instances cho fault-tolerant workloads** → Tiết kiệm 50-90%
4. ❌ **Không dùng Auto Scaling** → Mất HA và cost optimization
5. ❌ **Dùng EKS khi ECS đã đủ** → EKS phức tạp và đắt hơn

---

## 🎓 TIPS ĐẶC BIỆT

### 1. **Nhận Diện Keywords**

```
"Serverless" → Lambda, Fargate
"Containers" → ECS, EKS
"Batch processing" → AWS Batch, Spot Instances
"Cost-effective" → Reserved, Spot, Auto Scaling
"High availability" → Multi-AZ, Auto Scaling, ELB
"Low latency" → Placement Groups, Enhanced Networking
```

### 2. **Decision Tree**

```
Need compute?
├─ Serverless? → Lambda
├─ Containers?
│  ├─ Kubernetes? → EKS
│  └─ Simple? → ECS (Fargate)
├─ VMs?
│  ├─ Simple? → Lightsail
│  └─ Advanced? → EC2
└─ Batch? → AWS Batch
```

### 3. **Cost Optimization Formula**

```
Predictable + Long-term = Reserved Instances / Savings Plans
Fault-tolerant = Spot Instances
Variable load = Auto Scaling
Short tasks = Lambda
```

---

## 📚 CHECKLIST

- [ ] Hiểu rõ 6 EC2 pricing models
- [ ] Phân biệt được instance families (T, M, C, R, X, P, G, I, D)
- [ ] Biết 3 types của Placement Groups
- [ ] Nắm vững Auto Scaling policies
- [ ] Hiểu Lambda limitations (15 min, 10 GB memory)
- [ ] Phân biệt ECS vs EKS vs Fargate
- [ ] Biết Elastic Beanstalk deployment strategies
- [ ] Hiểu khi nào dùng Spot vs Reserved vs On-Demand

---

**Next**: [02-storage-services.md](02-storage-services.md) - Storage là phần quan trọng tiếp theo! 🚀
