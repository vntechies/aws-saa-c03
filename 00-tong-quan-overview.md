# 📚 TỔNG QUAN - AWS SAA-C03 CHEAT SHEET

## 🎯 Giới Thiệu Về Kỳ Thi

**AWS Certified Solutions Architect - Associate (SAA-C03)** là chứng chỉ xác nhận khả năng thiết kế và triển khai các hệ thống phân tán trên AWS.

### Thông Tin Kỳ Thi
- **Số câu hỏi**: 65 câu
- **Thời gian**: 130 phút
- **Điểm đạt**: 720/1000
- **Định dạng**: Multiple choice và multiple response
- **Ngôn ngữ**: Tiếng Anh (có hỗ trợ tiếng Việt)
- **Chi phí**: $150 USD
- **Thời hạn**: 3 năm

---

## 📊 Cấu Trúc Nội Dung Thi

| Domain | Tỷ Trọng | Trọng Tâm |
|--------|----------|-----------|
| **Domain 1**: Thiết kế kiến trúc bảo mật | 30% | IAM, Security Groups, NACLs, Encryption |
| **Domain 2**: Thiết kế kiến trúc resilient | 26% | High Availability, Disaster Recovery, Backup |
| **Domain 3**: Thiết kế kiến trúc hiệu suất cao | 24% | Auto Scaling, Caching, Load Balancing |
| **Domain 4**: Thiết kế kiến trúc tối ưu chi phí | 20% | Cost optimization, Right-sizing, Reserved Instances |

---

## 🎓 Chiến Lược Ôn Thi

### 1. **Phương Pháp Học Hiệu Quả**

#### Giai Đoạn 1: Nền Tảng (2-3 tuần)
- ✅ Xem video courses (A Cloud Guru, Stephane Maarek)
- ✅ Đọc AWS Documentation cho các service chính
- ✅ Thực hành hands-on labs
- ✅ Tạo tài khoản AWS Free Tier

#### Giai Đoạn 2: Củng Cố (2-3 tuần)
- ✅ Làm practice tests (Tutorials Dojo, Whizlabs)
- ✅ Review lại các câu sai
- ✅ Tạo flashcards cho các khái niệm quan trọng
- ✅ Tham gia AWS study groups

#### Giai Đoạn 3: Hoàn Thiện (1 tuần)
- ✅ Làm mock exams trong điều kiện thật
- ✅ Review cheat sheets này
- ✅ Focus vào điểm yếu
- ✅ Nghỉ ngơi trước ngày thi

### 2. **Tips Làm Bài Thi**

#### ⏰ Quản Lý Thời Gian
- Dành **2 phút/câu** (130 phút cho 65 câu)
- Flag các câu khó, quay lại sau
- Dành 15-20 phút cuối để review

#### 🎯 Kỹ Thuật Trả Lời
- **Đọc kỹ câu hỏi**: Chú ý keywords như "MOST cost-effective", "LEAST operational overhead"
- **Loại trừ đáp án sai** trước
- **Chú ý đến scenario**: On-premises vs Cloud-native
- **Well-Architected Framework**: Luôn áp dụng 6 pillars

#### 🚫 Tránh Các Lỗi Thường Gặp
- ❌ Không đọc kỹ yêu cầu về cost, performance, security
- ❌ Chọn giải pháp quá phức tạp khi có option đơn giản hơn
- ❌ Bỏ qua keywords như "immediately", "minimum effort"
- ❌ Không hiểu rõ use case của từng service

---

## 🔑 Các Khái Niệm Quan Trọng

### 1. **AWS Well-Architected Framework - 6 Pillars**

#### 🔒 Operational Excellence
- IaC (Infrastructure as Code)
- Monitoring và logging
- Automation
- Continuous improvement

#### 🛡️ Security
- Identity and Access Management
- Detective controls
- Infrastructure protection
- Data protection
- Incident response

#### 🔄 Reliability
- Recover from failures
- Horizontal scaling
- Stop guessing capacity
- Manage change through automation

#### ⚡ Performance Efficiency
- Democratize advanced technologies
- Go global in minutes
- Use serverless architectures
- Experiment more often

#### 💰 Cost Optimization
- Adopt consumption model
- Measure overall efficiency
- Stop spending on data center operations
- Analyze and attribute expenditure

#### 🌱 Sustainability
- Understand your impact
- Establish sustainability goals
- Maximize utilization
- Use managed services

### 2. **Shared Responsibility Model**

#### AWS Chịu Trách Nhiệm (Security OF the Cloud)
- Physical infrastructure
- Networking infrastructure
- Hypervisor
- Managed services

#### Customer Chịu Trách Nhiệm (Security IN the Cloud)
- Customer data
- Platform, applications
- IAM
- OS, network, firewall configuration
- Client-side encryption
- Server-side encryption

---

## 📖 Cấu Trúc Cheat Sheet

### Độ Ưu Tiên Các Services

#### 🔴 **CRITICAL** (Phải thuộc 100%)
1. **Compute**: EC2, Lambda, ECS, Auto Scaling
2. **Storage**: S3, EBS, EFS, Storage Gateway
3. **Database**: RDS, DynamoDB, Aurora, ElastiCache
4. **Networking**: VPC, Route 53, CloudFront, ELB

#### 🟠 **HIGH** (Phải hiểu rõ 80-90%)
5. **Security**: IAM, KMS, CloudTrail, GuardDuty
6. **Management**: CloudWatch, CloudFormation, Systems Manager
7. **Application Integration**: SQS, SNS, EventBridge

