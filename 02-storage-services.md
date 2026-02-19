# 💾 STORAGE SERVICES - AWS SAA-C03

## 📌 Tổng Quan

Storage là **NỀN TẢNG** của mọi kiến trúc AWS, chiếm **15-20% câu hỏi** trong kỳ thi. Phải hiểu rõ đặc điểm và use case của từng loại.

---

## 1️⃣ AMAZON S3 (Simple Storage Service)

### 🎯 Khái Niệm Cơ Bản

**S3** = Object storage service, lưu trữ unlimited data dưới dạng objects trong buckets.

### 📊 S3 Basics

- **Object**: File + metadata
- **Bucket**: Container for objects
- **Key**: Object name (full path)
- **Max object size**: 5 TB
- **Multipart upload**: Recommended for >100 MB, required for >5 GB

### 🌐 S3 Naming & Structure

```
s3://bucket-name/folder1/folder2/file.txt
         ↓           ↓
      Bucket      Key (path)
```

**Bucket naming rules**:
- Globally unique
- 3-63 characters
- Lowercase, numbers, hyphens
- No IP format

---

## 2️⃣ S3 STORAGE CLASSES

### 📦 Storage Classes Comparison

| Class | Durability | Availability | AZs | Use Case | Cost |
|-------|------------|--------------|-----|----------|------|
| **S3 Standard** | 11 9's | 99.99% | ≥3 | Frequently accessed | $$$$$ |
| **S3 Intelligent-Tiering** | 11 9's | 99.9% | ≥3 | Unknown/changing access | $$$$ |
| **S3 Standard-IA** | 11 9's | 99.9% | ≥3 | Infrequent access | $$$ |
| **S3 One Zone-IA** | 11 9's | 99.5% | 1 | Infrequent, non-critical | $$ |
| **S3 Glacier Instant Retrieval** | 11 9's | 99.9% | ≥3 | Archive, instant access | $$ |
| **S3 Glacier Flexible Retrieval** | 11 9's | 99.99% | ≥3 | Archive, minutes-hours | $ |
| **S3 Glacier Deep Archive** | 11 9's | 99.99% | ≥3 | Long-term archive | $ |

### 🔄 Retrieval Times & Costs

#### S3 Glacier Flexible Retrieval
- **Expedited**: 1-5 minutes (đắt nhất)
- **Standard**: 3-5 hours
- **Bulk**: 5-12 hours (rẻ nhất)

#### S3 Glacier Deep Archive
- **Standard**: 12 hours
- **Bulk**: 48 hours

### 🎯 Storage Class Selection

```
Frequently accessed → S3 Standard
Unknown pattern → S3 Intelligent-Tiering
Infrequent but fast access → S3 Standard-IA
Infrequent, non-critical → S3 One Zone-IA
Archive, instant retrieval → Glacier Instant Retrieval
Archive, hours retrieval → Glacier Flexible Retrieval
Long-term archive → Glacier Deep Archive
```

**🎯 Exam Tip**: 
- "Unknown access pattern" = Intelligent-Tiering
- "Infrequent access" = Standard-IA
- "Archive" + "immediate access" = Glacier Instant Retrieval
- "Long-term archive" + "lowest cost" = Glacier Deep Archive

---

## 3️⃣ S3 LIFECYCLE POLICIES

### 🔄 Lifecycle Transitions

```
S3 Standard (0 days)
    ↓ (30+ days)
S3 Standard-IA
    ↓ (30+ days)
S3 Intelligent-Tiering / One Zone-IA
    ↓ (0+ days)
Glacier Instant Retrieval
    ↓ (0+ days)
Glacier Flexible Retrieval
    ↓ (0+ days)
Glacier Deep Archive
```

### 📋 Lifecycle Rules

**Actions**:
1. **Transition actions**: Move objects between storage classes
2. **Expiration actions**: Delete objects after time period

**Example Policy**:
```
Day 0: Upload to S3 Standard
Day 30: Transition to S3 Standard-IA
Day 90: Transition to Glacier Flexible Retrieval
Day 365: Delete
```

**🎯 Exam Tip**: 
- Minimum 30 days in S3 Standard before transitioning to IA
- Lifecycle policies automate cost optimization

---

## 4️⃣ S3 VERSIONING

### 🔄 Versioning Features

