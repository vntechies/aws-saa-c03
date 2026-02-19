 # 📝 S. ĐỀ THI THỬ AWS SAA-C03

> **65 câu hỏi theo đúng format và độ khó của đề thi thực tế**
> 
> **Thời gian:** 130 phút | **Điểm đỗ:** 720/1000 (72%)

---

## 📋 Hướng Dẫn

### Cấu Trúc Đề Thi
- **Tổng số câu:** 65 câu
- **Domain 1 (Security):** 20 câu (30%)
- **Domain 2 (Resilient Architecture):** 17 câu (26%)
- **Domain 3 (High-Performance):** 16 câu (24%)
- **Domain 4 (Cost Optimization):** 12 câu (20%)

### Cách Làm Bài
1. Đọc kỹ câu hỏi và xác định keywords
2. Loại trừ 2 đáp án sai rõ ràng
3. So sánh 2 đáp án còn lại dựa trên requirements
4. Chọn đáp án phù hợp nhất (MOST, BEST, LEAST)
5. Đánh dấu câu khó để xem lại sau

### Đáp Án
- Đáp án chi tiết ở cuối file
- Giải thích tại sao đúng/sai cho mỗi option

---

## 🔐 DOMAIN 1: SECURE ARCHITECTURES (20 câu - 30%)

### Câu 1
A company is deploying a web application on Amazon EC2 instances. The EC2 instances need to access objects in an Amazon S3 bucket. What is the MOST secure way to grant the EC2 instances access to the S3 bucket?

**A.** Create an IAM user with programmatic access and store the access keys in the EC2 instance user data

**B.** Create an IAM role with the necessary permissions and attach the role to the EC2 instances

**C.** Create an S3 bucket policy that allows access from the EC2 instance IP addresses

**D.** Store AWS credentials in the application configuration file on the EC2 instances

---

### Câu 2
A solutions architect needs to encrypt data at rest in Amazon S3. The company wants to maintain full control over the encryption keys and rotate them automatically every year. Which solution meets these requirements?

**A.** Use S3 default encryption with Amazon S3-managed keys (SSE-S3)

**B.** Use S3 encryption with AWS KMS customer managed keys (SSE-KMS) and enable automatic key rotation

**C.** Use S3 encryption with customer-provided keys (SSE-C) and implement a Lambda function to rotate keys

**D.** Use client-side encryption before uploading to S3

---

### Câu 3
A company wants to track all API calls made to AWS services for security auditing purposes. Which AWS service should the solutions architect recommend?

**A.** Amazon CloudWatch

**B.** AWS CloudTrail

**C.** AWS Config

**D.** AWS X-Ray

---

### Câu 4
A solutions architect is designing a VPC security strategy. The company wants to prevent a specific IP address range (198.51.100.0/24) from accessing EC2 instances in the VPC. Which solution should the architect implement?

**A.** Configure security groups to deny traffic from 198.51.100.0/24

**B.** Configure network ACLs to deny traffic from 198.51.100.0/24

**C.** Configure AWS WAF to block traffic from 198.51.100.0/24

**D.** Configure Route 53 to block DNS requests from 198.51.100.0/24

---

### Câu 5
A company needs to share data in an Amazon S3 bucket with a partner company that has its own AWS account. What is the MOST secure way to grant access?

**A.** Create an IAM user in the company's account and share the credentials with the partner

**B.** Make the S3 bucket public and share the bucket URL with the partner

**C.** Create an IAM role with a trust policy that allows the partner's AWS account to assume the role

**D.** Create a pre-signed URL for the S3 objects and share it with the partner

---

### Câu 6
A solutions architect needs to ensure that all data stored in Amazon RDS databases is encrypted. The company requires the ability to bring its own encryption keys. Which solution meets these requirements?

**A.** Use RDS encryption with AWS managed keys

**B.** Use RDS encryption with AWS KMS customer managed keys

**C.** Use RDS encryption with AWS CloudHSM

**D.** Implement application-level encryption before storing data in RDS

---

### Câu 7
A company wants to detect and respond to potential security threats in real-time across AWS accounts. Which service should be implemented?

**A.** AWS Config

**B.** Amazon GuardDuty

**C.** AWS Inspector

**D.** AWS CloudTrail

---

### Câu 8
A solutions architect needs to implement a solution that automatically rotates database credentials every 30 days. Which service should be used?

**A.** AWS Systems Manager Parameter Store

**B.** AWS Secrets Manager

**C.** AWS KMS

**D.** Amazon S3 with lifecycle policies

---

### Câu 9
A company requires all Amazon S3 buckets to have server-side encryption enabled and wants to automatically check compliance. Which service should be used?

**A.** AWS CloudTrail

**B.** Amazon CloudWatch

**C.** AWS Config

**D.** AWS Inspector

---

### Câu 10
A web application is experiencing a DDoS attack. Which AWS service provides automatic protection against Layer 3 and Layer 4 DDoS attacks at no additional cost?

