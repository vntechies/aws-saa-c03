# 🏛️ A. NỀN TẢNG KIẾN TRÚC AWS

> **Mục tiêu:** Hiểu rõ các nguyên tắc thiết kế kiến trúc trên AWS - nền tảng cho mọi quyết định kiến trúc

---

## 📋 Mục Lục
1. [AWS Well-Architected Framework](#well-architected-framework)
2. [Shared Responsibility Model](#shared-responsibility-model)
3. [Các Nguyên Tắc Thiết Kế Cloud](#nguyên-tắc-thiết-kế)
4. [Global Infrastructure](#global-infrastructure)
5. [Câu Hỏi Thường Gặp](#câu-hỏi-thường-gặp)

---

## 🎯 AWS Well-Architected Framework

### 6 Trụ Cột (Pillars) - PHẢI NHỚ

#### 1️⃣ **Operational Excellence** (Xuất Sắc Vận Hành)
**Khái niệm:** Khả năng vận hành và giám sát hệ thống để mang lại giá trị kinh doanh

**Nguyên tắc thiết kế:**
- ✅ Thực hiện operations dưới dạng code (Infrastructure as Code)
- ✅ Tài liệu hóa mọi thứ
- ✅ Thực hiện thay đổi nhỏ, thường xuyên và có thể đảo ngược
- ✅ Cải tiến procedures liên tục
- ✅ Dự đoán failures và học từ chúng

**Dịch vụ AWS chính:**
- **CloudFormation** - Infrastructure as Code
- **AWS Config** - Track configuration changes
- **CloudWatch** - Monitoring và logging
- **CloudTrail** - Audit API calls
- **Systems Manager** - Operational insights
- **X-Ray** - Application tracing

**Ví dụ trong đề thi:**
> *"Công ty cần tự động hóa việc triển khai infrastructure và đảm bảo consistency. Giải pháp nào tốt nhất?"*
> 
> ✅ **Đáp án:** AWS CloudFormation - IaC, version control, repeatable deployments

---

#### 2️⃣ **Security** (Bảo Mật) - CHIẾM 30% ĐỀ THI

**Khái niệm:** Bảo vệ thông tin, hệ thống và tài sản

**Nguyên tắc thiết kế:**
- ✅ Implement strong identity foundation (IAM)
- ✅ Enable traceability (CloudTrail)
- ✅ Apply security at all layers
- ✅ Automate security best practices
- ✅ Protect data in transit and at rest
- ✅ Keep people away from data (least privilege)
- ✅ Prepare for security events

**Dịch vụ AWS chính:**
- **IAM** - Identity and Access Management
- **KMS** - Key Management Service (encryption)
- **Secrets Manager** - Rotate secrets automatically
- **GuardDuty** - Threat detection
- **Security Groups & NACLs** - Network security
- **WAF & Shield** - Web application protection
- **Macie** - Data privacy (PII detection)

**Ví dụ trong đề thi:**
> *"Application cần truy cập S3 bucket. Cách nào secure nhất?"*
> 
> ❌ Sai: Hard-code access keys trong code
> ✅ Đúng: Sử dụng IAM Role gán cho EC2 instance

---

#### 3️⃣ **Reliability** (Độ Tin Cậy) - CHIẾM 26% ĐỀ THI

**Khái niệm:** Khả năng hệ thống phục hồi từ failures và đáp ứng nhu cầu

**Nguyên tắc thiết kế:**
- ✅ Test recovery procedures (chaos engineering)
- ✅ Automatically recover from failure
- ✅ Scale horizontally (thêm instances thay vì tăng size)
- ✅ Stop guessing capacity (Auto Scaling)
- ✅ Manage change through automation

**Dịch vụ AWS chính:**
- **Auto Scaling** - Tự động scale
- **ELB** - Distribute traffic
- **Route 53** - DNS với health checks
- **RDS Multi-AZ** - Database failover
- **S3** - 99.999999999% durability
- **CloudWatch Alarms** - Monitor và alert

**Ví dụ trong đề thi:**
> *"Database cần high availability với automatic failover trong vòng 60 giây. Giải pháp nào phù hợp?"*
> 
> ✅ **Đáp án:** RDS Multi-AZ - automatic failover trong 1-2 phút

---

#### 4️⃣ **Performance Efficiency** (Hiệu Năng) - CHIẾM 24% ĐỀ THI

**Khái niệm:** Sử dụng computing resources hiệu quả để đáp ứng requirements

**Nguyên tắc thiết kế:**
- ✅ Democratize advanced technologies (dùng managed services)
- ✅ Go global in minutes (CloudFront, Global Accelerator)
- ✅ Use serverless architectures
- ✅ Experiment more often
- ✅ Consider mechanical sympathy (chọn đúng công nghệ cho use case)

**Dịch vụ AWS chính:**
- **Lambda** - Serverless compute
- **EBS** - Optimized storage (gp3, io2)
- **ElastiCache** - In-memory caching
- **CloudFront** - CDN
- **RDS Read Replicas** - Scale reads
- **DynamoDB DAX** - Microsecond latency

**Ví dụ trong đề thi:**
> *"Website có traffic từ toàn cầu, cần giảm latency. Giải pháp nào cost-effective nhất?"*
> 
> ✅ **Đáp án:** CloudFront (CDN) - cache content gần users

---

#### 5️⃣ **Cost Optimization** (Tối Ưu Chi Phí) - CHIẾM 20% ĐỀ THI

**Khái niệm:** Tránh chi phí không cần thiết

**Nguyên tắc thiết kế:**
- ✅ Implement Cloud Financial Management
- ✅ Adopt a consumption model (pay for what you use)
- ✅ Measure overall efficiency
- ✅ Stop spending money on undifferentiated heavy lifting
- ✅ Analyze and attribute expenditure

**Dịch vụ AWS chính:**
- **Cost Explorer** - Visualize costs
- **Budgets** - Set cost alerts
- **Trusted Advisor** - Cost optimization recommendations
- **S3 Intelligent-Tiering** - Automatic cost optimization
- **Reserved Instances / Savings Plans** - Commit for discounts
- **Spot Instances** - Up to 90% discount

**Ví dụ trong đề thi:**
> *"EC2 instances chạy 24/7 trong 3 năm. Cách nào tiết kiệm nhất?"*
> 
> ✅ **Đáp án:** Reserved Instances (3-year) - tiết kiệm up to 72%

---

#### 6️⃣ **Sustainability** (Bền Vững)

**Khái niệm:** Giảm thiểu tác động môi trường

**Nguyên tắc thiết kế:**
- ✅ Understand your impact
- ✅ Establish sustainability goals
- ✅ Maximize utilization
- ✅ Anticipate and adopt new efficient hardware
- ✅ Use managed services
- ✅ Reduce downstream impact

**Dịch vụ AWS chính:**
- **Graviton processors** - Energy efficient
- **S3 Glacier** - Cold storage với ít energy
- **Auto Scaling** - Right-size resources
- **Lambda** - Chỉ chạy khi cần

**Ví dụ trong đề thi:**
> *Ít khi ra trong đề thi SAA-C03, nhưng biết concept là đủ*

---

## 🔐 Shared Responsibility Model

### Phân Chia Trách Nhiệm

```
┌─────────────────────────────────────────┐
│       CUSTOMER (Khách Hàng)             │
├─────────────────────────────────────────┤
│ ✅ Data                                 │
│ ✅ Platform, Applications               │
│ ✅ Identity & Access Management (IAM)   │
│ ✅ Operating System, Network config     │
│ ✅ Client-side encryption               │
│ ✅ Server-side encryption               │
│ ✅ Network traffic protection           │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│         AWS (Nhà Cung Cấp)              │
├─────────────────────────────────────────┤
│ ✅ Hardware / Global Infrastructure     │
│ ✅ Compute, Storage, Database, Network  │
│ ✅ Regions, AZs, Edge Locations         │
│ ✅ Physical security of data centers    │
│ ✅ Network infrastructure               │
│ ✅ Virtualization layer                 │
└─────────────────────────────────────────┘
```

### Trách Nhiệm Theo Loại Dịch Vụ

#### 📦 **IaaS (Infrastructure as a Service) - VD: EC2**
**Customer chịu trách nhiệm:**
- Operating System patching
- Application installation
- Security Groups configuration
- Data encryption
- Network configuration

**AWS chịu trách nhiệm:**
- Physical infrastructure
- Hypervisor
- Network infrastructure

#### 🔧 **PaaS (Platform as a Service) - VD: RDS**
**Customer chịu trách nhiệm:**
- Database user management
- IAM policies
- Security Groups
- Data encryption (optional)

**AWS chịu trách nhiệm:**
- Operating System patching
- Database engine updates
- Hardware maintenance
- Backups (automated)

#### ☁️ **SaaS (Software as a Service) - VD: S3**
**Customer chịu trách nhiệm:**
- Data classification
- IAM policies
- Bucket policies
- Encryption settings

**AWS chịu trách nhiệm:**
- Infrastructure
- Platform
- Application
- Physical security

### Câu Hỏi Thi Thường Gặp

> **Q: Ai chịu trách nhiệm patch EC2 operating system?**
> 
> ✅ Customer (bạn phải tự patch hoặc dùng Systems Manager)

> **Q: Ai chịu trách nhiệm patch RDS database engine?**
> 
> ✅ AWS (automated patching trong maintenance window)

> **Q: Ai chịu trách nhiệm encrypt data trong S3?**
> 
> ✅ Customer quyết định enable encryption (SSE-S3, SSE-KMS, SSE-C)

---

## 🏗️ Nguyên Tắc Thiết Kế Cloud

### 1. Design for Failure
**Nguyên tắc:** Assume everything fails, design accordingly

**Áp dụng:**
- ✅ Multi-AZ deployments
- ✅ Auto Scaling groups
- ✅ Health checks và automatic recovery
- ✅ Database replication
- ✅ Backup và disaster recovery plans

**Ví dụ:**
```
❌ SAI: Single EC2 instance chạy web app
✅ ĐÚNG: Auto Scaling Group với minimum 2 instances ở 2 AZs khác nhau + ELB
```

### 2. Decouple Components
**Nguyên tắc:** Tách các components để chúng fail independently

**Áp dụng:**
- ✅ Sử dụng SQS giữa các services
- ✅ Event-driven architecture (EventBridge)
- ✅ Microservices thay vì monolith
- ✅ API Gateway cho frontend/backend

**Ví dụ:**
```
❌ SAI: Web tier gọi trực tiếp database
✅ ĐÚNG: Web tier → SQS → Worker tier → Database
```

### 3. Implement Elasticity
**Nguyên tắc:** Scale up/down dựa trên demand

**Áp dụng:**
- ✅ Auto Scaling Groups
- ✅ DynamoDB On-Demand mode
- ✅ Lambda (automatic scaling)
- ✅ Aurora Serverless

**Ví dụ:**
```
❌ SAI: Over-provision EC2 instances cho peak load
✅ ĐÚNG: Auto Scaling với target tracking dựa trên CPU/memory
```

### 4. Think Parallel
**Nguyên tắc:** Scale horizontally, not vertically

**Áp dụng:**
- ✅ Thêm nhiều instances thay vì tăng size
- ✅ Read Replicas thay vì upgrade master database
- ✅ Sharding cho databases lớn
- ✅ CloudFront edge locations

**Ví dụ:**
```
❌ SAI: Upgrade từ t3.medium → t3.2xlarge
✅ ĐÚNG: Thêm instances vào Auto Scaling Group
```

### 5. Leverage Different Storage Options
**Nguyên tắc:** Chọn đúng storage cho đúng use case

**Áp dụng:**
- ✅ S3 cho object storage
- ✅ EBS cho block storage (EC2)
- ✅ EFS cho shared file storage
- ✅ Database cho structured data
- ✅ ElastiCache cho caching

---

## 🌍 Global Infrastructure

### Cấu Trúc Vật Lý

```
AWS Global Infrastructure
│
├── Regions (33+ regions)
│   ├── Availability Zones (105+ AZs)
│   │   └── Data Centers (multiple per AZ)
│   │
│   └── Local Zones (32+ locations)
│
└── Edge Locations (600+ locations)
    ├── CloudFront POPs
    └── Regional Edge Caches
```

### 🗺️ Regions

**Định nghĩa:** Khu vực địa lý với nhiều Availability Zones

**Đặc điểm:**
- Mỗi Region hoàn toàn độc lập
- Data không tự động replicate giữa các Regions
- Chọn Region dựa trên: latency, cost, compliance, service availability

**Ví dụ Regions:**
- `us-east-1` (N. Virginia) - Region có nhiều services nhất
- `ap-southeast-1` (Singapore)
- `eu-west-1` (Ireland)

**Câu hỏi thi:**
> *"Company ở Việt Nam cần deploy application với lowest latency. Chọn Region nào?"*
> 
> ✅ `ap-southeast-1` (Singapore) - gần nhất

### 🏢 Availability Zones (AZs)

**Định nghĩa:** Một hoặc nhiều data centers trong một Region

**Đặc điểm:**
- Mỗi Region có ít nhất 3 AZs (thường 3-6 AZs)
- Cách nhau đủ xa để tránh disaster ảnh hưởng cùng lúc
- Kết nối với nhau qua low-latency links
- Tên: `us-east-1a`, `us-east-1b`, `us-east-1c`

**Best Practice:**
✅ Luôn deploy ít nhất 2 AZs cho high availability

**Ví dụ:**
```
Region: us-east-1
├── AZ: us-east-1a (EC2 instances, RDS primary)
├── AZ: us-east-1b (EC2 instances, RDS standby)
└── AZ: us-east-1c (EC2 instances)
```

### 🌐 Edge Locations

**Định nghĩa:** Data centers dùng cho CloudFront CDN

**Đặc điểm:**
- 600+ locations trên toàn cầu
- Cache content gần users
- Không phải tất cả services đều có ở Edge Locations

**Services sử dụng Edge Locations:**
- ✅ CloudFront (CDN)
- ✅ Route 53 (DNS)
- ✅ AWS WAF
- ✅ AWS Shield
- ✅ Lambda@Edge

---

## 🎯 Các Khái Niệm Quan Trọng Khác

### 1. High Availability vs Fault Tolerance

| Tiêu Chí | High Availability | Fault Tolerance |
|----------|-------------------|-----------------|
| **Downtime** | Có downtime ngắn | Zero downtime |
| **Cost** | Thấp hơn | Cao hơn |
| **Ví dụ** | RDS Multi-AZ (1-2 min failover) | Aurora with replicas (instant) |
| **Khi nào dùng** | Most applications | Mission-critical apps |

### 2. RPO vs RTO

**RPO (Recovery Point Objective):** Mất bao nhiêu data có thể chấp nhận được
- VD: RPO = 1 hour → backup mỗi giờ

**RTO (Recovery Time Objective):** Downtime tối đa có thể chấp nhận
- VD: RTO = 4 hours → phải restore trong 4 giờ

**Ví dụ trong đề thi:**
> *"Application cần RPO = 5 minutes, RTO = 1 hour. Disaster recovery strategy nào phù hợp?"*
> 
> ✅ Pilot Light hoặc Warm Standby (continuous replication)

### 3. Scalability Types

**Vertical Scaling (Scale Up/Down):**
- Tăng/giảm size của resource
- VD: t3.micro → t3.large
- ❌ Có downtime
- ❌ Có giới hạn (max instance size)

**Horizontal Scaling (Scale Out/In):**
- Thêm/bớt số lượng resources
- VD: 2 instances → 10 instances
- ✅ No downtime
- ✅ Unlimited scaling (trong giới hạn AWS)

---

## ❓ Câu Hỏi Thường Gặp

### Q1: Well-Architected Framework có bắt buộc không?
**A:** Không bắt buộc, nhưng đây là best practices. Đề thi thường hỏi giải pháp align với các pillars này.

### Q2: Pillar nào quan trọng nhất?
**A:** Trong đề thi, **Security** (30%) và **Reliability** (26%) chiếm nhiều nhất. Nhưng tất cả đều quan trọng.

### Q3: Shared Responsibility Model có thay đổi theo dịch vụ không?
**A:** Có. IaaS (EC2) customer chịu nhiều trách nhiệm hơn SaaS (S3).

### Q4: Phải deploy bao nhiêu AZs?
**A:** Minimum 2 AZs cho high availability. 3 AZs cho fault tolerance cao hơn.

### Q5: Region nào rẻ nhất?
**A:** Thường là `us-east-1` (N. Virginia). Nhưng chọn Region dựa trên requirements, không chỉ cost.

---

## ✅ Checklist Kiến Thức

- [ ] Nhớ 6 pillars của Well-Architected Framework
- [ ] Hiểu Shared Responsibility Model
- [ ] Phân biệt Region, AZ, Edge Location
- [ ] Biết khi nào Multi-AZ, khi nào Multi-Region
- [ ] Hiểu RPO vs RTO
- [ ] Phân biệt High Availability vs Fault Tolerance
- [ ] Biết scalability types (vertical vs horizontal)
- [ ] Nhớ "Design for Failure" principle

---

## 🔗 Liên Kết Với Các File Khác

- **Tiếp theo:** [B. Bảo Mật & Compliance](B-bao-mat-compliance.md) - Deep dive vào Security pillar
- **Áp dụng:** [H. High Availability](H-high-availability.md) - Reliability pillar
- **Áp dụng:** [G. Tối Ưu Chi Phí](G-toi-uu-chi-phi.md) - Cost Optimization pillar

---

*File này là nền tảng cho tất cả các quyết định kiến trúc. Đọc kỹ trước khi học các file khác.*
