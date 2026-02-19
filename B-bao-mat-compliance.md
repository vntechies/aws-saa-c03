# 🔐 B. BẢO MẬT & COMPLIANCE

> **Domain 1: Secure Architectures - 30% đề thi (~20 câu)**
> 
> **Mục tiêu:** Trả lời đúng 16-18/20 câu về security

---

## 📋 Mục Lục
1. [IAM - Identity & Access Management](#iam)
2. [Encryption & Key Management](#encryption)
3. [Network Security](#network-security)
4. [Monitoring & Compliance](#monitoring)
5. [Threat Detection & Protection](#threat-detection)
6. [Data Privacy & Protection](#data-privacy)
7. [Kịch Bản Thi Thực Tế](#kịch-bản-thi)

---

## 🔑 IAM - Identity & Access Management

### Các Thành Phần Cơ Bản

#### 1. **Users** (Người Dùng)
**Khi nào dùng:** Cho người thật (employees, contractors)

**Best Practices:**
- ✅ Enable MFA (Multi-Factor Authentication)
- ✅ Không share credentials
- ✅ Rotate access keys thường xuyên (90 ngày)
- ✅ Principle of Least Privilege

**Ví dụ sai lầm:**
```
❌ Root user dùng hàng ngày
❌ Hard-code access keys trong code
❌ Một user có AdministratorAccess cho tất cả
```

#### 2. **Groups** (Nhóm)
**Khi nào dùng:** Gom users có permissions giống nhau

**Best Practices:**
- ✅ Tạo groups theo job function (Developers, DBAdmins, etc.)
- ✅ Attach policies vào groups, không phải users
- ✅ Một user có thể thuộc nhiều groups

**Ví dụ:**
```
Group: Developers
├── User: Alice
├── User: Bob
└── Policy: DeveloperAccess
    ├── S3 Read/Write
    ├── EC2 Launch
    └── CloudWatch Logs
```

#### 3. **Roles** (Vai Trò) ⭐ QUAN TRỌNG NHẤT

**Khi nào dùng:**
- ✅ EC2 instances cần access AWS services
- ✅ Lambda functions
- ✅ Cross-account access
- ✅ Federation (SSO)

**Ưu điểm:**
- ✅ Không cần access keys
- ✅ Temporary credentials (tự động rotate)
- ✅ Secure hơn users

**Ví dụ trong đề thi:**
> **Q:** EC2 instance cần đọc S3 bucket. Cách nào secure nhất?
> 
> ❌ Store access keys trong EC2
> ❌ Hardcode credentials trong application
> ✅ **Attach IAM Role cho EC2 instance**

**Các loại Roles quan trọng:**

| Role Type | Use Case | Ví Dụ |
|-----------|----------|-------|
| **Service Role** | AWS service assume role | EC2 → S3, Lambda → DynamoDB |
| **Cross-Account Role** | Account A access Account B | Dev account → Prod account |
| **Federation Role** | External users (SAML, OIDC) | Corporate users → AWS Console |
| **Web Identity Role** | Mobile apps | Facebook/Google login → AWS |

#### 4. **Policies** (Chính Sách)

**Loại Policies:**

**a) AWS Managed Policies**
- Tạo sẵn bởi AWS
- VD: `AdministratorAccess`, `ReadOnlyAccess`, `PowerUserAccess`
- ✅ Dùng khi: Standard use cases
- ❌ Không customize được

**b) Customer Managed Policies**
- Bạn tự tạo
- ✅ Dùng khi: Cần custom permissions
- ✅ Có thể reuse cho nhiều users/roles

**c) Inline Policies**
- Attach trực tiếp vào user/role/group
- ❌ Không reuse được
- ✅ Dùng khi: One-to-one relationship

**Cấu trúc Policy (JSON):**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "203.0.113.0/24"
        }
      }
    }
  ]
}
```

**Policy Evaluation Logic:**
```
1. Explicit DENY → Always wins
2. Explicit ALLOW → Granted if no deny
3. Implicit DENY → Default (no allow = deny)
```

**Ví dụ trong đề thi:**
> **Q:** User có policy Allow S3:*, nhưng có policy khác Deny S3:DeleteBucket. User có thể delete bucket không?
> 
> ✅ **Không** - Explicit Deny always wins

### IAM Best Practices - PHẢI NHỚ

| # | Best Practice | Giải Thích |
|---|---------------|------------|
| 1 | **Không dùng Root Account** | Chỉ dùng để setup ban đầu, sau đó lock lại |
| 2 | **Enable MFA** | Đặc biệt cho Root và privileged users |
| 3 | **Least Privilege** | Chỉ cho permissions tối thiểu cần thiết |
| 4 | **Use Roles, not Users** | Cho EC2, Lambda, applications |
| 5 | **Rotate Credentials** | Access keys 90 ngày, passwords theo policy |
| 6 | **Use Policy Conditions** | Restrict by IP, time, MFA, etc. |
| 7 | **Enable CloudTrail** | Audit tất cả API calls |
| 8 | **Use Groups** | Không assign policies trực tiếp cho users |

### IAM Advanced Concepts

#### **Permission Boundaries**
**Mục đích:** Giới hạn maximum permissions mà IAM entity có thể có

**Use Case:**
- Developers có thể tạo roles nhưng không được vượt quá boundaries
- Prevent privilege escalation

**Ví dụ:**
```
User có policy: AdministratorAccess
Permission Boundary: S3 và EC2 only
→ User chỉ có admin quyền trên S3 và EC2, không có quyền trên các services khác
```

#### **Service Control Policies (SCPs)**
**Mục đích:** Restrict permissions trong AWS Organizations

**Đặc điểm:**
- Apply cho entire accounts hoặc OUs
- Không grant permissions, chỉ restrict
- Root account không bị ảnh hưởng bởi SCPs

**Ví dụ:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "ec2:Region": ["us-east-1", "us-west-2"]
        }
      }
    }
  ]
}
```
→ Ngăn launch EC2 ngoài us-east-1 và us-west-2