**A.** AWS WAF

**B.** AWS Shield Standard

**C.** AWS Shield Advanced

**D.** Amazon GuardDuty

---

### Câu 11
A company needs to scan Amazon S3 buckets for sensitive data such as personally identifiable information (PII). Which service should be used?

**A.** Amazon GuardDuty

**B.** AWS Macie

**C.** AWS Inspector

**D.** AWS Config

---

### Câu 12
A solutions architect is designing a multi-tier application. The application tier needs to access the database tier, but the database tier should not be accessible from the internet. How should the VPC be configured?

**A.** Place both tiers in public subnets with security groups

**B.** Place the application tier in a public subnet and the database tier in a private subnet

**C.** Place both tiers in private subnets with a NAT gateway

**D.** Place the application tier in a private subnet and the database tier in a public subnet

---

### Câu 13
A company wants to provide temporary access to an Amazon S3 bucket for external users without creating IAM users. Which solution should be implemented?

**A.** Create IAM users for each external user

**B.** Generate pre-signed URLs with expiration times

**C.** Make the S3 bucket public

**D.** Use S3 bucket policies to allow anonymous access

---

### Câu 14
A solutions architect needs to implement a solution where EC2 instances can securely access AWS services without traversing the internet. Which solution should be used?

**A.** NAT Gateway

**B.** Internet Gateway

**C.** VPC Endpoints

**D.** VPN Connection

---

### Câu 15
A company needs to ensure that all EC2 instances are patched with the latest security updates. Which service should be used to automate this process?

**A.** AWS Systems Manager Patch Manager

**B.** AWS Config

**C.** AWS CloudFormation

**D.** AWS Lambda

---

### Câu 16
A solutions architect is implementing MFA (Multi-Factor Authentication) for AWS console access. Which AWS service should be configured?

**A.** AWS IAM

**B.** Amazon Cognito

**C.** AWS Directory Service

**D.** AWS Single Sign-On

---

### Câu 17
A company wants to protect its web application from common web exploits such as SQL injection and cross-site scripting (XSS). Which service should be implemented?

**A.** AWS Shield

**B.** AWS WAF

**C.** Amazon GuardDuty

**D.** AWS Firewall Manager

---

### Câu 18
A solutions architect needs to implement encryption in transit for data traveling between EC2 instances and an Application Load Balancer. What should be configured?

**A.** Enable encryption on the EC2 instances

**B.** Configure SSL/TLS certificates on the Application Load Balancer using AWS Certificate Manager

**C.** Use AWS KMS to encrypt the traffic

**D.** Enable VPC Flow Logs

---

### Câu 19
A company requires that all API calls to AWS services be logged and stored for 7 years for compliance purposes. Which solution meets this requirement MOST cost-effectively?

**A.** Enable CloudTrail and store logs in S3 Standard

**B.** Enable CloudTrail and store logs in S3 Glacier Deep Archive with a lifecycle policy

**C.** Enable CloudWatch Logs and set retention to 7 years

**D.** Enable AWS Config and store configuration history for 7 years

---

### Câu 20
A solutions architect needs to implement network traffic monitoring to detect suspicious patterns. Which service provides this capability?

**A.** VPC Flow Logs

**B.** AWS CloudTrail

**C.** Amazon GuardDuty

**D.** AWS Config

---

## 🏗️ DOMAIN 2: RESILIENT ARCHITECTURES (17 câu - 26%)

### Câu 21
A company needs to ensure that its Amazon RDS database remains available in the event of an Availability Zone failure. Which solution provides automatic failover with minimal downtime?

**A.** Create a Read Replica in a different Availability Zone

**B.** Enable Multi-AZ deployment for the RDS instance

**C.** Create manual snapshots and store them in S3

**D.** Use AWS Backup to create automated backups

---

### Câu 22
A web application is experiencing variable traffic patterns. The solutions architect needs to ensure the application can automatically scale to handle traffic spikes. Which solution should be implemented?

**A.** Use larger EC2 instances to handle peak traffic

**B.** Implement an Auto Scaling group with target tracking scaling policy

**C.** Use Reserved Instances to ensure capacity

**D.** Manually add EC2 instances during peak hours

---

### Câu 23
A company wants to deploy a highly available web application across multiple Availability Zones. Which architecture provides the BEST availability?

**A.** Deploy EC2 instances in a single Availability Zone with an Elastic IP address

**B.** Deploy EC2 instances in multiple Availability Zones behind an Application Load Balancer

**C.** Deploy a single large EC2 instance with increased resources

**D.** Deploy EC2 instances in a single Availability Zone with Auto Scaling

---

### Câu 24
A solutions architect is designing a disaster recovery solution for a critical database. The company requires an RPO (Recovery Point Objective) of 5 minutes and an RTO (Recovery Time Objective) of 1 hour. Which solution meets these requirements?