- **Enable at bucket level**
- **Protects against accidental deletion**
- **Each version has unique Version ID**
- **Delete marker**: Hides object, doesn't delete

### 💡 Key Points

- Cannot disable, only suspend
- Versioning + Lifecycle = Cost optimization
- Delete permanently: Delete specific version

**🎯 Exam Tip**: 
- "Protect against accidental deletion" = Versioning
- "Rollback to previous version" = Versioning

---

## 5️⃣ S3 REPLICATION

### 🌍 Replication Types

#### 1. **Cross-Region Replication (CRR)**
- Replicate across different regions
- **Use case**: Compliance, lower latency, disaster recovery

#### 2. **Same-Region Replication (SRR)**
- Replicate within same region
- **Use case**: Log aggregation, prod/test sync

### ⚙️ Replication Requirements

- **Versioning must be enabled** on both buckets
- **IAM role** with proper permissions
- **Can replicate**:
  - All objects or subset (prefix/tags)
  - Storage class
  - Ownership
  - Encryption

### 📋 Replication Features

- **Asynchronous replication**
- **Replication Time Control (RTC)**: 15-minute SLA
- **Batch Replication**: Replicate existing objects
- **Delete markers**: Optional replication

**🎯 Exam Tip**: 
- "Disaster recovery" + "different region" = CRR
- "Compliance" + "store in multiple regions" = CRR
- Versioning required for replication

---

## 6️⃣ S3 SECURITY

### 🔒 Encryption

#### 1. **Encryption at Rest**

**Server-Side Encryption (SSE)**:
- **SSE-S3**: AWS-managed keys (AES-256)
  - Default encryption
  - No additional cost
  
- **SSE-KMS**: AWS KMS keys
  - More control
  - Audit trail (CloudTrail)
  - Có thể bị throttle (KMS limits)
  
- **SSE-C**: Customer-provided keys
  - Customer manages keys
  - HTTPS required

**Client-Side Encryption**:
- Encrypt before upload
- Customer manages encryption/decryption

#### 2. **Encryption in Transit**
- **SSL/TLS** (HTTPS)
- **Enforce**: Bucket policy with `aws:SecureTransport`

### 🛡️ Access Control

#### 1. **Bucket Policies**
- **JSON-based**
- Bucket-level permissions
- **Use case**: Cross-account access, public access

**Example**: Make bucket public
```json
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::bucket/*"
}
```

#### 2. **IAM Policies**
- User/role-level permissions
- **Use case**: Internal AWS access

#### 3. **Access Control Lists (ACLs)**
- Legacy, not recommended
- Object/bucket level

#### 4. **S3 Block Public Access**
- **Account/bucket level**
- Overrides all other settings
- **Best practice**: Keep enabled

### 🔐 Security Best Practices

```
1. Enable S3 Block Public Access (unless needed)
2. Use bucket policies for cross-account
3. Use IAM policies for AWS users
4. Enable versioning + MFA Delete
5. Encrypt sensitive data (SSE-KMS)
6. Enable CloudTrail logging
7. Use VPC Endpoints for private access
```

**🎯 Exam Tip**: 
- "Audit who accessed" = CloudTrail + SSE-KMS
- "Prevent accidental public access" = S3 Block Public Access
- "Protect from deletion" = Versioning + MFA Delete

---

## 7️⃣ S3 PERFORMANCE

### ⚡ Performance Optimization

#### 1. **Multi-Part Upload**
- **Recommended**: >100 MB
- **Required**: >5 GB
- Parallel uploads
- Faster, resilient

#### 2. **Transfer Acceleration**
- Upload via CloudFront edge locations
- **Use case**: Global uploads, large files
- URL: `bucket.s3-accelerate.amazonaws.com`

#### 3. **Byte-Range Fetches**
- Parallel downloads
- Download partial file
- **Use case**: Large files, resilience

#### 4. **S3 Select**
- Query data with SQL
- **Retrieve subset** of data
- Up to 400% faster, 80% cheaper

### 📊 S3 Performance Limits

- **3,500 PUT/COPY/POST/DELETE** per second per prefix
- **5,500 GET/HEAD** per second per prefix
- No limit on prefixes

**Prefix example**:
```
bucket/folder1/file1.txt → prefix: /folder1/
bucket/folder2/file2.txt → prefix: /folder2/
```

