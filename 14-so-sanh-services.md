# 🔀 SO SÁNH CÁC SERVICES - AWS SAA-C03

## 📌 Tổng Quan

File này là **QUAN TRỌNG NHẤT** cho kỳ thi! Giúp bạn phân biệt các services tương tự và chọn đúng trong exam.

---

## 1️⃣ COMPUTE SERVICES COMPARISON

### EC2 vs Lambda vs Fargate

| Feature | EC2 | Lambda | Fargate |
|---------|-----|--------|---------|
| **Type** | VMs | Serverless functions | Serverless containers |
| **Management** | You manage | AWS manages | AWS manages |
| **Scaling** | Manual/Auto Scaling | Automatic | Automatic |
| **Pricing** | Per hour/second | Per request + duration | Per vCPU/memory |
| **Max runtime** | Unlimited | 15 minutes | Unlimited |
| **Use case** | Long-running, full control | Event-driven, short tasks | Containers without management |

**Decision Tree**:
```
Need compute?
├─ Full control, long-running? → EC2
├─ Event-driven, <15 min? → Lambda
└─ Containers, no management? → Fargate
```

### ECS vs EKS vs Fargate

| Feature | ECS | EKS | Fargate |
|---------|-----|-----|---------|
| **Orchestration** | AWS proprietary | Kubernetes | Serverless |
| **Complexity** | Simple | Complex | Simplest |
| **Portability** | AWS only | Multi-cloud | AWS only |
| **Control** | High | Highest | Low |
| **Cost** | Low | Higher (control plane $0.10/hr) | Pay per task |

**When to use**:
- **ECS**: AWS-native, simpler, cost-effective
- **EKS**: Already using K8s, multi-cloud, advanced features
- **Fargate**: No infrastructure management

---

## 2️⃣ STORAGE SERVICES COMPARISON

### S3 vs EBS vs EFS vs Instance Store

| Feature | S3 | EBS | EFS | Instance Store |
|---------|----|----|-----|----------------|
| **Type** | Object | Block | File | Block |
| **Attachment** | Many (via API) | 1 instance (except Multi-Attach) | Many instances | 1 instance |
| **AZ** | Multi-AZ | Single AZ | Multi-AZ | Single AZ |
| **Persistence** | Yes | Yes | Yes | No (ephemeral) |
| **Max size** | Unlimited | 16 TB | Unlimited | Limited by instance |
| **Use case** | Static files, backups | Boot volumes, databases | Shared files | Cache, temporary |

**Decision Tree**:
```
Need storage?
├─ Object storage? → S3
├─ Block storage?
│  ├─ Shared? → EFS (Linux) / FSx (Windows)
│  ├─ Persistent? → EBS
│  └─ Temporary? → Instance Store
└─ File system? → EFS / FSx
```

### S3 Storage Classes Comparison

| Class | Retrieval Time | Cost | Use Case |
|-------|----------------|------|----------|
| **Standard** | Instant | $$$$$ | Frequent access |
| **Intelligent-Tiering** | Instant | $$$$ | Unknown pattern |
| **Standard-IA** | Instant | $$$ | Infrequent access |
| **One Zone-IA** | Instant | $$ | Infrequent, non-critical |
| **Glacier Instant** | Instant | $$ | Archive, instant access |
| **Glacier Flexible** | Minutes-hours | $ | Archive, rare access |
| **Glacier Deep Archive** | 12-48 hours | $ | Long-term archive |

**Keywords**:
- "Unknown access pattern" → Intelligent-Tiering
- "Infrequent access" → Standard-IA
- "Archive" + "instant" → Glacier Instant Retrieval
- "Lowest cost" + "archive" → Glacier Deep Archive

### EBS Volume Types Comparison

