# 🔑 M. KEYWORDS MAPPING

> **Nhận Diện Keywords Trong Đề Thi → Chọn Đúng Dịch Vụ AWS**

---

## 🎯 Cost Optimization Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"most cost-effective"** | Reserved Instances, Spot, S3 Intelligent-Tiering | Commitment hoặc flexible pricing |
| **"minimize cost"** | S3 Glacier, Spot Instances, Lambda | Cheapest options |
| **"unpredictable workload"** | Auto Scaling, Lambda, On-Demand | Pay for what you use |
| **"steady-state workload"** | Reserved Instances, Savings Plans | Predictable = commit for discount |
| **"fault-tolerant workload"** | Spot Instances | Can tolerate interruptions |
| **"infrequent access"** | S3 Standard-IA, S3 One Zone-IA | Lower storage cost |
| **"archive"** | S3 Glacier, Glacier Deep Archive | Long-term, rare access |
| **"pay per request"** | Lambda, DynamoDB On-Demand, API Gateway | No idle cost |

---

## ⚡ Performance Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"lowest latency"** | CloudFront, ElastiCache, DynamoDB DAX | Caching, CDN |
| **"microsecond latency"** | DynamoDB DAX | Sub-millisecond |
| **"millisecond latency"** | DynamoDB, ElastiCache | In-memory |
| **"high IOPS"** | EBS io2, io2 Block Express | Provisioned IOPS SSD |
| **"high throughput"** | EBS st1, S3 Transfer Acceleration | Optimized for throughput |
| **"real-time"** | Kinesis Data Streams, DynamoDB Streams | Stream processing |
| **"batch processing"** | AWS Batch, EMR, Spot Instances | Large-scale compute |
| **"millions of requests"** | NLB, DynamoDB, Lambda | High scalability |

---

## 🔐 Security Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"encryption at rest"** | KMS, S3 SSE, EBS encryption | Data stored encrypted |
| **"encryption in transit"** | TLS/SSL, ACM, VPN | Data transmitted encrypted |
| **"audit API calls"** | CloudTrail | Who did what, when |
| **"track configuration changes"** | AWS Config | Resource config history |
| **"detect threats"** | GuardDuty | ML-based threat detection |
| **"web application firewall"** | AWS WAF | Protect against exploits |
| **"DDoS protection"** | AWS Shield | Layer 3/4/7 protection |
| **"PII detection"** | Macie | Sensitive data discovery |
| **"secrets rotation"** | Secrets Manager | Automatic rotation |
| **"least privilege"** | IAM Policies, Roles | Minimal permissions |

---

## 🏗️ High Availability Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"highly available"** | Multi-AZ, Auto Scaling, ELB | No single point of failure |
| **"automatic failover"** | RDS Multi-AZ, Aurora, Route 53 | Auto-recovery |
| **"fault tolerant"** | Multi-AZ, Multi-Region | Withstand failures |
| **"disaster recovery"** | S3 CRR, Aurora Global, Backup | Cross-region replication |
| **"99.99% availability"** | Multi-AZ deployments | 4 nines SLA |
| **"zero downtime"** | Blue/Green deployment, Aurora | No service interruption |
| **"self-healing"** | Auto Scaling, Aurora storage | Automatic recovery |

---

## 🚀 Serverless Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"serverless"** | Lambda, Fargate, Aurora Serverless | No server management |
| **"no infrastructure management"** | Lambda, DynamoDB, S3 | Fully managed |
| **"auto-scaling"** | Lambda, DynamoDB, Aurora Serverless | Automatic capacity |
| **"event-driven"** | Lambda, EventBridge, Step Functions | Triggered by events |
| **"short-lived functions"** | Lambda | < 15 minutes |
| **"pay per invocation"** | Lambda | Per-request billing |

---

## 🌍 Global Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"global users"** | CloudFront, Global Accelerator | Edge locations |
| **"multi-region"** | S3 CRR, Aurora Global, DynamoDB Global Tables | Cross-region |
| **"lowest latency globally"** | CloudFront, Route 53 Latency Routing | Nearest edge |
| **"active-active"** | DynamoDB Global Tables, Aurora Global | Multi-region writes |
| **"disaster recovery"** | Cross-region replication, Pilot Light | Secondary region |

---

## 🔄 Integration Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"decouple"** | SQS, SNS, EventBridge | Loose coupling |
| **"asynchronous"** | SQS, SNS, Lambda async | Non-blocking |
| **"message queue"** | SQS | Buffer between services |
| **"pub/sub"** | SNS | One-to-many messaging |
| **"workflow orchestration"** | Step Functions | Coordinate services |
| **"API management"** | API Gateway | RESTful APIs |
| **"event bus"** | EventBridge | Event routing |

---

## 💾 Storage Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"object storage"** | S3 | Files, images, videos |
| **"block storage"** | EBS | EC2 volumes |
| **"file storage"** | EFS, FSx | Shared file systems |
| **"shared storage"** | EFS, FSx | Multi-instance access |
| **"lifecycle policies"** | S3 Lifecycle | Auto-tiering |
| **"versioning"** | S3 Versioning | Multiple versions |
| **"durability 11 nines"** | S3 | 99.999999999% |

---

## 🗄️ Database Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"relational database"** | RDS, Aurora | SQL databases |
| **"NoSQL"** | DynamoDB, DocumentDB | Non-relational |
| **"in-memory"** | ElastiCache | Caching layer |
| **"data warehouse"** | Redshift | Analytics, BI |
| **"graph database"** | Neptune | Relationships |
| **"time-series"** | Timestream | IoT, metrics |
| **"ledger"** | QLDB | Immutable history |
| **"read replicas"** | RDS, Aurora | Scale reads |

---

## 🔧 Operational Keywords

| Keyword | AWS Service | Explanation |
|---------|-------------|-------------|
| **"infrastructure as code"** | CloudFormation, CDK | IaC |
| **"configuration management"** | Systems Manager, Config | Manage configs |
| **"patch management"** | Systems Manager Patch Manager | OS patching |
| **"monitoring"** | CloudWatch | Metrics, logs, alarms |
| **"distributed tracing"** | X-Ray | Application insights |
| **"centralized logging"** | CloudWatch Logs, S3 | Log aggregation |

---

## 🎓 Exam Strategy

### Đọc Đề Và Tìm Keywords

**Step 1:** Identify keywords trong câu hỏi
**Step 2:** Map keywords tới AWS services
**Step 3:** Loại trừ đáp án sai
**Step 4:** Chọn đáp án phù hợp nhất

### Ví Dụ

**Câu hỏi:**
> "A company needs a **cost-effective** solution for **infrequently accessed** data with **11 nines durability**."

**Keywords:**
- "cost-effective" → S3 storage classes
- "infrequently accessed" → S3 IA
- "11 nines durability" → S3

**Đáp án:** ✅ S3 Standard-IA hoặc S3 One Zone-IA

---

## ✅ Checklist

- [ ] Học thuộc cost optimization keywords
- [ ] Nhớ performance keywords (latency, IOPS, throughput)
- [ ] Hiểu security keywords (encryption, audit, detect)
- [ ] Biết HA keywords (multi-AZ, failover, fault-tolerant)
- [ ] Nhận diện serverless keywords
- [ ] Phân biệt storage keywords (object, block, file)

*Keywords là chìa khóa để giải nhanh 80% câu hỏi trong đề thi!*