**🎯 Exam Tip**: 
- "Upload large files faster" = Multi-Part Upload + Transfer Acceleration
- "Download faster" = Byte-Range Fetches
- "Query data in S3" = S3 Select (or Athena)

---

## 8️⃣ S3 ADVANCED FEATURES

### 🌐 S3 Static Website Hosting

- Host static websites (HTML, CSS, JS)
- **URL format**: `bucket-name.s3-website-region.amazonaws.com`
- **Error document**: Custom 404 page
- **Use case**: Single-page apps, documentation

**Requirements**:
- Bucket name = domain name (for custom domain)
- Bucket must be public
- Enable static website hosting

### 🔔 S3 Event Notifications

**Trigger events**:
- Object created/removed/restored
- Replication events

**Destinations**:
- **SNS** topic
- **SQS** queue
- **Lambda** function
- **EventBridge** (more advanced)

**🎯 Exam Tip**: "Process files uploaded to S3" = S3 Event + Lambda

### 🏷️ S3 Object Tags

- Key-value pairs (max 10 per object)
- **Use case**: Lifecycle rules, IAM policies, analytics

### 📊 S3 Analytics

- **Storage Class Analysis**: Recommend when to transition
- **Data export**: Daily reports to S3

### 🔍 S3 Inventory

- List all objects and metadata
- **Format**: CSV, ORC, Parquet
- **Use case**: Audit, compliance

### 🔒 S3 Object Lock

- **WORM** (Write Once Read Many)
- **Modes**:
  - **Governance**: Can be overridden by admin
  - **Compliance**: Cannot be deleted (even by root)
- **Retention periods**
- **Legal hold**: Indefinite retention

**🎯 Exam Tip**: 
- "Regulatory compliance" + "cannot delete" = S3 Object Lock (Compliance)
- "WORM" = S3 Object Lock

### 🛡️ S3 Access Points

- Simplify access management
- Each access point has own policy
- **Use case**: Different teams, different permissions

### 🌐 S3 Multi-Region Access Points

- Single global endpoint
- Route to nearest S3 bucket
- **Use case**: Global applications, low latency

---

## 9️⃣ AMAZON EBS (Elastic Block Store)

### 💾 Khái Niệm

**EBS** = Block storage for EC2 instances. Network-attached, persistent.

### 📊 EBS Volume Types

| Type | Name | IOPS | Throughput | Use Case | Cost |
|------|------|------|------------|----------|------|
| **gp3** | General Purpose SSD | 3,000-16,000 | 125-1,000 MB/s | Balanced | $$ |
| **gp2** | General Purpose SSD | 3-16,000 | 128-250 MB/s | Balanced | $$ |
| **io2 Block Express** | Provisioned IOPS SSD | 256,000 | 4,000 MB/s | Critical apps | $$$$ |
| **io2** | Provisioned IOPS SSD | 64,000 | 1,000 MB/s | Databases | $$$ |
| **io1** | Provisioned IOPS SSD | 64,000 | 1,000 MB/s | Databases | $$$ |
| **st1** | Throughput Optimized HDD | 500 | 500 MB/s | Big data | $ |
| **sc1** | Cold HDD | 250 | 250 MB/s | Infrequent access | $ |

### 🎯 Volume Type Selection

```
Boot volume → gp3/gp2
General purpose → gp3 (newer, better)
High IOPS, low latency → io2/io1
Big data, sequential → st1
Cold data, infrequent → sc1
```

**🎯 Exam Tip**: 
- "High IOPS" + "database" = io2/io1
- "Cost-effective" + "balanced" = gp3
- "Throughput" + "big data" = st1
- Only gp and io can be boot volumes

### 🔄 EBS Features

#### 1. **EBS Snapshots**
- **Backup** of EBS volume to S3
- **Incremental**: Only changed blocks
- **Can copy** across regions
- **Create volume** from snapshot

**Snapshot Features**:
- **EBS Snapshot Archive**: 75% cheaper, 24-72h restore
- **Recycle Bin**: Protect against accidental deletion
- **Fast Snapshot Restore (FSR)**: No latency on first use (expensive)

#### 2. **EBS Multi-Attach** (io1/io2 only)
- Attach same volume to multiple EC2 instances
- **Same AZ only**
- **Use case**: Clustered applications
- Max 16 instances

