# 🌳 L. QUYẾT ĐỊNH NHANH

> **Decision Trees & Flowcharts Để Chọn Đúng Service Trong < 30 Giây**

---

## 💻 COMPUTE DECISION TREE

```
Cần compute resource?
│
├─ Có server requirements cụ thể?
│  ├─ YES → EC2
│  │  ├─ Predictable 24/7 → Reserved Instances
│  │  ├─ Fault-tolerant → Spot Instances
│  │  └─ Unpredictable → On-Demand
│  │
│  └─ NO (serverless) ↓
│
├─ Runtime < 15 minutes?
│  ├─ YES → Lambda
│  └─ NO → Fargate / ECS
│
├─ Cần containers?
│  ├─ Kubernetes experience → EKS
│  ├─ AWS-native → ECS
│  └─ No management → Fargate
│
└─ Simple web app?
   └─ Elastic Beanstalk
```

---

## 💾 STORAGE DECISION TREE

```
Loại data?
│
├─ Files / Objects (images, videos, documents)
│  │
│  ├─ Access frequency?
│  │  ├─ Frequent (> 1/month) → S3 Standard
│  │  ├─ Infrequent (1/month) → S3 Standard-IA
│  │  ├─ Rare (1/quarter) → Glacier Instant Retrieval
│  │  ├─ Archive (1/year) → Glacier Flexible Retrieval
│  │  ├─ Long-term (7-10 years) → Glacier Deep Archive
│  │  └─ Unknown pattern → S3 Intelligent-Tiering
│  │
│  └─ Durability requirement?
│     ├─ Critical (Multi-AZ) → S3 Standard / Standard-IA
│     └─ Non-critical (Single AZ) → S3 One Zone-IA
│
├─ Block Storage (EC2 volumes)
│  │
│  ├─ Performance need?
│  │  ├─ High IOPS (> 16,000) → io2 Block Express
│  │  ├─ Balanced → gp3 (General Purpose SSD)
│  │  ├─ Throughput-optimized → st1 (HDD)
│  │  └─ Cold data → sc1 (HDD)
│  │
│  └─ Temporary data?
│     └─ EC2 Instance Store
│
├─ Shared File Storage
│  │
│  ├─ Operating System?
│  │  ├─ Linux → EFS
│  │  └─ Windows → FSx for Windows
│  │
│  ├─ High Performance Computing?
│  │  └─ FSx for Lustre
│  │
│  └─ Enterprise features?
│     └─ FSx for NetApp ONTAP
│
└─ Hybrid (On-premises + Cloud)
   │
   ├─ File shares → Storage Gateway (File Gateway)
   ├─ Block volumes → Storage Gateway (Volume Gateway)
   ├─ Tape backup → Storage Gateway (Tape Gateway)
   └─ Data migration → DataSync, Snow Family
```

---

## 🗄️ DATABASE DECISION TREE

```
Data structure?
│
├─ Relational (SQL)
│  │
│  ├─ Workload type?
│  │  ├─ OLTP (transactions) ↓
│  │  │  ├─ Need AWS optimized? → Aurora
│  │  │  ├─ Specific engine required? → RDS
│  │  │  └─ Serverless? → Aurora Serverless
│  │  │
│  │  └─ OLAP (analytics) → Redshift
│  │
│  └─ Availability requirement?
│     ├─ High availability → Multi-AZ
│     ├─ Read scaling → Read Replicas
│     └─ Global → Aurora Global Database
│
├─ NoSQL
│  │
│  ├─ Key-Value / Document
│  │  ├─ Millisecond latency → DynamoDB
│  │  ├─ Microsecond latency → DynamoDB + DAX
│  │  └─ MongoDB compatible → DocumentDB
│  │
│  ├─ Graph (relationships)
│  │  └─ Neptune
│  │
│  ├─ Time-Series (IoT, metrics)
│  │  └─ Timestream
│  │
│  └─ Ledger (immutable)
│     └─ QLDB
│
└─ Caching
   │
   ├─ Complex data structures → ElastiCache Redis
   └─ Simple key-value → ElastiCache Memcached
```

---

## 🌐 NETWORKING DECISION TREE