| Type | IOPS | Throughput | Use Case | Boot? |
|------|------|------------|----------|-------|
| **gp3** | 3K-16K | 125-1,000 MB/s | General purpose | ✅ |
| **gp2** | 3-16K | 128-250 MB/s | General purpose | ✅ |
| **io2** | Up to 64K | Up to 1,000 MB/s | Databases, critical | ✅ |
| **io1** | Up to 64K | Up to 1,000 MB/s | Databases | ✅ |
| **st1** | 500 | 500 MB/s | Big data, sequential | ❌ |
| **sc1** | 250 | 250 MB/s | Cold data | ❌ |

**Decision**:
- General purpose → **gp3** (newer, better)
- High IOPS → **io2**
- Throughput-heavy → **st1**
- Cold, infrequent → **sc1**

---

## 3️⃣ DATABASE SERVICES COMPARISON

### RDS vs DynamoDB vs Aurora

| Feature | RDS | DynamoDB | Aurora |
|---------|-----|----------|--------|
| **Type** | Relational (SQL) | NoSQL (Key-Value) | Relational (SQL) |
| **Management** | Managed | Fully managed | Fully managed |
| **Scaling** | Vertical (manual), Horizontal (Read Replicas) | Horizontal (automatic) | Horizontal (automatic) |
| **Performance** | Standard | Millisecond latency | 5x MySQL, 3x PostgreSQL |
| **Cost** | $$ | $ (On-Demand), $$ (Provisioned) | $$$ (20% more than RDS) |
| **Use case** | OLTP, traditional apps | Serverless, high scale | High performance RDBMS |

**Decision Tree**:
```
Need database?
├─ Relational (SQL)?
│  ├─ High performance? → Aurora
│  ├─ Specific engine? → RDS
│  └─ Analytics? → Redshift
├─ NoSQL?
│  ├─ Key-value, serverless? → DynamoDB
│  ├─ Document (MongoDB)? → DocumentDB
│  └─ Graph? → Neptune
└─ In-memory cache? → ElastiCache
```

### RDS Read Replicas vs Multi-AZ

| Feature | Read Replicas | Multi-AZ |
|---------|---------------|----------|
| **Purpose** | Scale reads | High availability |
| **Replication** | Asynchronous | Synchronous |
| **Readable** | Yes | No (standby) |
| **Failover** | Manual | Automatic (60-120s) |
| **Cross-Region** | Yes | No |
| **Use case** | Read-heavy workloads | DR, HA |

**Keywords**:
- "Read scaling" → Read Replicas
- "High availability" → Multi-AZ
- "Automatic failover" → Multi-AZ

### ElastiCache: Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Data types** | Rich (lists, sets, sorted sets) | Simple (strings) |
| **Persistence** | Yes | No |
| **Replication** | Yes (Multi-AZ) | No |
| **Backup** | Yes | No |
| **Pub/Sub** | Yes | No |
| **Multi-threading** | No | Yes |
| **Use case** | Session store, leaderboards | Simple cache, multi-threaded |

**Decision**:
- Need persistence → **Redis**
- Pub/Sub → **Redis**
- Leaderboards (sorted sets) → **Redis**
- Simple cache, multi-threaded → **Memcached**

---

## 4️⃣ NETWORKING SERVICES COMPARISON

### Security Groups vs NACLs

| Feature | Security Groups | NACLs |
|---------|----------------|-------|
| **Level** | Instance (ENI) | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow + Deny |
| **Processing** | All rules | Rules in order |
| **Default** | Deny all inbound | Allow all |
| **Use case** | Instance security | Subnet security, block IPs |

**Keywords**:
- "Block specific IP" → NACL (deny rule)
- "Stateful" → Security Group
- "Stateless" → NACL

### ALB vs NLB vs GWLB

| Feature | ALB | NLB | GWLB |
|---------|-----|-----|------|
| **Layer** | 7 (HTTP/HTTPS) | 4 (TCP/UDP/TLS) | 3 (IP) |
| **Latency** | ~400 ms | ~100 ms | Minimal |
| **Static IP** | No | Yes (Elastic IP) | No |
| **Preserve source IP** | Via headers | Yes | Yes |
| **Path routing** | Yes | No | No |
| **WebSockets** | Yes | Yes | No |
| **Use case** | Web apps, microservices | Performance, static IP | Security appliances |