**A.** Backup and Restore: Daily snapshots to S3

**B.** Pilot Light: Core database running in secondary region with continuous replication

**C.** Warm Standby: Scaled-down version running in secondary region

**D.** Multi-Site Active-Active: Full capacity in both regions

---

### Câu 25
A company needs to ensure that its Amazon S3 data is protected against accidental deletion. Which feature should be enabled?

**A.** S3 Lifecycle policies

**B.** S3 Versioning

**C.** S3 Transfer Acceleration

**D.** S3 Intelligent-Tiering

---

### Câu 26
A solutions architect needs to distribute traffic across multiple EC2 instances in different Availability Zones. The application uses HTTP and HTTPS protocols. Which load balancer should be used?

**A.** Classic Load Balancer

**B.** Network Load Balancer

**C.** Application Load Balancer

**D.** Gateway Load Balancer

---

### Câu 27
A company wants to implement a backup strategy for Amazon EBS volumes. The backups should be automated and stored across multiple Availability Zones. Which solution meets these requirements?

**A.** Use AWS Backup to create automated EBS snapshots

**B.** Create manual EBS snapshots daily

**C.** Use Amazon S3 to store EBS data

**D.** Use Amazon EFS instead of EBS

---

### Câu 28
A solutions architect is designing a solution to ensure that a static website hosted on Amazon S3 remains available even if an entire AWS region fails. Which solution provides the BEST availability?

**A.** Enable S3 Versioning

**B.** Enable S3 Cross-Region Replication to a bucket in another region

**C.** Use S3 Transfer Acceleration

**D.** Enable S3 Lifecycle policies

---

### Câu 29
A company needs to ensure that its application can handle the failure of a single EC2 instance without service disruption. Which solution should be implemented?

**A.** Use a larger EC2 instance type

**B.** Deploy multiple EC2 instances across multiple Availability Zones with an Elastic Load Balancer

**C.** Use Amazon EC2 Auto Recovery

**D.** Create an AMI and manually launch a new instance when failure occurs

---

### Câu 30
A solutions architect needs to implement a solution where database reads can be scaled independently from writes. Which solution should be used?

**A.** Use a larger RDS instance

**B.** Implement RDS Multi-AZ deployment

**C.** Create RDS Read Replicas

**D.** Use DynamoDB instead of RDS

---

### Câu 31
A company wants to ensure that its Amazon Aurora database can failover to another AWS region in case of a regional outage. Which feature should be enabled?

**A.** Aurora Multi-AZ

**B.** Aurora Read Replicas

**C.** Aurora Global Database

**D.** Aurora Serverless

---

### Câu 32
A solutions architect is designing a message queue system that can handle temporary spikes in message volume without losing messages. Which service should be used?

**A.** Amazon SNS

**B.** Amazon SQS

**C.** Amazon Kinesis

**D.** AWS Step Functions

---

### Câu 33
A company needs to implement health checks for EC2 instances behind an Application Load Balancer. What happens when an instance fails a health check?

**A.** The instance is terminated immediately

**B.** The load balancer stops sending traffic to the instance

**C.** The instance is automatically replaced

**D.** An alarm is triggered but traffic continues

---

### Câu 34
A solutions architect needs to ensure that an application can continue to function even if an entire Availability Zone becomes unavailable. Which minimum number of Availability Zones should the application be deployed across?

**A.** 1

**B.** 2

**C.** 3

**D.** 4

---

### Câu 35
A company wants to implement automatic failover for its DNS records in case the primary region becomes unavailable. Which Route 53 routing policy should be used?

**A.** Simple routing

**B.** Weighted routing

**C.** Failover routing

**D.** Latency-based routing

---

### Câu 36
A solutions architect is designing a solution to protect against data loss in Amazon DynamoDB. Which feature provides point-in-time recovery?

**A.** DynamoDB Streams

**B.** DynamoDB Accelerator (DAX)

**C.** DynamoDB Global Tables

**D.** DynamoDB Point-in-Time Recovery (PITR)

---

### Câu 37
A company needs to ensure that its application remains available during planned maintenance windows. Which solution provides zero-downtime deployments?

**A.** In-place deployment

**B.** Blue/Green deployment

**C.** Rolling deployment with one instance at a time

**D.** Big bang deployment

---

## ⚡ DOMAIN 3: HIGH-PERFORMING ARCHITECTURES (16 câu - 24%)

### Câu 38
A web application is experiencing high latency for users in Europe, while the application is hosted in us-east-1. Which solution reduces latency MOST effectively?

**A.** Use larger EC2 instances

**B.** Implement Amazon CloudFront as a CDN

**C.** Enable Auto Scaling

**D.** Use Elastic Load Balancing

---

### Câu 39
A database is experiencing performance issues due to high read traffic. Which solution improves read performance without modifying the application code?

**A.** Upgrade to a larger RDS instance

**B.** Enable RDS Multi-AZ

**C.** Create RDS Read Replicas