#### 3. **EBS Encryption**
- **At rest**: AES-256
- **In transit**: Between instance and volume
- **Snapshots**: Encrypted if volume encrypted
- **Minimal performance impact**

**Encrypt unencrypted volume**:
1. Create snapshot
2. Copy snapshot (enable encryption)
3. Create volume from encrypted snapshot

### 📍 EBS Characteristics

- **Locked to AZ**: Cannot attach to instance in different AZ
- **Network drive**: Slight latency
- **Persistent**: Data survives instance termination (if DeleteOnTermination = false)
- **Can detach/attach** to different instance

**🎯 Exam Tip**: 
- "Persistent storage" = EBS
- "Backup" = EBS Snapshot
- "Move to different AZ" = Snapshot → Create volume in new AZ
- "Encrypt existing volume" = Snapshot → Copy with encryption

---

## 🔟 AMAZON EFS (Elastic File System)

### 📁 Khái Niệm

**EFS** = Managed NFS (Network File System), shared file storage.

### 🎯 Key Features

- **Multi-AZ**: High availability
- **Shared**: Multiple EC2 instances can mount
- **Auto-scaling**: Grows/shrinks automatically
- **Linux only** (not Windows)
- **Pay per use**: No capacity planning

### 📊 EFS Storage Classes

#### 1. **EFS Standard**
- Frequently accessed
- Multi-AZ

#### 2. **EFS Standard-IA**
- Infrequent access
- Lower cost
- Multi-AZ

#### 3. **EFS One Zone**
- Single AZ
- 90% cost savings
- Good for dev/test

#### 4. **EFS One Zone-IA**
- Single AZ + Infrequent access
- Lowest cost

### ⚙️ EFS Performance Modes

#### 1. **General Purpose** (default)
- Low latency
- **Use case**: Web servers, CMS

#### 2. **Max I/O**
- Higher latency, higher throughput
- **Use case**: Big data, media processing

### 📈 EFS Throughput Modes

#### 1. **Bursting**
- Throughput scales with size
- 50 MB/s per TB

#### 2. **Provisioned**
- Set throughput regardless of size
- **Use case**: Small files, high throughput

#### 3. **Elastic** (recommended)
- Automatically scales
- Pay for what you use

### 🔒 EFS Security

- **Encryption at rest**: KMS
- **Encryption in transit**: TLS
- **Access control**: Security groups, IAM, EFS Access Points

### 🔄 EFS vs EBS vs Instance Store

| Feature | EBS | EFS | Instance Store |
|---------|-----|-----|----------------|
| **Type** | Block | File | Block |
| **Attachment** | Single instance (except io1/io2 Multi-Attach) | Multiple instances | Single instance |
| **AZ** | Single AZ | Multi-AZ | Single AZ |
| **Persistence** | Yes | Yes | No (ephemeral) |
| **Use case** | Boot volumes, databases | Shared files, content management | Cache, temporary |

**🎯 Exam Tip**: 
- "Shared file system" + "Multiple instances" = EFS
- "Linux" + "NFS" = EFS
- "Single instance" + "persistent" = EBS
- "Temporary" + "high IOPS" = Instance Store

---

## 1️⃣1️⃣ AWS STORAGE GATEWAY

### 🌉 Khái Niệm

**Storage Gateway** = Hybrid cloud storage, bridge on-premises and AWS.

### 🎯 Storage Gateway Types

#### 1. **File Gateway (S3 File Gateway)**
- **Protocol**: NFS, SMB
- **Backend**: S3
- **Use case**: File shares, backup
- **Cache**: Local cache for frequently accessed

#### 2. **Volume Gateway**

**a) Stored Volumes**
- **Primary data**: On-premises
- **Backup**: Async to S3 (EBS snapshots)
- **Use case**: Low latency, full dataset on-premises

**b) Cached Volumes**
- **Primary data**: S3
- **Cache**: Frequently accessed on-premises
- **Use case**: Minimize on-premises storage

#### 3. **Tape Gateway (VTL)**
- **Virtual Tape Library**
- **Backend**: S3, Glacier
- **Use case**: Backup applications (Veeam, NetBackup)

### 🔄 Storage Gateway Selection