**Decision**:
- HTTP/HTTPS + path routing → **ALB**
- TCP/UDP + high performance → **NLB**
- Static IP required → **NLB**
- Security appliances → **GWLB**

### CloudFront vs Global Accelerator

| Feature | CloudFront | Global Accelerator |
|---------|------------|-------------------|
| **Purpose** | CDN, cache content | Accelerate TCP/UDP |
| **Caching** | Yes | No |
| **Static IP** | No | Yes (2 Anycast IPs) |
| **Protocols** | HTTP, HTTPS, WebSockets | TCP, UDP |
| **Use case** | Static content, APIs | Gaming, IoT, VoIP |

**Keywords**:
- "Cache" + "global" → CloudFront
- "Static IP" + "global" → Global Accelerator
- "Non-HTTP" → Global Accelerator

### VPN vs Direct Connect

| Feature | VPN | Direct Connect |
|---------|-----|----------------|
| **Type** | Encrypted over internet | Dedicated physical connection |
| **Setup time** | Minutes | Weeks/months |
| **Bandwidth** | Up to 1.25 Gbps/tunnel | 50 Mbps - 100 Gbps |
| **Latency** | Higher | Lower |
| **Cost** | Low | High |
| **Encryption** | Yes (IPsec) | No (add VPN over DX) |
| **Use case** | Quick setup, backup | High throughput, consistent |

**Decision**:
- Quick setup, encrypted → **VPN**
- High bandwidth, low latency → **Direct Connect**
- Both → **VPN as backup** for Direct Connect

---

## 5️⃣ SECURITY SERVICES COMPARISON

### CloudTrail vs CloudWatch vs Config

| Feature | CloudTrail | CloudWatch | Config |
|---------|-----------|-----------|--------|
| **Purpose** | API audit | Monitoring | Configuration tracking |
| **Focus** | Who did what | Metrics, logs, alarms | Resource state |
| **Data** | API calls | Metrics, logs | Configuration history |
| **Use case** | Security audit, compliance | Performance monitoring | Compliance, change tracking |

**Keywords**:
- "Who did what" → CloudTrail
- "Monitoring" + "alarms" → CloudWatch
- "Configuration compliance" → Config

### IAM Role vs IAM User vs IAM Group

| Feature | IAM User | IAM Group | IAM Role |
|---------|----------|-----------|----------|
| **For** | Person | Collection of users | AWS service, external identity |
| **Credentials** | Long-term (password, access keys) | N/A | Temporary (STS) |
| **Use case** | Human access | Manage permissions for multiple users | EC2, Lambda, cross-account |

**Best Practice**:
- EC2 accessing S3 → **IAM Role** (not access keys)
- Multiple users with same permissions → **IAM Group**
- External user → **IAM Role** (assume role)

### Secrets Manager vs Parameter Store

| Feature | Secrets Manager | Parameter Store |
|---------|----------------|-----------------|
| **Automatic rotation** | Yes (Lambda-based) | No |
| **RDS integration** | Yes | No |
| **Cost** | $0.40/secret/month | Free (Standard), $0.05 (Advanced) |
| **Use case** | Database passwords, API keys | Application configs |

**Decision**:
- Automatic rotation → **Secrets Manager**
- Database credentials → **Secrets Manager**
- Application configs → **Parameter Store**

---

## 6️⃣ APPLICATION INTEGRATION COMPARISON

### SQS vs SNS vs EventBridge

| Feature | SQS | SNS | EventBridge |
|---------|-----|-----|-------------|
| **Pattern** | Queue (1:1) | Pub/Sub (1:N) | Event bus |
| **Delivery** | Pull | Push | Push |
| **Subscribers** | 1 per message | Multiple | Multiple targets |
| **Ordering** | FIFO option | FIFO option | No |
| **Use case** | Decouple, buffer | Notifications, fan-out | Event-driven, SaaS |