**D.** Migrate to Amazon Redshift

---

### Câu 40
A company needs to cache frequently accessed data to reduce database load and improve application response time. Which service should be used?

**A.** Amazon S3

**B.** Amazon ElastiCache

**C.** Amazon EBS

**D.** Amazon EFS

---

### Câu 41
A solutions architect needs to design a solution for real-time data processing with sub-second latency. Which service is MOST appropriate?

**A.** Amazon S3

**B.** Amazon Kinesis Data Streams

**C.** AWS Batch

**D.** Amazon SQS

---

### Câu 42
An application requires extremely low latency (microseconds) for database queries. Which solution provides the LOWEST latency?

**A.** Amazon RDS

**B.** Amazon DynamoDB

**C.** Amazon DynamoDB with DynamoDB Accelerator (DAX)

**D.** Amazon Aurora

---

### Câu 43
A company needs to process large amounts of data in parallel. Which EC2 placement group type provides the LOWEST network latency between instances?

**A.** Spread placement group

**B.** Partition placement group

**C.** Cluster placement group

**D.** Default placement (no group)

---

### Câu 44
A solutions architect is designing a solution to serve static content (images, CSS, JavaScript) with low latency to global users. Which solution is MOST cost-effective?

**A.** Deploy EC2 instances in multiple regions

**B.** Use Amazon CloudFront with an S3 origin

**C.** Use Elastic Load Balancing across regions

**D.** Use AWS Global Accelerator

---

### Câu 45
An application requires high IOPS (over 20,000) for its database storage. Which EBS volume type should be used?

**A.** General Purpose SSD (gp3)

**B.** Provisioned IOPS SSD (io2)

**C.** Throughput Optimized HDD (st1)

**D.** Cold HDD (sc1)

---

### Câu 46
A company needs to improve the performance of database queries that involve complex joins. Which AWS service is optimized for analytical queries?

**A.** Amazon RDS

**B.** Amazon DynamoDB

**C.** Amazon Redshift

**D.** Amazon ElastiCache

---

### Câu 47
A solutions architect needs to reduce the time required to upload large files (over 5 GB) to Amazon S3. Which feature should be used?

**A.** S3 Transfer Acceleration

**B.** S3 Multipart Upload

**C.** S3 Versioning

**D.** S3 Lifecycle policies

---

### Câu 48
An application experiences performance degradation during peak hours due to high CPU utilization. Which solution automatically adjusts capacity based on demand?

**A.** Use Reserved Instances

**B.** Implement Auto Scaling with target tracking policy

**C.** Use Spot Instances

**D.** Manually add instances during peak hours

---

### Câu 49
A company needs to accelerate TCP and UDP traffic for a gaming application with users worldwide. Which service should be used?

**A.** Amazon CloudFront

**B.** AWS Global Accelerator

**C.** Elastic Load Balancing

**D.** Amazon Route 53

---

### Câu 50
A solutions architect is designing a solution for a data warehouse that needs to query petabytes of data. Which service is MOST appropriate?

**A.** Amazon RDS

**B.** Amazon DynamoDB

**C.** Amazon Redshift

**D.** Amazon Aurora

---

### Câu 51
An application needs to process messages from a queue with guaranteed order. Which service should be used?

**A.** Amazon SQS Standard Queue

**B.** Amazon SQS FIFO Queue

**C.** Amazon SNS

**D.** Amazon Kinesis Data Firehose

---

### Câu 52
A company wants to improve the performance of its website by caching API responses. Which service provides API caching?

**A.** Amazon CloudFront

**B.** Amazon API Gateway

**C.** Elastic Load Balancing

**D.** Amazon ElastiCache

---

### Câu 53
A solutions architect needs to design a solution that provides consistent single-digit millisecond latency for a NoSQL database. Which service meets this requirement?

**A.** Amazon RDS

**B.** Amazon DynamoDB

**C.** Amazon Redshift

**D.** Amazon Aurora

---

## 💰 DOMAIN 4: COST-OPTIMIZED ARCHITECTURES (12 câu - 20%)

### Câu 54
A company runs EC2 instances 24/7 for 3 years. Which pricing model provides the MOST cost savings?

**A.** On-Demand Instances

**B.** Spot Instances

**C.** Reserved Instances (3-year term)

**D.** Savings Plans

---

### Câu 55
A solutions architect needs to store infrequently accessed data that must be retrieved within milliseconds when needed. Which S3 storage class is MOST cost-effective?

**A.** S3 Standard

**B.** S3 Intelligent-Tiering

**C.** S3 Standard-IA

**D.** S3 Glacier

---

### Câu 56
A company has unpredictable workloads and wants to minimize costs. Which EC2 pricing model is MOST appropriate?

**A.** On-Demand Instances

**B.** Reserved Instances

**C.** Spot Instances

**D.** Dedicated Hosts

---

### Câu 57
A solutions architect needs to reduce data transfer costs between EC2 instances and S3 in the same region. What should be implemented?

