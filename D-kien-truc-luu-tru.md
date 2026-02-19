# 💾 D. KIẾN TRÚC LƯU TRỮ

> **Mục tiêu:** Thiết kế storage solutions cho các use cases khác nhau

## 📋 Storage Decision Tree

```
Cần lưu trữ gì?
│
├─ Object Storage (files, images, videos)
│  ├─ Frequently accessed → S3 Standard
│  ├─ Infrequent access → S3 Standard-IA / One Zone-IA
│  ├─ Archive → S3 Glacier / Glacier Deep Archive
│  └─ Unknown pattern → S3 Intelligent-Tiering
│
├─ Block Storage (EC2 volumes)
│  ├─ High IOPS → io2 Block Express
│  ├─ Balanced → gp3 (general purpose SSD)
│  ├─ Throughput-optimized → st1 (HDD)
│  └─ Cold data → sc1 (HDD)
│
├─ File Storage (shared access)
│  ├─ Linux/POSIX → EFS
│  ├─ Windows → FSx for Windows File Server
│  ├─ Lustre (HPC) → FSx for Lustre
│  └─ NetApp/OpenZFS → FSx variants
│
└─ Hybrid Storage
   ├─ On-prem to cloud → Storage Gateway
   ├─ Data transfer → DataSync, Snow Family
   └─ Edge computing → AWS Outposts
```

---

## 🪣 Amazon S3

### Storage Classes Comparison

| Class | Availability | AZs | Min Duration | Retrieval | Use Case | Cost |
|-------|--------------|-----|--------------|-----------|----------|------|
| **Standard** | 99.99% | ≥3 | None | Instant | Frequently accessed | $$$$ |
| **Intelligent-Tiering** | 99.9% | ≥3 | None | Instant | Unknown/changing patterns | $$$ |
| **Standard-IA** | 99.9% | ≥3 | 30 days | Instant | Infrequent access | $$ |
| **One Zone-IA** | 99.5% | 1 | 30 days | Instant | Non-critical, infrequent | $ |
| **Glacier Instant Retrieval** | 99.9% | ≥3 | 90 days | Instant | Archive, quarterly access | $ |
| **Glacier Flexible Retrieval** | 99.99% | ≥3 | 90 days | Min-hours | Archive, annual access | $ |
| **Glacier Deep Archive** | 99.99% | ≥3 | 180 days | 12 hours | Long-term archive | Cheapest |

### S3 Lifecycle Policies

**Ví dụ Policy:**
```xml
<LifecycleConfiguration>
  <Rule>
    <Status>Enabled</Status>
    <Transition>
      <Days>30</Days>
      <StorageClass>STANDARD_IA</StorageClass>
    </Transition>
    <Transition>
      <Days>90</Days>
      <StorageClass>GLACIER</StorageClass>
    </Transition>
    <Expiration>
      <Days>365</Days>
    </Expiration>
  </Rule>
</LifecycleConfiguration>
```

**Timeline:**
```
Day 0: Upload → S3 Standard
Day 30: Auto-move → S3 Standard-IA
Day 90: Auto-move → Glacier
Day 365: Auto-delete
```

### S3 Replication

| Feature | CRR (Cross-Region) | SRR (Same-Region) |
|---------|-------------------|-------------------|
| **Use Case** | DR, compliance, latency | Log aggregation, compliance |
| **Versioning** | Required | Required |
| **Encryption** | Supported | Supported |
| **Delete markers** | Optional | Optional |

**Setup Requirements:**
1. ✅ Enable versioning on source & destination
2. ✅ Create IAM role with replication permissions
3. ✅ Configure replication rule

**Ví dụ trong đề thi:**
> **Q:** S3 bucket ở us-east-1 cần replicate sang eu-west-1 for disaster recovery. Giải pháp nào?
> 
> ✅ **S3 Cross-Region Replication (CRR)**

### S3 Performance Optimization

**Multipart Upload:**
- ✅ Recommended for files > 100 MB
- ✅ Required for files > 5 GB
- ✅ Parallel uploads (faster)
- ✅ Resume failed uploads

**Transfer Acceleration:**
- ✅ Use CloudFront edge locations
- ✅ Up to 50-500% faster uploads
- ✅ Cost: $0.04-$0.08 per GB

**S3 Select:**
- ✅ Retrieve subset of data using SQL
- ✅ Up to 400% faster, 80% cheaper
- ✅ Works with CSV, JSON, Parquet

**Request Rate:**
- 3,500 PUT/COPY/POST/DELETE per second per prefix
- 5,500 GET/HEAD per second per prefix
- Use multiple prefixes for higher throughput

---

## 💿 Amazon EBS

### EBS Volume Types

| Type | Max IOPS | Max Throughput | Use Case | Cost |
|------|----------|----------------|----------|------|
| **gp3** (SSD) | 16,000 | 1,000 MB/s | General purpose | $$ |
| **gp2** (SSD) | 16,000 | 250 MB/s | Legacy general purpose | $$ |
| **io2 Block Express** | 256,000 | 4,000 MB/s | Mission-critical, databases | $$$$ |
| **io2** (SSD) | 64,000 | 1,000 MB/s | High-performance databases | $$$ |
| **st1** (HDD) | 500 | 500 MB/s | Big data, data warehouses | $ |
| **sc1** (HDD) | 250 | 250 MB/s | Cold data, infrequent access | Cheapest |

**Decision Tree:**
```
Workload type?
│
├─ Transactional (databases, boot volumes)
│  ├─ Need > 16,000 IOPS → io2/io2 Block Express
│  └─ General use → gp3
│
└─ Throughput-optimized (big data, logs)
   ├─ Frequently accessed → st1
   └─ Infrequently accessed → sc1
```