**Decision Tree**:
```
Need messaging?
├─ Queue (1:1)? → SQS
├─ Pub/Sub (1:N)? → SNS
├─ Event-driven? → EventBridge
└─ Fan-out? → SNS → Multiple SQS
```

### SQS Standard vs SQS FIFO

| Feature | Standard | FIFO |
|---------|----------|------|
| **Throughput** | Unlimited | 300 TPS (3,000 with batching) |
| **Ordering** | Best-effort | Strict FIFO |
| **Duplicates** | Possible | No (exactly-once) |
| **Name** | Any | Must end with `.fifo` |
| **Use case** | High throughput | Order matters, no duplicates |

**Keywords**:
- "Order matters" + "no duplicates" → FIFO
- "High throughput" → Standard

---

## 7️⃣ MIGRATION & DATA TRANSFER COMPARISON

### DataSync vs Snow Family vs DMS

| Feature | DataSync | Snow Family | DMS |
|---------|----------|-------------|-----|
| **Purpose** | File transfer | Physical data migration | Database migration |
| **Method** | Network | Physical device | Network |
| **Use case** | Continuous sync | Large data, limited bandwidth | Database migration |

**Decision**:
- On-prem files to S3/EFS → **DataSync**
- Large data, limited bandwidth → **Snow Family**
- Database migration → **DMS**

### Snowcone vs Snowball vs Snowmobile

| Device | Storage | Use Case |
|--------|---------|----------|
| **Snowcone** | 8-14 TB | Small, portable |
| **Snowball Edge** | 42-80 TB | Large migrations, edge computing |
| **Snowmobile** | 100 PB | Data center migration |

---

## 8️⃣ ANALYTICS SERVICES COMPARISON

### Kinesis Data Streams vs Kinesis Firehose

| Feature | Data Streams | Firehose |
|---------|-------------|----------|
| **Latency** | Real-time (200ms) | Near real-time (60s) |
| **Scaling** | Manual (shards) | Automatic |
| **Retention** | 1-365 days | No retention |
| **Destinations** | Custom consumers | S3, Redshift, OpenSearch, Splunk |
| **Use case** | Real-time processing | Load to destinations |

**Decision**:
- Real-time processing → **Data Streams**
- Load to S3/Redshift → **Firehose**

### Athena vs Redshift vs EMR

| Feature | Athena | Redshift | EMR |
|---------|--------|----------|-----|
| **Type** | Serverless SQL | Data warehouse | Big data cluster |
| **Data location** | S3 | Redshift storage | HDFS, S3 |
| **Use case** | Ad-hoc queries | OLAP, BI | Hadoop, Spark |
| **Cost** | Pay per query | Pay for cluster | Pay for EC2 + EMR |

**Decision**:
- Ad-hoc queries on S3 → **Athena**
- Data warehouse, BI → **Redshift**
- Hadoop/Spark → **EMR**

---

## 9️⃣ KEYWORDS TO SERVICE MAPPING

### Cost Keywords
- "Most cost-effective" + "predictable" → **Reserved Instances**
- "Cost-effective" + "fault-tolerant" → **Spot Instances**
- "Lowest cost" + "archive" → **Glacier Deep Archive**
- "Pay only for what you use" → **Lambda**, **Fargate**

### Performance Keywords
- "Lowest latency" → **CloudFront**, **ElastiCache**, **Global Accelerator**
- "High IOPS" → **io2 EBS**, **Instance Store**
- "High throughput" → **st1 EBS**, **FSx Lustre**
- "Real-time" → **Kinesis Data Streams**, **DynamoDB Streams**

### Availability Keywords
- "Highly available" → **Multi-AZ**, **ELB**, **Auto Scaling**
- "Automatic failover" → **Multi-AZ RDS**, **Aurora**
- "Disaster recovery" → **Multi-Region**, **Backups**, **Pilot Light**

### Security Keywords
- "Encrypt at rest" → **KMS**, **S3 encryption**
- "Encrypt in transit" → **SSL/TLS**, **VPN**
- "Least privilege" → **IAM policies**
- "Audit" → **CloudTrail**