```
File shares → File Gateway
Block storage, full copy on-prem → Stored Volumes
Block storage, minimize on-prem → Cached Volumes
Tape backups → Tape Gateway
```

**🎯 Exam Tip**: 
- "On-premises" + "S3" = Storage Gateway
- "File shares" = File Gateway
- "Backup tapes" = Tape Gateway
- "Low latency" + "full dataset" = Stored Volumes

---

## 1️⃣2️⃣ AWS SNOW FAMILY

### ❄️ Khái Niệm

**Snow Family** = Physical devices for data migration and edge computing.

### 📦 Snow Devices

| Device | Storage | Use Case | Transfer Time (100 TB) |
|--------|---------|----------|------------------------|
| **Snowcone** | 8-14 TB | Small, portable | Weeks |
| **Snowball Edge Storage Optimized** | 80 TB | Large migrations | Days |
| **Snowball Edge Compute Optimized** | 42 TB + compute | Edge computing | Days |
| **Snowmobile** | 100 PB | Exabyte-scale | Weeks |

### 🎯 Snow Family Features

#### **Snowcone**
- Smallest, portable
- USB-C power
- **DataSync agent** pre-installed
- **Use case**: Edge locations, drones

#### **Snowball Edge**
- **Storage Optimized**: 80 TB, 40 vCPUs
- **Compute Optimized**: 42 TB, 52 vCPUs, optional GPU
- **Use case**: Large migrations, edge computing

#### **Snowmobile**
- Shipping container on truck
- **100 PB** capacity
- **Use case**: Data center migration

### 🔄 When to Use Snow Family

```
Network bandwidth calculation:
Time = Data / Bandwidth

Example: 100 TB over 1 Gbps
= 100 TB / 1 Gbps = 100 days

If > 1 week → Consider Snow Family
```

**🎯 Exam Tip**: 
- "Limited bandwidth" + "large data" = Snow Family
- "Petabytes" = Snowball or Snowmobile
- "Edge computing" = Snowball Edge
- "Data center migration" = Snowmobile

---

## 1️⃣3️⃣ AWS FSx

### 🗄️ Khái Niệm

**FSx** = Managed third-party file systems.

### 🎯 FSx Types

#### 1. **FSx for Windows File Server**
- **Protocol**: SMB
- **Features**: Active Directory integration, DFS
- **Use case**: Windows applications, SharePoint
- **Multi-AZ**: High availability

#### 2. **FSx for Lustre**
- **High-performance** file system
- **Use case**: HPC, ML, video processing
- **Integration**: S3 (read/write)
- **Throughput**: 100s GB/s, millions IOPS

**Deployment Options**:
- **Scratch**: Temporary, no replication, high performance
- **Persistent**: Long-term, replicated, HA

#### 3. **FSx for NetApp ONTAP**
- NetApp ONTAP on AWS
- **Protocol**: NFS, SMB, iSCSI
- **Features**: Snapshots, cloning, compression
- **Use case**: Enterprise applications

#### 4. **FSx for OpenZFS**
- OpenZFS file system
- **Use case**: ZFS migrations
- **Performance**: Up to 1M IOPS

### 🔄 FSx Selection

```
Windows applications → FSx for Windows
HPC, ML, video processing → FSx for Lustre
NetApp users → FSx for NetApp ONTAP
ZFS migrations → FSx for OpenZFS
```

**🎯 Exam Tip**: 
- "Windows" + "SMB" = FSx for Windows
- "HPC" + "high performance" = FSx for Lustre
- "S3 integration" + "compute" = FSx for Lustre

---

## 1️⃣4️⃣ AWS DATASYNC

### 🔄 Khái Niệm

**DataSync** = Automated data transfer service.

### 🎯 Key Features

- **Agent-based**: Install agent on-premises
- **Destinations**: S3, EFS, FSx
- **Bandwidth**: Can throttle
- **Scheduling**: Hourly, daily, weekly
- **Encryption**: In transit and at rest
- **Verification**: Integrity checking

### 🔄 DataSync vs Snow Family

```
DataSync:
- Network-based
- Continuous sync
- Bandwidth available

Snow Family:
- Physical device
- One-time migration
- Limited bandwidth
```

**🎯 Exam Tip**: 
- "Automated sync" + "on-premises to AWS" = DataSync
- "Limited bandwidth" = Snow Family
- "Continuous replication" = DataSync