#### 🟡 **MEDIUM** (Hiểu khái niệm chính 60-70%)
8. **Developer Tools**: CodeDeploy, CodePipeline
9. **Analytics**: Kinesis, Athena, EMR
10. **Migration**: DMS, Snow Family, DataSync
11. **API**: API Gateway, AppSync

#### 🟢 **LOW** (Biết cơ bản 30-40%)
12. **ML/AI**: SageMaker, Rekognition, Comprehend
13. **Other Services**: WorkSpaces, AppStream

---

## 🎯 Keywords Quan Trọng Trong Đề Thi

### Cost-Related Keywords
- **"MOST cost-effective"** → Reserved Instances, Spot Instances, S3 Intelligent-Tiering
- **"Minimize costs"** → Serverless, Auto Scaling, S3 Lifecycle
- **"Pay only for what you use"** → Lambda, Fargate

### Performance Keywords
- **"Lowest latency"** → CloudFront, ElastiCache, Global Accelerator
- **"High performance"** → Provisioned IOPS, Enhanced Networking
- **"Real-time"** → Kinesis Data Streams, DynamoDB Streams

### Availability Keywords
- **"Highly available"** → Multi-AZ, Multi-Region, ELB
- **"Fault-tolerant"** → Auto Scaling, Route 53 health checks
- **"Disaster recovery"** → Backup, Pilot Light, Warm Standby

### Security Keywords
- **"Encrypt at rest"** → KMS, S3 encryption
- **"Encrypt in transit"** → SSL/TLS, VPN
- **"Least privilege"** → IAM policies, SCPs

### Operational Keywords
- **"Minimum operational overhead"** → Managed services, Serverless
- **"Automated"** → CloudFormation, Lambda, EventBridge
- **"Immediately"** → CloudFront, Route 53, Lambda

---

## 📚 Tài Nguyên Học Tập

### Video Courses
- ⭐ **Stephane Maarek** - Ultimate AWS Certified Solutions Architect Associate (Udemy)
- ⭐ **A Cloud Guru** - AWS Certified Solutions Architect Associate
- **Adrian Cantrill** - AWS Certified Solutions Architect Associate

### Practice Tests
- ⭐⭐⭐ **Tutorials Dojo** (Jon Bonso) - Highly recommended
- **Whizlabs** - AWS SAA Practice Tests
- **AWS Official Practice Exam**

### Documentation
- **AWS Documentation** - docs.aws.amazon.com
- **AWS Whitepapers** - aws.amazon.com/whitepapers
- **AWS FAQs** - Đọc FAQs của các service chính

### Hands-On
- **AWS Free Tier** - aws.amazon.com/free
- **AWS Well-Architected Labs** - wellarchitectedlabs.com
- **QwikLabs** / **A Cloud Guru Labs**

---

## 🎯 Checklist Trước Kỳ Thi

### 1 Tuần Trước
- [ ] Hoàn thành ít nhất 3 mock exams với điểm >80%
- [ ] Review lại tất cả câu sai
- [ ] Đọc lại cheat sheets
- [ ] Làm flashcards cho concepts khó

### 3 Ngày Trước
- [ ] Làm 1 mock exam cuối
- [ ] Review Well-Architected Framework
- [ ] Ôn lại service comparisons
- [ ] Nghỉ ngơi đầy đủ

### Ngày Thi
- [ ] Đến sớm 15 phút
- [ ] Mang theo 2 IDs
- [ ] Tắt điện thoại
- [ ] Thư giãn và tự tin

---

## 💡 Lời Khuyên Cuối Cùng

### Mindset
- ✨ **Đừng học vẹt** - Hiểu WHY, không chỉ WHAT
- ✨ **Think like an architect** - Focus vào trade-offs
- ✨ **Hands-on practice** - Tạo và phá hủy resources
- ✨ **Learn from mistakes** - Review mọi câu sai

### Trong Phòng Thi
- 🎯 Đọc câu hỏi 2 lần
- 🎯 Tìm keywords quan trọng
- 🎯 Loại trừ đáp án sai trước
- 🎯 Flag và quay lại câu khó
- 🎯 Tin vào instinct đầu tiên

---

## 📋 Cấu Trúc Files

```
00-tong-quan-overview.md ← BẠN ĐANG Ở ĐÂY
01-compute-services.md - CRITICAL ⭐⭐⭐
02-storage-services.md - CRITICAL ⭐⭐⭐
03-database-services.md - CRITICAL ⭐⭐⭐
04-networking-services.md - CRITICAL ⭐⭐⭐
05-security-services.md - HIGH ⭐⭐
06-management-governance.md - HIGH ⭐⭐
07-application-integration.md - HIGH ⭐⭐
08-developer-tools.md - MEDIUM ⭐
09-analytics-bigdata.md - MEDIUM ⭐
10-migration-transfer.md - MEDIUM ⭐
11-api-integration.md - MEDIUM ⭐
12-ml-ai.md - LOW
13-other-services.md - LOW
14-so-sanh-services.md - QUAN TRỌNG NHẤT ⭐⭐⭐
```

---

## 🚀 Bắt Đầu Học

**Khuyến nghị đọc theo thứ tự:**
1. File này (Overview) - Hiểu cấu trúc thi
2. Files CRITICAL (01-04) - Nền tảng quan trọng nhất
3. Files HIGH (05-07) - Services hay ra thi
4. File 14 (So sánh) - Phân biệt các services
5. Files MEDIUM (08-11) - Kiến thức bổ sung
6. Files LOW (12-13) - Đọc nhanh

---

**Chúc bạn học tốt và đạt chứng chỉ AWS SAA-C03! 🎉**

*Cập nhật lần cuối: 2026*