**A.** Use CloudFront

**B.** Use VPC Endpoints for S3

**C.** Use Direct Connect

**D.** Use NAT Gateway

---

### Câu 58
A company wants to automatically move S3 objects to lower-cost storage classes based on access patterns. Which feature should be used?

**A.** S3 Versioning

**B.** S3 Lifecycle policies

**C.** S3 Replication

**D.** S3 Object Lock

---

### Câu 59
A development team needs EC2 instances for testing that run 8 hours per day, 5 days per week. Which pricing model is MOST cost-effective?

**A.** On-Demand Instances

**B.** Reserved Instances

**C.** Spot Instances

**D.** Dedicated Instances

---

### Câu 60
A company has data that is accessed once per year for compliance purposes. Which S3 storage class provides the LOWEST cost?

**A.** S3 Standard-IA

**B.** S3 One Zone-IA

**C.** S3 Glacier Flexible Retrieval

**D.** S3 Glacier Deep Archive

---

### Câu 61
A solutions architect needs to reduce costs for a database that has unpredictable usage patterns. Which solution is MOST cost-effective?

**A.** RDS with Reserved Instances

**B.** RDS with On-Demand Instances

**C.** Aurora Serverless

**D.** DynamoDB with Provisioned Capacity

---

### Câu 62
A company wants to reduce costs for compute resources that can tolerate interruptions. Which EC2 pricing model should be used?

**A.** On-Demand Instances

**B.** Reserved Instances

**C.** Spot Instances

**D.** Savings Plans

---

### Câu 63
A solutions architect is designing a solution to reduce NAT Gateway costs. Which solution is MOST cost-effective for EC2 instances that need to download software updates from the internet?

**A.** Use multiple NAT Gateways

**B.** Use NAT Instances instead of NAT Gateways

**C.** Use VPC Endpoints where possible and a single NAT Gateway for other traffic

**D.** Use an Internet Gateway for all traffic

---

### Câu 64
A company needs to analyze CloudWatch Logs data to identify cost optimization opportunities. Which service should be used?

**A.** AWS Cost Explorer

**B.** AWS Budgets

**C.** AWS Trusted Advisor

**D.** CloudWatch Logs Insights

---

### Câu 65
A solutions architect needs to reduce storage costs for EBS volumes that are infrequently accessed. Which solution is MOST cost-effective?

**A.** Use EBS gp3 volumes

**B.** Use EBS sc1 (Cold HDD) volumes

**C.** Create EBS snapshots and delete the volumes

**D.** Use EBS io2 volumes

---

## ✅ ĐÁP ÁN CHI TIẾT

### DOMAIN 1: SECURE ARCHITECTURES

**Câu 1: B**
- A: ❌ Storing credentials is insecure
- **B: ✅ IAM roles provide temporary credentials, most secure**
- C: ❌ IP-based policies are not scalable
- D: ❌ Storing credentials in files is insecure

**Câu 2: B**
- A: ❌ No control over keys
- **B: ✅ Customer managed KMS keys with automatic rotation**
- C: ❌ Manual rotation, operational overhead
- D: ❌ No automatic rotation

**Câu 3: B**
- A: ❌ CloudWatch is for monitoring, not API auditing
- **B: ✅ CloudTrail logs all API calls**
- C: ❌ Config tracks configuration changes
- D: ❌ X-Ray is for application tracing

**Câu 4: B**
- A: ❌ Security Groups don't have DENY rules
- **B: ✅ NACLs support DENY rules**
- C: ❌ WAF is for application layer, not network layer
- D: ❌ Route 53 doesn't block IPs

**Câu 5: C**
- A: ❌ Sharing credentials is insecure
- B: ❌ Public bucket is not secure
- **C: ✅ Cross-account IAM role is best practice**
- D: ❌ Pre-signed URLs are temporary, not for permanent access

**Câu 6: C**
- A: ❌ AWS manages keys
- B: ❌ AWS manages keys (customer managed but in AWS)
- **C: ✅ CloudHSM allows BYOK (Bring Your Own Key)**
- D: ❌ Not required if RDS encryption available

**Câu 7: B**
- A: ❌ Config is for compliance, not threat detection
- **B: ✅ GuardDuty detects threats in real-time**
- C: ❌ Inspector is for vulnerability scanning
- D: ❌ CloudTrail logs events, doesn't detect threats

**Câu 8: B**
- A: ❌ Parameter Store doesn't auto-rotate
- **B: ✅ Secrets Manager has automatic rotation**
- C: ❌ KMS is for key management
- D: ❌ S3 is for storage, not secrets

**Câu 9: C**
- A: ❌ CloudTrail logs API calls
- B: ❌ CloudWatch monitors metrics
- **C: ✅ Config checks compliance with rules**
- D: ❌ Inspector scans for vulnerabilities