---

## 📋 STORAGE SERVICES COMPARISON

| Service | Type | Persistence | Sharing | Use Case |
|---------|------|-------------|---------|----------|
| **S3** | Object | Yes | Public/Private | Static files, backups, archives |
| **EBS** | Block | Yes | Single instance | Boot volumes, databases |
| **EFS** | File | Yes | Multiple instances | Shared file system |
| **Instance Store** | Block | No | Single instance | Cache, temporary |
| **Storage Gateway** | Hybrid | Yes | On-prem + Cloud | Hybrid storage |
| **FSx** | File | Yes | Multiple | Windows, HPC, NetApp |
| **Snow Family** | Physical | Yes | Migration | Large data migration |

---

## 🎯 EXAM SCENARIOS - STORAGE

### Scenario 1: Cost Optimization
**Q**: "Store logs, accessed frequently for 30 days, then rarely. Lowest cost?"

**A**: S3 Standard (30 days) → Lifecycle to S3 Standard-IA → Glacier Deep Archive

---

### Scenario 2: Disaster Recovery
**Q**: "Replicate S3 data to another region for DR?"

**A**: S3 Cross-Region Replication (CRR) with Versioning

---

### Scenario 3: Shared Storage
**Q**: "Multiple EC2 instances need shared file system?"

**A**: Amazon EFS (for Linux) or FSx for Windows (for Windows)

---

### Scenario 4: High IOPS Database
**Q**: "Database requires 50,000 IOPS?"

**A**: EBS io2/io1 Provisioned IOPS SSD

---

### Scenario 5: Hybrid Storage
**Q**: "On-premises applications need access to S3?"

**A**: AWS Storage Gateway (File Gateway)

---

### Scenario 6: Large Data Migration
**Q**: "Migrate 500 TB, limited bandwidth?"

**A**: AWS Snowball Edge

---

## ⚠️ COMMON MISTAKES

1. ❌ **Dùng S3 Standard cho archive data** → Dùng Glacier
2. ❌ **Quên enable Versioning trước khi setup Replication** → Required
3. ❌ **Dùng EBS cho shared storage** → Dùng EFS (except Multi-Attach)
4. ❌ **Không optimize S3 costs với Lifecycle** → Automate transitions
5. ❌ **Chọn wrong EBS type** → gp3 for general, io2 for high IOPS, st1 for throughput

---

## 🎓 TIPS ĐẶC BIỆT

### 1. **S3 Decision Tree**

```
Need storage?
├─ Object storage? → S3
│  ├─ Frequent access? → Standard
│  ├─ Unknown pattern? → Intelligent-Tiering
│  ├─ Infrequent? → Standard-IA
│  └─ Archive? → Glacier
├─ Block storage?
│  ├─ Shared? → EFS (Linux) / FSx (Windows)
│  ├─ Persistent? → EBS
│  └─ Temporary? → Instance Store
└─ Hybrid? → Storage Gateway
```

### 2. **Cost Optimization Keywords**

```
"Lowest cost" + "archive" → Glacier Deep Archive
"Cost-effective" + "unknown pattern" → Intelligent-Tiering
"Infrequent access" → Standard-IA / One Zone-IA
"Lifecycle" → Automatic transitions
```

### 3. **Performance Keywords**

```
"High IOPS" → io2/io1 EBS
"Throughput" → st1 EBS or FSx Lustre
"Shared" + "high performance" → FSx Lustre
"Low latency" → EBS or Instance Store
```

---

## 📚 CHECKLIST

- [ ] Hiểu rõ 7 S3 storage classes và use cases
- [ ] Biết S3 Lifecycle transitions (minimum days)
- [ ] Nắm vững S3 Replication (CRR vs SRR)
- [ ] Phân biệt 4 EBS volume types (gp3, io2, st1, sc1)
- [ ] Hiểu EFS vs EBS vs Instance Store
- [ ] Biết Storage Gateway types (File, Volume, Tape)
- [ ] Nắm Snow Family (Snowcone, Snowball, Snowmobile)
- [ ] Phân biệt FSx types (Windows, Lustre, ONTAP, OpenZFS)

---

**Next**: [03-database-services.md](03-database-services.md) - Databases là phần quan trọng tiếp theo! 🚀