```
Networking requirement?
│
├─ Load Balancing
│  │
│  ├─ Protocol?
│  │  ├─ HTTP/HTTPS → ALB
│  │  ├─ TCP/UDP → NLB
│  │  └─ Third-party appliances → GWLB
│  │
│  └─ Features needed?
│     ├─ Path-based routing → ALB
│     ├─ Static IP required → NLB
│     └─ Extreme performance → NLB
│
├─ Content Delivery
│  │
│  ├─ Static content caching → CloudFront
│  ├─ TCP/UDP acceleration → Global Accelerator
│  └─ Dynamic content → CloudFront + ALB
│
├─ DNS Routing
│  │
│  └─ Route 53 Routing Policies:
│     ├─ Simple routing → Simple
│     ├─ Multiple resources, weighted → Weighted
│     ├─ Lowest latency → Latency-Based
│     ├─ Active-passive failover → Failover
│     ├─ Geographic restrictions → Geolocation
│     ├─ Proximity-based → Geoproximity
│     └─ Multiple IPs, health checks → Multi-Value
│
├─ VPC Connectivity
│  │
│  ├─ Internet access?
│  │  ├─ Public subnet → Internet Gateway
│  │  └─ Private subnet → NAT Gateway
│  │
│  ├─ VPC to VPC?
│  │  ├─ Simple connection → VPC Peering
│  │  └─ Hub-and-spoke → Transit Gateway
│  │
│  └─ On-premises to AWS?
│     ├─ Encrypted over internet → Site-to-Site VPN
│     ├─ Dedicated connection → Direct Connect
│     └─ Hybrid → Direct Connect + VPN (backup)
│
└─ Private Connectivity
   │
   ├─ AWS services privately → VPC Endpoints
   │  ├─ S3, DynamoDB → Gateway Endpoint (free)
   │  └─ Other services → Interface Endpoint (PrivateLink)
   │
   └─ Third-party SaaS → PrivateLink
```

---

## 🔐 SECURITY DECISION TREE

```
Security requirement?
│
├─ Identity & Access
│  │
│  ├─ AWS resources access?
│  │  ├─ Human users → IAM Users + MFA
│  │  ├─ Applications/Services → IAM Roles
│  │  └─ External users → Federation (SAML, OIDC)
│  │
│  └─ Cross-account access?
│     └─ Cross-account IAM Roles
│
├─ Encryption
│  │
│  ├─ At rest?
│  │  ├─ AWS manages keys → SSE-S3, AWS Managed KMS
│  │  ├─ Customer manages keys → Customer Managed KMS
│  │  └─ Customer provides keys → SSE-C
│  │
│  └─ In transit?
│     ├─ HTTPS/TLS → ACM (certificates)
│     └─ VPN → Site-to-Site VPN, Client VPN
│
├─ Monitoring & Compliance
│  │
│  ├─ API audit trail → CloudTrail
│  ├─ Configuration tracking → AWS Config
│  ├─ Resource monitoring → CloudWatch
│  └─ Compliance dashboard → Security Hub
│
├─ Threat Detection & Protection
│  │
│  ├─ Intelligent threat detection → GuardDuty
│  ├─ Vulnerability scanning → Inspector
│  ├─ PII data discovery → Macie
│  ├─ Web application firewall → WAF
│  └─ DDoS protection → Shield
│
└─ Secrets Management
   │
   ├─ Database credentials (auto-rotate) → Secrets Manager
   └─ Configuration data → Systems Manager Parameter Store
```

---

## 📈 HIGH AVAILABILITY DECISION TREE

```
Availability requirement?
│
├─ Compute HA
│  │
│  ├─ Single instance → Multi-AZ with Auto Scaling
│  ├─ Load balancing → ELB + Auto Scaling
│  └─ Health checks → ELB Health Checks
│
├─ Database HA
│  │
│  ├─ RDS
│  │  ├─ Automatic failover → Multi-AZ
│  │  ├─ Read scaling → Read Replicas
│  │  └─ Global → Aurora Global Database
│  │
│  └─ DynamoDB
│     ├─ Regional → DynamoDB (built-in Multi-AZ)
│     └─ Global → DynamoDB Global Tables
│
├─ Storage HA
│  │
│  ├─ S3 → Built-in Multi-AZ (except One Zone-IA)
│  ├─ EBS → Snapshots to S3 (Multi-AZ)
│  └─ EFS → Built-in Multi-AZ
│
└─ DNS Failover
   └─ Route 53 Health Checks + Failover Routing
```