**Câu 10: B**
- A: ❌ WAF is Layer 7, requires configuration
- **B: ✅ Shield Standard is free and automatic**
- C: ❌ Shield Advanced costs $3,000/month
- D: ❌ GuardDuty detects threats, doesn't protect

**Câu 11: B**
- A: ❌ GuardDuty is for threat detection
- **B: ✅ Macie discovers and protects sensitive data**
- C: ❌ Inspector is for EC2 vulnerabilities
- D: ❌ Config is for compliance

**Câu 12: B**
- A: ❌ Database shouldn't be in public subnet
- **B: ✅ Correct architecture: public app, private DB**
- C: ❌ App tier needs internet access
- D: ❌ Database shouldn't be public

**Câu 13: B**
- A: ❌ Creating IAM users for external users is not recommended
- **B: ✅ Pre-signed URLs provide temporary access**
- C: ❌ Public bucket is not secure
- D: ❌ Anonymous access is not secure

**Câu 14: C**
- A: ❌ NAT Gateway is for outbound internet access
- B: ❌ Internet Gateway is for public access
- **C: ✅ VPC Endpoints provide private connectivity**
- D: ❌ VPN is for on-premises connection

**Câu 15: A**
- **A: ✅ Patch Manager automates patching**
- B: ❌ Config tracks compliance, doesn't patch
- C: ❌ CloudFormation is for IaC
- D: ❌ Lambda would require custom code

**Câu 16: A**
- **A: ✅ IAM manages MFA for console access**
- B: ❌ Cognito is for application users
- C: ❌ Directory Service is for Active Directory
- D: ❌ SSO is for federated access

**Câu 17: B**
- A: ❌ Shield is for DDoS
- **B: ✅ WAF protects against web exploits**
- C: ❌ GuardDuty detects threats
- D: ❌ Firewall Manager manages security policies

**Câu 18: B**
- A: ❌ Encryption on instances doesn't cover transit
- **B: ✅ SSL/TLS on ALB encrypts in transit**
- C: ❌ KMS is for at-rest encryption
- D: ❌ Flow Logs don't encrypt traffic

**Câu 19: B**
- A: ❌ S3 Standard is expensive for 7 years
- **B: ✅ Glacier Deep Archive is cheapest for long-term**
- C: ❌ CloudWatch Logs is more expensive
- D: ❌ Config history is expensive

**Câu 20: C**
- A: ❌ Flow Logs capture traffic but don't detect patterns
- B: ❌ CloudTrail logs API calls
- **C: ✅ GuardDuty analyzes traffic for threats**
- D: ❌ Config tracks configuration

### DOMAIN 2: RESILIENT ARCHITECTURES

**Câu 21: B**
- A: ❌ Read Replica doesn't provide automatic failover
- **B: ✅ Multi-AZ provides automatic failover**
- C: ❌ Manual snapshots require manual restore
- D: ❌ Backup doesn't provide automatic failover

**Câu 22: B**
- A: ❌ Larger instances don't auto-scale
- **B: ✅ Auto Scaling with target tracking scales automatically**
- C: ❌ Reserved Instances don't provide scaling
- D: ❌ Manual scaling is not automatic

**Câu 23: B**
- A: ❌ Single AZ is not highly available
- **B: ✅ Multi-AZ with ALB provides best availability**
- C: ❌ Single instance is single point of failure
- D: ❌ Single AZ is not highly available

**Câu 24: B**
- A: ❌ Daily snapshots don't meet RPO of 5 minutes
- **B: ✅ Pilot Light with continuous replication meets requirements**
- C: ❌ More expensive than needed
- D: ❌ Most expensive, overkill

**Câu 25: B**
- A: ❌ Lifecycle policies move/delete objects
- **B: ✅ Versioning protects against deletion**
- C: ❌ Transfer Acceleration speeds uploads
- D: ❌ Intelligent-Tiering is for cost optimization

**Câu 26: C**
- A: ❌ CLB is legacy
- B: ❌ NLB is Layer 4 (TCP/UDP)
- **C: ✅ ALB is Layer 7 (HTTP/HTTPS)**
- D: ❌ GWLB is for appliances

**Câu 27: A**
- **A: ✅ AWS Backup automates EBS snapshots**
- B: ❌ Manual snapshots are not automated
- C: ❌ S3 doesn't store EBS data directly
- D: ❌ EFS is different use case

**Câu 28: B**
- A: ❌ Versioning doesn't protect against region failure
- **B: ✅ Cross-Region Replication provides region-level DR**
- C: ❌ Transfer Acceleration speeds uploads
- D: ❌ Lifecycle policies don't provide DR

**Câu 29: B**
- A: ❌ Larger instance is still single point of failure
- **B: ✅ Multiple instances with ELB provides redundancy**
- C: ❌ Auto Recovery has downtime
- D: ❌ Manual launch has downtime