#### **Resource-Based Policies**
**Services hỗ trợ:**
- S3 (bucket policies)
- SQS (queue policies)
- SNS (topic policies)
- Lambda (function policies)
- KMS (key policies)

**Khác với Identity-Based Policies:**
- Attach vào resource thay vì IAM identity
- Có thể grant cross-account access
- Có Principal element

**Ví dụ S3 Bucket Policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:role/MyRole"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

---

## 🔐 Encryption & Key Management

### Encryption Types

#### **1. Encryption at Rest** (Dữ liệu đang lưu trữ)

**Services hỗ trợ:**
- ✅ S3 - SSE-S3, SSE-KMS, SSE-C
- ✅ EBS - KMS encryption
- ✅ RDS - TDE (Transparent Data Encryption)
- ✅ DynamoDB - KMS encryption
- ✅ EFS - KMS encryption

**Ví dụ trong đề thi:**
> **Q:** S3 bucket chứa sensitive data. Cần encryption với full control over keys. Giải pháp nào?
> 
> ❌ SSE-S3 (AWS managed keys)
> ✅ **SSE-KMS** (Customer managed keys) hoặc SSE-C (Customer provided keys)

#### **2. Encryption in Transit** (Dữ liệu đang truyền)

**Phương pháp:**
- ✅ HTTPS/TLS
- ✅ SSL certificates (ACM)
- ✅ VPN (IPSec)
- ✅ AWS Certificate Manager (ACM)

**Ví dụ:**
```
Client → HTTPS → CloudFront → HTTPS → ALB → HTTPS → EC2
```

### AWS Key Management Service (KMS) ⭐

#### **Key Types**

| Key Type | Quản Lý | Rotation | Use Case |
|----------|---------|----------|----------|
| **AWS Managed** | AWS | Automatic (1 year) | Default encryption |
| **Customer Managed** | Bạn | Optional (manual/auto) | Custom control |
| **AWS Owned** | AWS | AWS controls | Internal AWS services |
| **CloudHSM** | Bạn | Manual | Regulatory compliance |