### Operational Keywords
- "Minimum operational overhead" → **Managed services**, **Serverless**
- "Serverless" → **Lambda**, **Fargate**, **DynamoDB**, **Aurora Serverless**
- "Automated" → **CloudFormation**, **Auto Scaling**, **EventBridge**

---

## 🔟 COMMON EXAM SCENARIOS

### Scenario: High Availability Web Application
**Requirements**: Multi-AZ, auto-scaling, load balancing

**Solution**:
```
Users → Route 53
  → CloudFront (optional, for global)
    → ALB (multi-AZ)
      → Auto Scaling Group (multi-AZ)
        → EC2 instances
          → RDS Multi-AZ
```

### Scenario: Serverless Application
**Requirements**: No servers, event-driven, scalable

**Solution**:
```
Users → API Gateway
  → Lambda
    → DynamoDB
S3 events → Lambda → Process
```

### Scenario: Real-Time Data Processing
**Requirements**: Ingest streaming data, process, store

**Solution**:
```
Data sources → Kinesis Data Streams
  → Lambda / Kinesis Data Analytics
    → Kinesis Firehose
      → S3 / Redshift
```

### Scenario: Hybrid Cloud Storage
**Requirements**: On-premises access to S3

**Solution**:
```
On-premises → Storage Gateway (File Gateway)
  → S3
```

### Scenario: Multi-Account Management
**Requirements**: Centralized billing, governance

**Solution**:
```
AWS Organizations
├─ Consolidated billing
├─ SCPs (Service Control Policies)
└─ Control Tower (best practices)
```

---

## 📋 FINAL CHECKLIST

### Must Know Comparisons
- [ ] EC2 vs Lambda vs Fargate
- [ ] S3 storage classes (7 types)
- [ ] EBS volume types (gp3, io2, st1, sc1)
- [ ] RDS vs DynamoDB vs Aurora
- [ ] Read Replicas vs Multi-AZ
- [ ] Redis vs Memcached
- [ ] Security Groups vs NACLs
- [ ] ALB vs NLB vs GWLB
- [ ] CloudFront vs Global Accelerator
- [ ] VPN vs Direct Connect
- [ ] CloudTrail vs CloudWatch vs Config
- [ ] SQS vs SNS vs EventBridge
- [ ] SQS Standard vs FIFO
- [ ] Kinesis Data Streams vs Firehose
- [ ] Secrets Manager vs Parameter Store

### Decision-Making Framework
1. **Identify keywords** in question (cost-effective, serverless, high availability)
2. **Understand requirements** (performance, cost, security, operational overhead)
3. **Eliminate wrong answers** (rule out obviously incorrect)
4. **Apply Well-Architected principles** (6 pillars)
5. **Choose simplest solution** that meets requirements

---

## 🎯 EXAM DAY TIPS

### Time Management
- **2 minutes per question** (65 questions, 130 minutes)
- **Flag difficult questions**, return later
- **Reserve 15-20 minutes** for review

### Reading Questions
- **Read carefully**: Look for keywords
- **Identify requirements**: Cost, performance, security, operational overhead
- **Watch for qualifiers**: "MOST cost-effective", "LEAST operational overhead"

### Common Traps
- ❌ Overthinking: Choose simplest solution
- ❌ Ignoring keywords: "serverless", "minimum effort"
- ❌ Missing requirements: Read entire question
- ❌ Confusing similar services: Use this comparison sheet!

---

## 🚀 YOU'RE READY!

Nếu bạn đã nắm vững:
1. ✅ 4 CRITICAL files (Compute, Storage, Database, Networking)
2. ✅ 3 HIGH files (Security, Management, Application Integration)
3. ✅ File này (Services Comparison)

**Bạn đã sẵn sàng đạt chứng chỉ AWS SAA-C03! 🎉**

---

**Chúc bạn thi tốt và đạt điểm cao! Good luck! 💪**

*Cập nhật: 2026*