**Câu 30: C**
- A: ❌ Larger instance doesn't scale reads independently
- B: ❌ Multi-AZ is for HA, not read scaling
- **C: ✅ Read Replicas scale reads independently**
- D: ❌ DynamoDB is different data model

**Câu 31: C**
- A: ❌ Multi-AZ is within same region
- B: ❌ Read Replicas don't provide automatic regional failover
- **C: ✅ Global Database provides cross-region DR**
- D: ❌ Serverless is for scaling, not DR

**Câu 32: B**
- A: ❌ SNS is pub/sub, not queue
- **B: ✅ SQS is message queue with buffering**
- C: ❌ Kinesis is for streaming
- D: ❌ Step Functions is for orchestration

**Câu 33: B**
- A: ❌ Instance is not terminated
- **B: ✅ Load balancer stops routing traffic**
- C: ❌ Auto Scaling terminates, not LB
- D: ❌ Traffic stops

**Câu 34: B**
- A: ❌ Single AZ is not resilient
- **B: ✅ Minimum 2 AZs for high availability**
- C: ❌ 3 is better but not minimum
- D: ❌ 4 is overkill

**Câu 35: C**
- A: ❌ Simple routing doesn't provide failover
- B: ❌ Weighted routing is for traffic distribution
- **C: ✅ Failover routing provides automatic failover**
- D: ❌ Latency-based is for performance

**Câu 36: D**
- A: ❌ Streams capture changes
- B: ❌ DAX is caching
- C: ❌ Global Tables are for multi-region
- **D: ✅ PITR provides point-in-time recovery**

**Câu 37: B**
- A: ❌ In-place has downtime
- **B: ✅ Blue/Green provides zero-downtime**
- C: ❌ Rolling has temporary reduced capacity
- D: ❌ Big bang has downtime

### DOMAIN 3: HIGH-PERFORMING ARCHITECTURES

**Câu 38: B**
- A: ❌ Larger instances don't reduce latency
- **B: ✅ CloudFront caches content at edge locations**
- C: ❌ Auto Scaling doesn't reduce latency
- D: ❌ ELB doesn't reduce global latency

**Câu 39: C**
- A: ❌ Larger instance helps but doesn't scale reads independently
- B: ❌ Multi-AZ is for HA, not read performance
- **C: ✅ Read Replicas offload read traffic**
- D: ❌ Redshift is for analytics, not OLTP

**Câu 40: B**
- A: ❌ S3 is storage, not cache
- **B: ✅ ElastiCache is in-memory caching**
- C: ❌ EBS is block storage
- D: ❌ EFS is file storage

**Câu 41: B**
- A: ❌ S3 is storage, not real-time
- **B: ✅ Kinesis Data Streams for real-time**
- C: ❌ Batch is for batch processing
- D: ❌ SQS has higher latency

**Câu 42: C**
- A: ❌ RDS has millisecond latency
- B: ❌ DynamoDB has single-digit millisecond
- **C: ✅ DAX provides microsecond latency**
- D: ❌ Aurora has millisecond latency

**Câu 43: C**
- A: ❌ Spread has higher latency
- B: ❌ Partition has higher latency
- **C: ✅ Cluster provides lowest latency**
- D: ❌ Default has higher latency

**Câu 44: B**
- A: ❌ Multiple EC2 regions is expensive
- **B: ✅ CloudFront with S3 is cost-effective**
- C: ❌ ELB doesn't work across regions
- D: ❌ Global Accelerator is for TCP/UDP

**Câu 45: B**
- A: ❌ gp3 max is 16,000 IOPS
- **B: ✅ io2 provides > 20,000 IOPS**
- C: ❌ st1 is throughput-optimized, not IOPS
- D: ❌ sc1 is cold storage

**Câu 46: C**
- A: ❌ RDS is for OLTP
- B: ❌ DynamoDB is NoSQL
- **C: ✅ Redshift is for analytics (OLAP)**
- D: ❌ ElastiCache is caching

**Câu 47: B**
- A: ❌ Transfer Acceleration speeds uploads but multipart is better for large files
- **B: ✅ Multipart Upload for files > 5 GB**
- C: ❌ Versioning is for protection
- D: ❌ Lifecycle is for cost optimization

**Câu 48: B**
- A: ❌ Reserved Instances don't auto-scale
- **B: ✅ Auto Scaling with target tracking adjusts automatically**
- C: ❌ Spot Instances can be interrupted
- D: ❌ Manual is not automatic

**Câu 49: B**
- A: ❌ CloudFront is for HTTP/HTTPS
- **B: ✅ Global Accelerator for TCP/UDP**
- C: ❌ ELB is regional
- D: ❌ Route 53 is DNS

**Câu 50: C**
- A: ❌ RDS is for OLTP
- B: ❌ DynamoDB is NoSQL
- **C: ✅ Redshift for petabyte-scale analytics**
- D: ❌ Aurora is for OLTP