#### **KMS Key Policies**

**Default Key Policy:**
- Root user có full access
- Không ai khác có access

**Custom Key Policy:**
- Specify users/roles có quyền sử dụng key
- Specify key administrators

**Ví dụ:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Enable IAM policies",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:role/MyRole"
      },
      "Action": [
        "kms:Decrypt",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    }
  ]
}
```

#### **Envelope Encryption**

**Concept:** Encrypt data key bằng master key

```
1. KMS tạo Data Key (plaintext + encrypted)
2. Dùng plaintext Data Key để encrypt data
3. Xóa plaintext Data Key
4. Lưu encrypted Data Key cùng với encrypted data
5. Khi decrypt: KMS decrypt Data Key → dùng để decrypt data
```

**Ưu điểm:**
- ✅ Không gửi large data lên KMS
- ✅ Master key không bao giờ rời khỏi KMS
- ✅ Performance tốt hơn

**Giới hạn KMS:**
- Max 4 KB data có thể encrypt trực tiếp
- Request quota: 5,500-30,000 requests/second (tùy region)

#### **S3 Encryption Options**

| Method | Key Management | Performance | Cost |
|--------|----------------|-------------|------|
| **SSE-S3** | AWS manages | Fastest | Free |
| **SSE-KMS** | Customer manages | Medium | $0.03/10k requests |
| **SSE-C** | Customer provides | Medium | Free (but complex) |
| **Client-Side** | Customer encrypts | Depends | Free |

**Khi nào dùng cái nào:**

```
SSE-S3: Default choice, AWS handles everything
SSE-KMS: Need audit trail (CloudTrail), custom key policies
SSE-C: Regulatory requirement to manage keys outside AWS
Client-Side: Maximum control, encrypt before upload
```

**Ví dụ trong đề thi:**
> **Q:** Company cần encrypt S3 objects và audit key usage. Giải pháp nào?
> 
> ✅ **SSE-KMS** - CloudTrail logs key usage

### AWS Secrets Manager vs Systems Manager Parameter Store

| Feature | Secrets Manager | Parameter Store |
|---------|-----------------|-----------------|
| **Purpose** | Secrets (passwords, API keys) | Configuration data |
| **Rotation** | Automatic (Lambda) | Manual |
| **Encryption** | Always encrypted (KMS) | Optional |
| **Cost** | $0.40/secret/month | Free (Standard), $0.05/param (Advanced) |
| **Use Case** | Database credentials | Application config |

**Ví dụ trong đề thi:**
> **Q:** RDS database password cần rotate automatically mỗi 30 ngày. Giải pháp nào?
> 
> ✅ **Secrets Manager** với automatic rotation

---

## 🛡️ Network Security

### Security Groups vs Network ACLs

| Feature | Security Groups | Network ACLs |
|---------|-----------------|--------------|
| **Level** | Instance level | Subnet level |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow + Deny |
| **Evaluation** | All rules | Rules in order |
| **Default** | Deny all inbound | Allow all |

#### **Security Groups (Stateful)**

**Đặc điểm:**
- ✅ Return traffic tự động allowed
- ✅ Chỉ có ALLOW rules
- ✅ All rules evaluated
- ✅ Có thể reference SG khác

**Ví dụ:**
```
Inbound Rule:
- Type: HTTP
- Protocol: TCP
- Port: 80
- Source: 0.0.0.0/0

→ Response traffic tự động allowed (stateful)
```

**Best Practices:**
- ✅ Least privilege (chỉ mở ports cần thiết)
- ✅ Use descriptive names
- ✅ Reference other SGs thay vì IP ranges
- ✅ Separate SGs cho từng tier (web, app, db)

#### **Network ACLs (Stateless)**

**Đặc điểm:**
- ✅ Phải explicitly allow return traffic
- ✅ Có ALLOW và DENY rules
- ✅ Rules evaluated in order (lowest number first)
- ✅ Default NACL allows all

**Ví dụ:**
```
Inbound Rules:
100 - Allow HTTP (80) from 0.0.0.0/0
200 - Allow HTTPS (443) from 0.0.0.0/0
* - Deny all