**Ví dụ trong đề thi:**
> **Q:** Database cần 50,000 IOPS. EBS volume type nào phù hợp?
> 
> ✅ **io2** (up to 64,000 IOPS)

### EBS Features

**Snapshots:**
- ✅ Incremental backups
- ✅ Stored in S3 (managed by AWS)
- ✅ Can copy across regions
- ✅ Can create AMI from snapshot

**Encryption:**
- ✅ Encryption at rest using KMS
- ✅ Encryption in transit (instance ↔ volume)
- ✅ Snapshots of encrypted volumes are encrypted
- ✅ Cannot change encryption after creation

**Multi-Attach (io2 only):**
- ✅ Attach single volume to multiple EC2 instances
- ✅ Same AZ only
- ✅ Use case: Clustered applications

---

## 📁 Amazon EFS

### EFS Overview

**Characteristics:**
- ✅ Fully managed NFS (Network File System)
- ✅ Multi-AZ by default (high availability)
- ✅ Auto-scaling (no capacity planning)
- ✅ Pay for what you use
- ✅ Linux only (POSIX-compliant)

### EFS Performance Modes

| Mode | Latency | Throughput | Use Case |
|------|---------|------------|----------|
| **General Purpose** | Low | Up to 7,000 ops/sec | Web serving, CMS |
| **Max I/O** | Higher | > 7,000 ops/sec | Big data, media processing |

### EFS Throughput Modes

| Mode | Throughput | Cost | Use Case |
|------|------------|------|----------|
| **Bursting** | Scales with size | Lower | Variable workloads |
| **Provisioned** | Fixed, independent of size | Higher | Consistent high throughput |
| **Elastic** | Auto-scales | Highest | Unpredictable workloads |

### EFS Storage Classes

| Class | Availability | Cost | Use Case |
|-------|--------------|------|----------|
| **Standard** | Multi-AZ | $$$ | Frequently accessed |
| **Infrequent Access (IA)** | Multi-AZ | $ | Infrequent access |
| **One Zone** | Single AZ | $$ | Non-critical |
| **One Zone-IA** | Single AZ | Cheapest | Non-critical, infrequent |

**Lifecycle Policy:**
```
Files not accessed for 7/14/30/60/90 days
→ Auto-move to IA storage class
```

**Ví dụ trong đề thi:**
> **Q:** Multiple EC2 instances cần shared file storage với auto-scaling. Giải pháp nào?
> 
> ✅ **Amazon EFS** (shared, auto-scaling, multi-AZ)

---

## 🪟 Amazon FSx

### FSx Variants

| Variant | Protocol | Use Case | Performance |
|---------|----------|----------|-------------|
| **FSx for Windows** | SMB | Windows apps, Active Directory | Up to 2 GB/s |
| **FSx for Lustre** | POSIX | HPC, ML, video processing | Up to 1 TB/s |
| **FSx for NetApp ONTAP** | NFS, SMB | Enterprise, multi-protocol | Up to 2 GB/s |
| **FSx for OpenZFS** | NFS | Linux, snapshots | Up to 1 GB/s |

**Decision:**
```
Need Windows file server? → FSx for Windows
Need HPC/ML performance? → FSx for Lustre
Need enterprise features? → FSx for NetApp ONTAP
Need Linux with snapshots? → FSx for OpenZFS
```

**Ví dụ trong đề thi:**
> **Q:** Windows application cần shared storage với Active Directory integration. Giải pháp nào?
> 
> ✅ **FSx for Windows File Server**

---

## 🌉 Hybrid Storage

### AWS Storage Gateway

**Types:**

| Type | Protocol | Use Case | Cache |
|------|----------|----------|-------|
| **File Gateway** | NFS, SMB | File shares backed by S3 | Local cache |
| **Volume Gateway** | iSCSI | Block storage backed by S3 | Stored/Cached |
| **Tape Gateway** | iSCSI VTL | Backup with virtual tapes | Tape library |

**Architecture:**
```
On-Premises
│
└─ Storage Gateway (VM/Hardware)
   │
   └─ AWS Direct Connect / VPN
      │
      └─ S3 / Glacier
```

### AWS DataSync

**Purpose:** Automate data transfer between on-premises and AWS

**Features:**
- ✅ Up to 10x faster than open-source tools
- ✅ Bandwidth throttling
- ✅ Data validation
- ✅ Encryption in transit

**Destinations:**
- S3
- EFS
- FSx

**Ví dụ trong đề thi:**
> **Q:** Migrate 50 TB data from on-premises NAS to S3. Cách nào fastest?
> 
> ✅ **AWS DataSync** (10x faster, automated)

### Snow Family

| Device | Capacity | Use Case | Transfer Time |
|--------|----------|----------|---------------|
| **Snowcone** | 8 TB | Edge computing, small transfers | Days |
| **Snowball Edge** | 80 TB | Large data migrations | Weeks |
| **Snowmobile** | 100 PB | Datacenter migration | Months |

**Decision:**
```
< 10 TB → DataSync over internet
10-80 TB → Snowball Edge
> 80 TB → Snowmobile
```

---

## ✅ Checklist

- [ ] Nhớ 7 S3 storage classes và use cases
- [ ] Hiểu S3 lifecycle policies
- [ ] Biết 4 EBS volume types (gp3, io2, st1, sc1)
- [ ] Phân biệt EBS vs EFS vs FSx
- [ ] Hiểu Storage Gateway types
- [ ] Biết khi nào dùng Snow Family

*Tiếp theo: [E. Kiến Trúc Database](E-kien-truc-database.md)*