**Câu 51: B**
- A: ❌ Standard Queue doesn't guarantee order
- **B: ✅ FIFO Queue guarantees order**
- C: ❌ SNS is pub/sub
- D: ❌ Firehose is for delivery

**Câu 52: B**
- A: ❌ CloudFront caches content, not APIs
- **B: ✅ API Gateway has built-in caching**
- C: ❌ ELB doesn't cache
- D: ❌ ElastiCache requires custom integration

**Câu 53: B**
- A: ❌ RDS has higher latency
- **B: ✅ DynamoDB provides single-digit millisecond**
- C: ❌ Redshift is for analytics
- D: ❌ Aurora has higher latency than DynamoDB

### DOMAIN 4: COST-OPTIMIZED ARCHITECTURES

**Câu 54: C**
- A: ❌ On-Demand is most expensive
- B: ❌ Spot can be interrupted
- **C: ✅ 3-year Reserved Instances save up to 72%**
- D: ❌ Savings Plans save up to 72% but less flexible

**Câu 55: C**
- A: ❌ Standard is expensive for infrequent access
- B: ❌ Intelligent-Tiering has monitoring cost
- **C: ✅ Standard-IA is cost-effective for infrequent + instant retrieval**
- D: ❌ Glacier has retrieval time (not milliseconds)

**Câu 56: A**
- **A: ✅ On-Demand for unpredictable workloads**
- B: ❌ Reserved requires commitment
- C: ❌ Spot can be interrupted
- D: ❌ Dedicated Hosts are expensive

**Câu 57: B**
- A: ❌ CloudFront is for global delivery
- **B: ✅ VPC Endpoints eliminate data transfer costs**
- C: ❌ Direct Connect is for on-premises
- D: ❌ NAT Gateway has costs

**Câu 58: B**
- A: ❌ Versioning is for protection
- **B: ✅ Lifecycle policies automate tiering**
- C: ❌ Replication is for DR
- D: ❌ Object Lock is for compliance

**Câu 59: A**
- **A: ✅ On-Demand for part-time usage**
- B: ❌ Reserved requires 24/7 usage
- C: ❌ Spot can be interrupted
- D: ❌ Dedicated is expensive

**Câu 60: D**
- A: ❌ Standard-IA is for monthly access
- B: ❌ One Zone-IA is for monthly access
- C: ❌ Glacier Flexible is for annual access
- **D: ✅ Glacier Deep Archive is cheapest for rare access**

**Câu 61: C**
- A: ❌ Reserved requires commitment
- B: ❌ On-Demand is expensive for continuous usage
- **C: ✅ Aurora Serverless scales to zero, most cost-effective**
- D: ❌ Provisioned requires capacity planning

**Câu 62: C**
- A: ❌ On-Demand is expensive
- B: ❌ Reserved requires commitment
- **C: ✅ Spot Instances save up to 90%**
- D: ❌ Savings Plans require commitment

**Câu 63: C**
- A: ❌ Multiple NAT Gateways increase cost
- B: ❌ NAT Instances require management
- **C: ✅ VPC Endpoints eliminate NAT Gateway costs where possible**
- D: ❌ Internet Gateway doesn't work for private subnets

**Câu 64: C**
- A: ❌ Cost Explorer shows costs, not optimization
- B: ❌ Budgets set alerts
- **C: ✅ Trusted Advisor provides cost optimization recommendations**
- D: ❌ Logs Insights analyzes logs

**Câu 65: B**
- A: ❌ gp3 is general purpose, not cheapest
- **B: ✅ sc1 (Cold HDD) is cheapest for infrequent access**
- C: ❌ Snapshots + deletion is good but sc1 is better for active volumes
- D: ❌ io2 is most expensive

---

## 📊 Phân Tích Điểm

### Tính Điểm
```
Điểm = (Số câu đúng ÷ 65) × 1000

Ví dụ:
- 47 câu đúng: (47 ÷ 65) × 1000 = 723 điểm (ĐỖ)
- 46 câu đúng: (46 ÷ 65) × 1000 = 708 điểm (TRƯỢT)
```

### Mục Tiêu Theo Domain
- Domain 1 (Security): 16-18/20 đúng (80-90%)
- Domain 2 (Resilient): 14-15/17 đúng (82-88%)
- Domain 3 (Performance): 13-14/16 đúng (81-88%)
- Domain 4 (Cost): 10-12/12 đúng (83-100%)

**Tổng:** 53-59/65 đúng (82-91%) = 815-908 điểm

---

## 💡 Tips Sau Khi Làm Bài

1. **Review sai:** Xem lại tất cả câu sai, hiểu tại sao
2. **Identify patterns:** Nhận diện các patterns trong câu hỏi
3. **Practice more:** Làm thêm đề thi thử khác
4. **Focus on weak areas:** Tập trung vào domain yếu
5. **Time management:** Luyện làm trong 130 phút

---

*Chúc bạn làm bài tốt và đạt chứng chỉ AWS SAA-C03! 🎉*