Outbound Rules:
100 - Allow ephemeral ports (1024-65535) to 0.0.0.0/0
* - Deny all
```

**Khi nào dùng NACLs:**
- ✅ Explicit DENY rules (block specific IPs)
- ✅ Additional layer of security
- ✅ Subnet-level protection

**Ví dụ trong đề thi:**
> **Q:** Cần block một IP address cụ thể. Dùng Security Group hay NACL?
> 
> ✅ **NACL** - Security Groups không có DENY rules

### VPC Security Best Practices

#### **1. Network Segmentation**

```
VPC (10.0.0.0/16)
│
├── Public Subnet (10.0.1.0/24)
│   ├── Internet Gateway
│   ├── NAT Gateway
│   └── Bastion Host
│
├── Private Subnet - App Tier (10.0.2.0/24)
│   └── EC2 instances (no public IP)
│
└── Private Subnet - DB Tier (10.0.3.0/24)
    └── RDS (isolated)
```

**Best Practices:**
- ✅ Public subnet: Chỉ load balancers, NAT Gateway
- ✅ Private subnet: Application servers, databases
- ✅ Separate subnets cho từng tier
- ✅ Use multiple AZs

#### **2. VPC Flow Logs**

**Mục đích:** Capture network traffic information

**Levels:**
- VPC level
- Subnet level
- ENI (Elastic Network Interface) level

**Destinations:**
- CloudWatch Logs
- S3
- Kinesis Data Firehose

**Use Cases:**
- ✅ Troubleshoot connectivity issues
- ✅ Security analysis
- ✅ Compliance auditing

**Ví dụ trong đề thi:**
> **Q:** Cần analyze network traffic để detect suspicious patterns. Giải pháp nào?
> 
> ✅ **VPC Flow Logs** → CloudWatch Logs → CloudWatch Insights

#### **3. AWS PrivateLink**

**Mục đích:** Private connectivity giữa VPCs và AWS services

**Ưu điểm:**
- ✅ Traffic không đi qua internet
- ✅ Không cần VPC peering, NAT, Internet Gateway
- ✅ Scalable và secure

**Use Cases:**
- ✅ Access AWS services privately (S3, DynamoDB)
- ✅ Expose services to other VPCs
- ✅ SaaS applications

**Ví dụ:**
```
VPC A → VPC Endpoint (PrivateLink) → S3
(No Internet Gateway needed)
```

---

## 📊 Monitoring & Compliance

### AWS CloudTrail ⭐ QUAN TRỌNG

**Mục đích:** Audit API calls và user activity

**Đặc điểm:**
- ✅ Logs tất cả API calls (who, what, when, where)
- ✅ Enabled by default (90 days)
- ✅ Có thể send logs tới S3, CloudWatch Logs
- ✅ Support cross-account logging

**Event Types:**
- **Management Events:** Control plane operations (create EC2, delete S3 bucket)
- **Data Events:** Data plane operations (S3 GetObject, Lambda Invoke)
- **Insights Events:** Detect unusual activity

**Best Practices:**
- ✅ Enable multi-region trail
- ✅ Enable log file validation (integrity)
- ✅ Encrypt logs với KMS
- ✅ Send logs tới separate account

**Ví dụ trong đề thi:**
> **Q:** Cần track ai đã xóa S3 bucket. Dùng service nào?
> 
> ✅ **CloudTrail** - Logs DeleteBucket API call

### AWS Config

**Mục đích:** Track resource configuration changes và compliance

**Đặc điểm:**
- ✅ Record configuration changes over time
- ✅ Evaluate compliance với Config Rules
- ✅ Remediate non-compliant resources automatically

**Config Rules Examples:**
- `s3-bucket-public-read-prohibited`
- `ec2-instance-managed-by-systems-manager`
- `rds-encryption-enabled`

**Ví dụ trong đề thi:**
> **Q:** Cần ensure tất cả S3 buckets có encryption enabled. Giải pháp nào?
> 
> ✅ **AWS Config** với rule `s3-bucket-server-side-encryption-enabled`

### CloudTrail vs Config vs CloudWatch

| Service | Purpose | What It Tracks |
|---------|---------|----------------|
| **CloudTrail** | Audit API calls | WHO did WHAT, WHEN |
| **Config** | Configuration changes | Resource configuration over time |
| **CloudWatch** | Monitoring | Metrics, logs, alarms |

**Ví dụ:**
```
CloudTrail: "User Alice called TerminateInstances at 2PM"
Config: "EC2 instance i-123 changed from t3.micro to t3.large"
CloudWatch: "EC2 CPU utilization is 80%"
```

---

## 🚨 Threat Detection & Protection

### Amazon GuardDuty ⭐

**Mục đích:** Intelligent threat detection sử dụng ML

**Data Sources:**
- VPC Flow Logs
- CloudTrail logs
- DNS logs
- Kubernetes audit logs

**Threat Types Detected:**
- Reconnaissance (port scanning, unusual API calls)
- Instance compromise (malware, backdoors)
- Account compromise (leaked credentials)
- Bucket compromise (unusual S3 access)

**Ưu điểm:**
- ✅ No agents to install
- ✅ Machine learning based
- ✅ 30-day free trial
- ✅ Integration với Security Hub, EventBridge

**Ví dụ trong đề thi:**
> **Q:** Cần detect unauthorized access và malicious activity. Giải pháp nào với least operational overhead?
> 
> ✅ **GuardDuty** - Fully managed, no agents

### AWS WAF (Web Application Firewall)

**Mục đích:** Protect web applications từ common exploits

**Deploy on:**
- ✅ CloudFront
- ✅ Application Load Balancer
- ✅ API Gateway
- ✅ AppSync

**Rule Types:**
- IP address filtering
- Geographic restrictions
- Rate limiting
- SQL injection protection
- XSS (Cross-Site Scripting) protection

**Ví dụ trong đề thi:**
> **Q:** Website đang bị DDoS attack. Cần block requests từ specific country. Giải pháp nào?
> 
> ✅ **AWS WAF** với geo-blocking rule

### AWS Shield

**Tiers:**

| Feature | Shield Standard | Shield Advanced |
|---------|-----------------|-----------------|
| **Cost** | Free | $3,000/month |
| **Protection** | Layer 3/4 DDoS | Layer 3/4/7 DDoS |
| **Services** | All AWS | CloudFront, Route 53, ELB, EC2 EIP |
| **Support** | Basic | 24/7 DDoS Response Team |
| **Cost Protection** | No | Yes (refund for scaling costs) |

**Khi nào dùng Shield Advanced:**
- ✅ Mission-critical applications
- ✅ Need 24/7 support
- ✅ Want cost protection
- ✅ Advanced attack visibility

---

## 🔒 Data Privacy & Protection

### Amazon Macie

**Mục đích:** Discover và protect sensitive data trong S3

**Capabilities:**
- ✅ Detect PII (Personally Identifiable Information)
- ✅ Financial data (credit cards)
- ✅ Healthcare data (PHI)
- ✅ Machine learning based

**Use Cases:**
- Data privacy compliance (GDPR, HIPAA)
- Prevent data leaks
- Data classification

**Ví dụ trong đề thi:**
> **Q:** S3 buckets chứa millions of files. Cần identify files có PII data. Giải pháp nào?
> 
> ✅ **Amazon Macie** - Automated PII discovery

### AWS Certificate Manager (ACM)

**Mục đích:** Provision và manage SSL/TLS certificates

**Ưu điểm:**
- ✅ Free certificates
- ✅ Automatic renewal
- ✅ Integration với ELB, CloudFront, API Gateway
- ✅ No certificate management overhead

**Ví dụ trong đề thi:**
> **Q:** Website cần HTTPS. Cách nào simplest để provision SSL certificate?
> 
> ✅ **ACM** - Free và automatic renewal

---

## 🎯 Kịch Bản Thi Thực Tế

### Scenario 1: EC2 Access to S3

**Đề bài:**
> Application chạy trên EC2 cần read/write S3 bucket. Cách nào MOST SECURE?

**Options:**
A. Store AWS access keys trong EC2 instance
B. Hardcode credentials trong application code
C. Create IAM role và attach vào EC2 instance
D. Use root account credentials

**Đáp án:** ✅ **C**

**Giải thích:**
- A, B: Không secure (credentials có thể leak)
- D: Never use root account
- C: Best practice - IAM role với temporary credentials

---

### Scenario 2: Cross-Account Access

**Đề bài:**
> Account A cần access S3 bucket trong Account B. Cách nào tốt nhất?

**Options:**
A. Share IAM user credentials
B. Make S3 bucket public
C. Create cross-account IAM role
D. Copy data giữa accounts

**Đáp án:** ✅ **C**

**Giải thích:**
- A: Security risk
- B: Exposed to internet
- D: Không scalable
- C: Secure cross-account access pattern

---

### Scenario 3: Encrypt RDS Database

**Đề bài:**
> RDS database cần encryption. Company muốn control encryption keys. Giải pháp nào?

**Options:**
A. RDS default encryption
B. RDS encryption với AWS managed KMS key
C. RDS encryption với customer managed KMS key
D. Application-level encryption

**Đáp án:** ✅ **C**

**Giải thích:**
- A: Không có encryption
- B: AWS manages key (không control được)
- C: Full control over keys, audit trail
- D: Complex, không cần thiết

---

### Scenario 4: Block Malicious IP

**Đề bài:**
> Website đang bị attack từ specific IP address. Cần block immediately. Giải pháp nào?

**Options:**
A. Modify Security Group
B. Modify Network ACL
C. Use AWS WAF
D. Use AWS Shield

**Đáp án:** ✅ **B hoặc C**

**Giải thích:**
- A: Security Groups không có DENY rules
- B: NACL có DENY rules, subnet-level
- C: WAF có IP filtering, application-level
- D: Shield cho DDoS, không block specific IP

---

### Scenario 5: Audit API Calls

**Đề bài:**
> Cần track ai đã terminate EC2 instance. Service nào cung cấp thông tin này?

**Options:**
A. CloudWatch
B. CloudTrail
C. AWS Config
D. VPC Flow Logs

**Đáp án:** ✅ **B**

**Giải thích:**
- A: Metrics và logs, không có API audit
- B: Logs tất cả API calls (who, what, when)
- C: Configuration changes, không có user info
- D: Network traffic, không có API calls

---

## ✅ Checklist Kiến Thức Security

### IAM
- [ ] Phân biệt Users, Groups, Roles, Policies
- [ ] Hiểu Policy Evaluation Logic (Explicit Deny wins)
- [ ] Biết khi nào dùng Roles thay vì Users
- [ ] Nhớ IAM Best Practices (8 điều)

### Encryption
- [ ] Phân biệt SSE-S3, SSE-KMS, SSE-C
- [ ] Hiểu KMS key types
- [ ] Biết Envelope Encryption
- [ ] Secrets Manager vs Parameter Store

### Network Security
- [ ] So sánh Security Groups vs NACLs
- [ ] Hiểu VPC segmentation best practices
- [ ] Biết khi nào dùng VPC Flow Logs
- [ ] Hiểu AWS PrivateLink

### Monitoring
- [ ] CloudTrail vs Config vs CloudWatch
- [ ] Biết GuardDuty use cases
- [ ] WAF vs Shield (Standard vs Advanced)
- [ ] Macie cho PII detection

---

## 🔗 Liên Kết

- **Tiếp theo:** [C. Kiến Trúc Web Application](C-kien-truc-web-app.md)
- **Áp dụng:** [K. Kịch Bản Thi](K-kich-ban-thi.md) - Nhiều câu hỏi security

---

*Security chiếm 30% đề thi - đây là phần QUAN TRỌNG NHẤT!*