---

## 💰 COST OPTIMIZATION DECISION TREE

```
Cost optimization need?
│
├─ Compute Costs
│  │
│  ├─ Usage pattern?
│  │  ├─ Steady-state (24/7) → Reserved Instances (1-3 year)
│  │  ├─ Predictable schedule → Scheduled Reserved
│  │  ├─ Flexible → Savings Plans
│  │  ├─ Fault-tolerant → Spot Instances (up to 90% off)
│  │  └─ Unpredictable → On-Demand
│  │
│  └─ Serverless option?
│     └─ Lambda, Fargate (pay per use)
│
├─ Storage Costs
│  │
│  ├─ S3 optimization
│  │  ├─ Unknown pattern → S3 Intelligent-Tiering
│  │  ├─ Lifecycle policies → Auto-tier to IA/Glacier
│  │  └─ Delete old versions → Lifecycle expiration
│  │
│  └─ EBS optimization
│     ├─ Right-size volumes → CloudWatch metrics
│     └─ Delete unused snapshots
│
├─ Database Costs
│  │
│  ├─ Unpredictable workload → Aurora Serverless
│  ├─ DynamoDB → On-Demand mode
│  └─ Reserved capacity → RDS Reserved Instances
│
└─ Data Transfer Costs
   │
   ├─ Reduce egress → CloudFront (cache at edge)
   ├─ Same-region transfer → Free within AZ
   └─ S3 Transfer Acceleration → For global uploads
```

---

## ⚡ PERFORMANCE OPTIMIZATION DECISION TREE

```
Performance bottleneck?
│
├─ Compute Performance
│  │
│  ├─ CPU-bound → C5 instances (Compute Optimized)
│  ├─ Memory-bound → R5 instances (Memory Optimized)
│  ├─ Storage-bound → I3 instances (Storage Optimized)
│  └─ GPU-needed → P3, G4 instances
│
├─ Database Performance
│  │
│  ├─ Read-heavy
│  │  ├─ RDS → Read Replicas
│  │  ├─ DynamoDB → DAX (microsecond latency)
│  │  └─ Caching → ElastiCache
│  │
│  └─ Write-heavy
│     ├─ DynamoDB → Provisioned capacity
│     └─ Aurora → Multi-master
│
├─ Storage Performance
│  │
│  ├─ High IOPS → EBS io2 Block Express
│  ├─ High throughput → EBS st1
│  └─ S3 performance → Multipart upload, Transfer Acceleration
│
├─ Network Performance
│  │
│  ├─ Latency
│  │  ├─ Global users → CloudFront
│  │  ├─ Caching → ElastiCache, CloudFront
│  │  └─ Proximity → Multi-region deployment
│  │
│  └─ Throughput
│     ├─ Enhanced Networking → SR-IOV
│     └─ Placement Groups → Cluster placement
│
└─ Application Performance
   │
   ├─ Caching strategy
   │  ├─ Database queries → ElastiCache
   │  ├─ Static content → CloudFront
   │  └─ API responses → API Gateway caching
   │
   └─ Async processing
      ├─ Decouple → SQS
      └─ Background jobs → Lambda, SQS
```

---

## ✅ Cách Sử Dụng Decision Trees

### Trong Phòng Thi

1. **Đọc câu hỏi** - Identify requirement chính
2. **Tìm decision tree** phù hợp (compute, storage, database, etc.)
3. **Follow the tree** - Trả lời Yes/No questions
4. **Arrive at answer** trong < 30 giây

### Ví Dụ

**Câu hỏi:** "Application needs to store frequently accessed images with high durability"

**Decision Process:**
```
Storage Decision Tree
└─ Files/Objects? YES
   └─ Access frequency? Frequent
      └─ Durability? High (Multi-AZ)
         → Answer: S3 Standard
```

---

*Practice với decision trees này để giảm thời gian làm bài từ 2 phút/câu xuống 30 giây/câu!*
