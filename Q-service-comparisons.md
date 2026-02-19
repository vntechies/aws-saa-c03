# 📊 Q. SO SÁNH DỊCH VỤ AWS

> **Bảng so sánh chi tiết cho tất cả dịch vụ AWS trong kiến trúc patterns**

---

## 📋 Mục Lục

1. [Compute Services](#1-compute-services)
2. [Storage Services](#2-storage-services)
3. [Database Services](#3-database-services)
4. [Networking & Content Delivery](#4-networking--content-delivery)
5. [Security & Identity](#5-security--identity)
6. [Application Integration](#6-application-integration)
7. [Analytics & Big Data](#7-analytics--big-data)
8. [Management & Governance](#8-management--governance)
9. [Migration & Transfer](#9-migration--transfer)
10. [Developer Tools](#10-developer-tools)

---

## 1. COMPUTE SERVICES

### 1.1 EC2 vs Lambda vs Fargate vs ECS vs EKS

| Feature | EC2 | Lambda | Fargate | ECS | EKS |
|---------|-----|--------|---------|-----|-----|
| **Type** | IaaS | FaaS (Serverless) | Serverless Containers | Container Orchestration | Kubernetes |
| **Management** | Full control | Fully managed | Fully managed | Managed orchestration | Managed control plane |
| **Scaling** | Manual/Auto Scaling | Automatic | Automatic | Manual/Auto Scaling | Manual/Auto Scaling |
| **Pricing** | Per hour/second | Per request + duration | Per vCPU/memory/second | Free (pay for EC2) | $0.10/hour + EC2 |
| **Max Runtime** | Unlimited | 15 minutes | Unlimited | Unlimited | Unlimited |
| **Cold Start** | None (always on) | Yes (~100-1000ms) | Yes (~30s) | None (always on) | None (always on) |
| **Use Case** | Traditional apps | Event-driven, short tasks | Containers without servers | AWS-native containers | Kubernetes workloads |
| **Learning Curve** | Low | Low | Medium | Medium | High |
| **Customization** | Full OS control | Limited (runtime) | Container definition | Container definition | Full Kubernetes |
| **State** | Stateful | Stateless | Stateless/Stateful | Stateless/Stateful | Stateless/Stateful |
| **Min Cost/Month** | ~$8 (t3.micro) | $0 (1M free tier) | ~$10 | ~$8 (EC2) | ~$80 ($73 cluster + EC2) |

**Decision Matrix:**
```
Choose EC2 if:
✅ Need full OS control
✅ Long-running processes
✅ Specific software requirements
✅ Predictable workload (use Reserved Instances)

Choose Lambda if:
✅ Event-driven architecture
✅ Short-lived functions (< 15 min)
✅ Unpredictable/sporadic workload
✅ Want zero operational overhead

Choose Fargate if:
✅ Want containers without managing servers
✅ Microservices architecture
✅ Don't need Kubernetes
✅ Variable workload

Choose ECS if:
✅ AWS-native container solution
✅ Want control over EC2 instances
✅ Simpler than Kubernetes
✅ Tight AWS integration

Choose EKS if:
✅ Already using Kubernetes
✅ Multi-cloud strategy
✅ Complex orchestration needs
✅ Have Kubernetes expertise
```

### 1.2 EC2 Instance Types Comparison

| Family | Type | vCPU | Memory | Network | Storage | Use Case | Price/Hour* |
|--------|------|------|--------|---------|---------|----------|-------------|
| **T3** | Burstable | 2-8 | 0.5-32 GB | Up to 5 Gbps | EBS only | Dev/test, web servers | $0.0104-$0.1664 |
| **M5** | General Purpose | 2-96 | 8-384 GB | Up to 25 Gbps | EBS only | Balanced workloads | $0.096-$4.608 |
| **M6i** | General Purpose | 2-128 | 8-512 GB | Up to 50 Gbps | EBS only | Latest generation | $0.096-$6.144 |
| **C5** | Compute Optimized | 2-96 | 4-192 GB | Up to 25 Gbps | EBS only | High-performance compute | $0.085-$4.08 |
| **C6i** | Compute Optimized | 2-128 | 4-256 GB | Up to 50 Gbps | EBS only | Latest compute | $0.085-$5.44 |
| **R5** | Memory Optimized | 2-96 | 16-768 GB | Up to 25 Gbps | EBS only | In-memory databases | $0.126-$6.048 |
| **R6i** | Memory Optimized | 2-128 | 16-1024 GB | Up to 50 Gbps | EBS only | Latest memory | $0.126-$8.064 |
| **X1e** | Memory Optimized | 32-128 | 976-3904 GB | Up to 25 Gbps | SSD | SAP HANA, in-memory DB | $6.669-$26.688 |
| **I3** | Storage Optimized | 2-64 | 15.25-488 GB | Up to 25 Gbps | NVMe SSD | NoSQL, data warehousing | $0.156-$4.992 |
| **I4i** | Storage Optimized | 2-128 | 16-1024 GB | Up to 75 Gbps | NVMe SSD | Latest storage | $0.171-$10.944 |
| **G4dn** | GPU | 4-96 | 16-384 GB | Up to 100 Gbps | NVMe SSD | ML inference, graphics | $0.526-$10.78 |
| **P3** | GPU | 8-64 | 61-488 GB | Up to 100 Gbps | EBS only | ML training, HPC | $3.06-$24.48 |

*Prices in us-east-1, On-Demand, subject to change

**Instance Family Selection:**
```
T3: Web servers, dev/test, small databases
M5/M6i: Application servers, balanced workloads
C5/C6i: Batch processing, video encoding, gaming servers
R5/R6i: In-memory caches, real-time analytics
X1e: SAP HANA, high-performance databases
I3/I4i: NoSQL databases (Cassandra, MongoDB)
G4dn: ML inference, video transcoding
P3: Deep learning training, HPC simulations
```

### 1.3 EC2 Pricing Models Comparison

| Model | Commitment | Discount | Flexibility | Payment | Best For |
|-------|------------|----------|-------------|---------|----------|
| **On-Demand** | None | 0% | Full | Hourly/Second | Unpredictable workloads |
| **Spot Instances** | None | Up to 90% | Can be interrupted | Hourly | Fault-tolerant workloads |
| **Reserved (1-year)** | 1 year | Up to 40% | Limited | Upfront/Monthly/Hourly | Steady-state workloads |
| **Reserved (3-year)** | 3 years | Up to 72% | Limited | Upfront/Monthly/Hourly | Long-term predictable |
| **Savings Plans** | 1-3 years | Up to 72% | Flexible | Hourly | Flexible compute usage |
| **Dedicated Hosts** | On-Demand/Reserved | Varies | Full control | Hourly/Reserved | Licensing, compliance |
| **Dedicated Instances** | On-Demand/Reserved | Varies | Isolated hardware | Hourly/Reserved | Compliance, isolation |

**Cost Comparison Example (t3.medium in us-east-1):**
```
On-Demand:           $0.0416/hour × 730 hours = $30.37/month
1-Year Reserved:     $0.025/hour × 730 hours  = $18.25/month (40% savings)
3-Year Reserved:     $0.0117/hour × 730 hours = $8.54/month (72% savings)
Spot (average):      $0.0125/hour × 730 hours = $9.13/month (70% savings)
```

---

## 2. STORAGE SERVICES

### 2.1 S3 Storage Classes Comparison

| Storage Class | Availability | AZs | Min Duration | Retrieval Time | Retrieval Cost | Storage Cost* | Use Case |
|---------------|--------------|-----|--------------|----------------|----------------|---------------|----------|
| **S3 Standard** | 99.99% | ≥3 | None | Milliseconds | Free | $0.023/GB | Frequently accessed |
| **S3 Intelligent-Tiering** | 99.9% | ≥3 | None | Milliseconds | Free | $0.023/GB + $0.0025/1000 objects | Unknown/changing patterns |
| **S3 Standard-IA** | 99.9% | ≥3 | 30 days | Milliseconds | $0.01/GB | $0.0125/GB | Infrequent access |
| **S3 One Zone-IA** | 99.5% | 1 | 30 days | Milliseconds | $0.01/GB | $0.01/GB | Non-critical, infrequent |
| **S3 Glacier Instant Retrieval** | 99.9% | ≥3 | 90 days | Milliseconds | $0.03/GB | $0.004/GB | Archive, quarterly access |
| **S3 Glacier Flexible Retrieval** | 99.99% | ≥3 | 90 days | Minutes-Hours | $0.01-$0.03/GB | $0.0036/GB | Archive, annual access |
| **S3 Glacier Deep Archive** | 99.99% | ≥3 | 180 days | 12 hours | $0.02/GB | $0.00099/GB | Long-term archive (7-10 years) |

*Prices in us-east-1, first 50 TB/month

**Retrieval Times Breakdown:**
```
S3 Standard:                  Milliseconds (instant)
S3 Intelligent-Tiering:       Milliseconds (instant)
S3 Standard-IA:               Milliseconds (instant)
S3 One Zone-IA:               Milliseconds (instant)
Glacier Instant Retrieval:    Milliseconds (instant)
Glacier Flexible Retrieval:   
  - Expedited: 1-5 minutes ($0.03/GB)
  - Standard: 3-5 hours ($0.01/GB)
  - Bulk: 5-12 hours ($0.0025/GB)
Glacier Deep Archive:
  - Standard: 12 hours ($0.02/GB)
  - Bulk: 48 hours ($0.0025/GB)
```

**Decision Tree:**
```
Access frequency?
│
├─ Multiple times/week → S3 Standard
├─ Once/month → S3 Standard-IA
├─ Once/quarter → Glacier Instant Retrieval
├─ Once/year → Glacier Flexible Retrieval
└─ Rarely (7-10 years) → Glacier Deep Archive

Unknown pattern? → S3 Intelligent-Tiering

Non-critical data? → Consider One Zone-IA (cheaper)
```

### 2.2 EBS Volume Types Comparison

| Type | Category | Max IOPS | Max Throughput | Max Size | Latency | Price/GB* | Use Case |
|------|----------|----------|----------------|----------|---------|-----------|----------|
| **gp3** | General Purpose SSD | 16,000 | 1,000 MB/s | 16 TB | Single-digit ms | $0.08 | Boot volumes, virtual desktops |
| **gp2** | General Purpose SSD | 16,000 | 250 MB/s | 16 TB | Single-digit ms | $0.10 | Legacy, general purpose |
| **io2 Block Express** | Provisioned IOPS SSD | 256,000 | 4,000 MB/s | 64 TB | Sub-millisecond | $0.125 + IOPS | Mission-critical, largest databases |
| **io2** | Provisioned IOPS SSD | 64,000 | 1,000 MB/s | 16 TB | Single-digit ms | $0.125 + IOPS | High-performance databases |
| **io1** | Provisioned IOPS SSD | 64,000 | 1,000 MB/s | 16 TB | Single-digit ms | $0.125 + IOPS | Legacy provisioned IOPS |
| **st1** | Throughput Optimized HDD | 500 | 500 MB/s | 16 TB | Higher | $0.045 | Big data, data warehouses |
| **sc1** | Cold HDD | 250 | 250 MB/s | 16 TB | Higher | $0.015 | Infrequently accessed data |

*Prices in us-east-1

**IOPS Pricing (io2/io2 Block Express):**
```
io2: $0.065 per provisioned IOPS/month
io2 Block Express: $0.065 per provisioned IOPS/month
```

**Performance Comparison:**
```
gp3 (3,000 IOPS baseline):
  - 125 GB volume = 3,000 IOPS, 125 MB/s
  - Can provision up to 16,000 IOPS independently
  - Cost: $0.08/GB + $0.005/provisioned IOPS (above 3,000)

io2 (50 IOPS/GB):
  - 100 GB volume = 5,000 IOPS
  - 1,000 GB volume = 50,000 IOPS
  - Cost: $0.125/GB + $0.065/IOPS

st1 (40 MB/s per TB baseline):
  - 1 TB volume = 40 MB/s baseline, 250 MB/s burst
  - 5 TB volume = 200 MB/s baseline, 500 MB/s burst
```

**Decision Matrix:**
```
Need > 16,000 IOPS? → io2 Block Express
Need 16,000 IOPS? → gp3 (most cost-effective)
Need consistent low latency? → io2/io2 Block Express
Throughput-intensive (big data)? → st1
Infrequently accessed? → sc1
Boot volumes, general use? → gp3
```

### 2.3 EBS vs EFS vs S3 vs Instance Store

| Feature | EBS | EFS | S3 | Instance Store |
|---------|-----|-----|----|----|
| **Type** | Block storage | File storage | Object storage | Ephemeral block |
| **Attachment** | Single instance* | Multiple instances | Internet/API | Single instance |
| **Availability** | Single AZ | Multi-AZ | Multi-AZ (except One Zone) | Single instance |
| **Durability** | 99.8-99.9% | 99.999999999% (11 9s) | 99.999999999% (11 9s) | Non-durable |
| **Max Size** | 64 TB | Unlimited (petabytes) | Unlimited | Varies by instance |
| **Performance** | Up to 256,000 IOPS | Up to 500,000 IOPS | 3,500-5,500 req/s/prefix | Highest IOPS |
| **Latency** | Single-digit ms | Single-digit ms | Milliseconds | Lowest |
| **Persistence** | Yes | Yes | Yes | No (lost on stop/terminate) |
| **Pricing** | $0.08-$0.125/GB/month | $0.30/GB/month | $0.023/GB/month | Free (included) |
| **Use Case** | EC2 volumes | Shared file storage | Object storage, backups | Temporary cache, buffers |
| **Protocol** | Block device | NFS | HTTP/S | Block device |
| **Backup** | Snapshots to S3 | AWS Backup | Versioning, replication | Manual copy |

*Except io2 Multi-Attach

**When to Use What:**
```
EBS:
✅ Boot volumes for EC2
✅ Databases requiring block storage
✅ Applications needing consistent low latency
✅ Single instance attachment (usually)

EFS:
✅ Shared file storage across multiple instances
✅ Content management systems
✅ Web serving (shared /var/www)
✅ Big data analytics (shared data)

S3:
✅ Static website hosting
✅ Backup and archiving
✅ Data lakes
✅ Application assets (images, videos)

Instance Store:
✅ Temporary cache
✅ Scratch data
✅ Buffers
✅ High IOPS temporary workloads
```

### 2.4 FSx Variants Comparison

| Variant | Protocol | OS Support | Performance | Use Case | Price* |
|---------|----------|------------|-------------|----------|--------|
| **FSx for Windows File Server** | SMB | Windows, Linux | Up to 2 GB/s, 100,000 IOPS | Windows apps, Active Directory | $0.013/GB/month |
| **FSx for Lustre** | POSIX | Linux | Up to 1 TB/s, millions IOPS | HPC, ML training, video processing | $0.145/GB/month (SSD) |
| **FSx for NetApp ONTAP** | NFS, SMB, iSCSI | Windows, Linux, macOS | Up to 2 GB/s | Enterprise, multi-protocol | $0.182/GB/month |
| **FSx for OpenZFS** | NFS | Linux | Up to 1 GB/s, 1M IOPS | Linux workloads, snapshots | $0.165/GB/month |

*Prices in us-east-1, SSD storage

**Feature Comparison:**
```
FSx for Windows:
✅ Native Windows SMB
✅ Active Directory integration
✅ DFS namespaces
✅ Shadow copies
✅ Data deduplication

FSx for Lustre:
✅ POSIX-compliant
✅ S3 integration (read/write)
✅ Sub-millisecond latency
✅ Parallel file system
✅ Scratch and persistent options

FSx for NetApp ONTAP:
✅ Multi-protocol (NFS, SMB, iSCSI)
✅ SnapMirror replication
✅ FlexClone (instant clones)
✅ Storage efficiency (compression, dedup)
✅ Multi-AZ

FSx for OpenZFS:
✅ Point-in-time snapshots
✅ Data compression
✅ Copy-on-write
✅ NFS v3, v4, v4.1, v4.2
✅ Up to 1M IOPS
```

---

## 3. DATABASE SERVICES

### 3.1 RDS vs Aurora vs DynamoDB vs Redshift

| Feature | RDS | Aurora | DynamoDB | Redshift |
|---------|-----|--------|----------|----------|
| **Type** | Relational (SQL) | Relational (SQL) | NoSQL (Key-Value/Document) | Data Warehouse (SQL) |
| **Engines** | MySQL, PostgreSQL, MariaDB, Oracle, SQL Server | MySQL, PostgreSQL | Proprietary | Proprietary (PostgreSQL compatible) |
| **Performance** | Standard | 5x MySQL, 3x PostgreSQL | Single-digit ms | 10x faster than traditional DW |
| **Scalability** | Vertical (instance size) | Vertical + Horizontal (15 replicas) | Horizontal (unlimited) | Horizontal (massive parallel) |
| **Storage** | Up to 64 TB | Up to 128 TB (auto-scaling) | Unlimited | Up to 8 PB per cluster |
| **Replication** | Read Replicas (5) | Read Replicas (15) | Global Tables | Multi-node clusters |
| **Availability** | Multi-AZ (2 AZ) | Multi-AZ (3 AZ, 6 copies) | Multi-AZ (3 AZ) | Multi-node, multi-AZ |
| **Backup** | Automated, snapshots | Automated, continuous | On-demand, PITR | Automated, snapshots |
| **Pricing Model** | Instance + storage | Instance + storage + I/O | On-Demand or Provisioned | Node hours + data scanned |
| **Use Case** | Traditional apps, migrations | High-performance, cloud-native | High-scale, low-latency | Analytics, BI, OLAP |
| **Serverless** | ❌ No | ✅ Yes (Aurora Serverless) | ✅ Yes (On-Demand mode) | ✅ Yes (Redshift Serverless) |
| **Price (Starter)** | ~$30/month (t3.micro) | ~$60/month (t3.small) | ~$1.25/month (On-Demand) | ~$180/month (dc2.large) |

**Performance Comparison (Transactions per Second):**
```
RDS MySQL (db.r5.large):     ~10,000 TPS
Aurora MySQL:                ~50,000 TPS (5x faster)
DynamoDB:                    ~100,000+ TPS (unlimited scaling)
Redshift (analytics queries): Optimized for complex queries, not TPS
```

**Decision Matrix:**
```
Choose RDS if:
✅ Migrating existing database (lift-and-shift)
✅ Need specific engine version
✅ Standard SQL workload
✅ Budget-conscious

Choose Aurora if:
✅ Need high performance (5x MySQL)
✅ High availability critical
✅ Want cloud-native features
✅ Need global database

Choose DynamoDB if:
✅ Need single-digit millisecond latency
✅ Massive scale (millions of requests/sec)
✅ Serverless preferred
✅ Key-value or document data model

Choose Redshift if:
✅ Data warehousing
✅ Complex analytical queries
✅ Business intelligence
✅ Petabyte-scale data
```

### 3.2 RDS Multi-AZ vs Read Replicas

| Feature | Multi-AZ | Read Replicas |
|---------|----------|---------------|
| **Purpose** | High availability | Read scalability |
| **Replication** | Synchronous | Asynchronous |
| **Standby Usage** | Passive (no read traffic) | Active (serves read traffic) |
| **Failover** | Automatic (1-2 minutes) | Manual promotion |
| **Endpoint** | Single endpoint (DNS switches) | Separate endpoint per replica |
| **Number** | 1 standby | Up to 5 (RDS), 15 (Aurora) |
| **Cross-Region** | ❌ No | ✅ Yes |
| **Performance Impact** | Minimal (sync replication overhead) | None on primary |
| **Cost** | 2x instance cost | 1x instance cost per replica |
| **Use Case** | Disaster recovery, HA | Read-heavy workloads, reporting |

**Replication Lag:**
```
Multi-AZ:
  - Synchronous replication
  - Zero data loss (RPO = 0)
  - RTO = 1-2 minutes (automatic failover)

Read Replicas:
  - Asynchronous replication
  - Replication lag: seconds to minutes
  - RPO = replication lag
  - RTO = manual promotion time
```

**Can You Combine Both?**
```
✅ YES! Best practice for production:

Primary (Multi-AZ) → Standby (same region, sync)
                  ↓
          Read Replica 1 (same region, async)
          Read Replica 2 (different region, async)

Benefits:
- High availability (Multi-AZ)
- Read scaling (Read Replicas)
- Disaster recovery (Cross-region replica)
```

### 3.3 Aurora vs Aurora Serverless

| Feature | Aurora Provisioned | Aurora Serverless v2 |
|---------|-------------------|----------------------|
| **Scaling** | Manual or Auto Scaling | Instant auto-scaling |
| **Capacity** | Fixed instance size | 0.5-128 ACU (Aurora Capacity Units) |
| **Scaling Time** | Minutes (instance change) | Seconds (automatic) |
| **Minimum Cost** | Instance always running | Can scale to 0 (pause) |
| **Cold Start** | None | ~30 seconds (if paused) |
| **Use Case** | Predictable workload | Variable/unpredictable workload |
| **Price Example** | db.t3.medium = $0.082/hour ($60/month) | 0.5 ACU min = $0.06/hour ($44/month) |
| **Best For** | Production, steady traffic | Dev/test, intermittent, unpredictable |

**Aurora Capacity Units (ACU):**
```
1 ACU = ~2 GB RAM + corresponding CPU/networking

Scaling:
- Serverless v2: Scales in 0.5 ACU increments
- Min: 0.5 ACU (1 GB RAM)
- Max: 128 ACU (256 GB RAM)
- Scale up: Instant (seconds)
- Scale down: Gradual (after 15 min of low activity)
```

**Cost Comparison (us-east-1, MySQL):**
```
Provisioned (db.t3.medium):
  - $0.082/hour × 730 hours = $59.86/month
  - Always running

Serverless v2 (0.5-8 ACU):
  - $0.12/ACU/hour
  - Average 2 ACU: $0.24/hour × 730 hours = $175.20/month
  - Can pause to 0: $0 when idle
  - Variable workload: ~$50-100/month (with pausing)
```

### 3.4 DynamoDB Capacity Modes

| Feature | On-Demand | Provisioned |
|---------|-----------|-------------|
| **Billing** | Per request | Per hour (RCU/WCU) |
| **Capacity Planning** | None | Must estimate |
| **Scaling** | Automatic | Auto Scaling (with delays) |
| **Cost (Low Traffic)** | Higher per request | Lower (if utilized) |
| **Cost (High Traffic)** | Can be expensive | Lower (predictable) |
| **Use Case** | Unpredictable traffic | Predictable, steady traffic |
| **Price** | $1.25/million writes, $0.25/million reads | $0.00065/WCU/hour, $0.00013/RCU/hour |

**Cost Comparison Example:**
```
Scenario: 1 million writes/month, 10 million reads/month

On-Demand:
  Writes: 1M × $1.25 = $1.25
  Reads: 10M × $0.25 = $2.50
  Total: $3.75/month

Provisioned (with Auto Scaling):
  WCU needed: 1M / (30 days × 24 hours × 3600 sec) = ~0.4 WCU → 1 WCU
  RCU needed: 10M / (30 days × 24 hours × 3600 sec) = ~4 RCU → 4 RCU
  
  WCU cost: 1 × $0.00065 × 730 hours = $0.47
  RCU cost: 4 × $0.00013 × 730 hours = $0.38
  Total: $0.85/month

Savings: $3.75 - $0.85 = $2.90 (77% cheaper with Provisioned)
```

**When to Use On-Demand:**
```
✅ New applications (unknown traffic)
✅ Unpredictable spikes
✅ Pay-per-use preference
✅ Low-traffic applications

When to Use Provisioned:
✅ Predictable traffic patterns
✅ Steady-state workload
✅ Cost optimization priority
✅ Can forecast capacity
```

### 3.5 ElastiCache Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Data Types** | Strings, Lists, Sets, Sorted Sets, Hashes, Bitmaps, HyperLogLogs, Geospatial | Strings only |
| **Persistence** | ✅ Yes (RDB, AOF) | ❌ No |
| **Replication** | ✅ Yes (Multi-AZ, read replicas) | ❌ No |
| **High Availability** | ✅ Yes (automatic failover) | ❌ No |
| **Backup/Restore** | ✅ Yes | ❌ No |
| **Pub/Sub** | ✅ Yes | ❌ No |
| **Lua Scripting** | ✅ Yes | ❌ No |
| **Geospatial** | ✅ Yes | ❌ No |
| **Multi-threading** | ❌ No (single-threaded) | ✅ Yes (multi-threaded) |
| **Max Key Size** | 512 MB | 1 MB |
| **Sharding** | Cluster mode | Consistent hashing |
| **Use Case** | Advanced caching, session store, real-time analytics | Simple caching, multi-core utilization |
| **Complexity** | Higher | Lower |
| **Price** | Slightly higher | Slightly lower |

**Performance Comparison:**
```
Redis (single-threaded):
  - ~100,000 ops/sec per node
  - Better for complex operations
  - Persistent data

Memcached (multi-threaded):
  - ~200,000 ops/sec per node (multi-core)
  - Better for simple GET/SET
  - Volatile data only
```

**Decision Matrix:**
```
Choose Redis if:
✅ Need data persistence
✅ Need high availability (Multi-AZ)
✅ Need complex data types (lists, sets, sorted sets)
✅ Need pub/sub messaging
✅ Need backup and restore
✅ Need geospatial data
✅ Need Lua scripting

Choose Memcached if:
✅ Need simple caching only
✅ Want to utilize multi-core CPUs
✅ Don't need persistence
✅ Don't need replication
✅ Simpler architecture preferred
```

---

## 4. NETWORKING & CONTENT DELIVERY

### 4.1 ALB vs NLB vs CLB vs GWLB

| Feature | ALB | NLB | CLB | GWLB |
|---------|-----|-----|-----|------|
| **Layer** | Layer 7 (HTTP/HTTPS) | Layer 4 (TCP/UDP/TLS) | Layer 4/7 | Layer 3 (IP) |
| **Protocol** | HTTP, HTTPS, gRPC, WebSocket | TCP, UDP, TLS | HTTP, HTTPS, TCP, SSL | IP |
| **Performance** | Moderate | Ultra-high (millions req/sec) | Low | High |
| **Latency** | ~50-100ms | < 100 microseconds | Higher | Low |
| **Static IP** | ❌ No | ✅ Yes (per AZ) | ❌ No | ✅ Yes |
| **Elastic IP** | ❌ No | ✅ Yes | ❌ No | ✅ Yes |
| **Target Types** | Instances, IPs, Lambda, Containers | Instances, IPs, ALB | Instances only | Virtual appliances |
| **Path-based Routing** | ✅ Yes | ❌ No | ❌ No | ❌ No |
| **Host-based Routing** | ✅ Yes | ❌ No | ❌ No | ❌ No |
| **SSL Termination** | ✅ Yes | ✅ Yes (TLS) | ✅ Yes | ❌ No |
| **WebSocket** | ✅ Yes | ✅ Yes | ❌ No | ❌ No |
| **HTTP/2** | ✅ Yes | ❌ No | ❌ No | ❌ No |
| **Preserve Source IP** | Via X-Forwarded-For | ✅ Yes | Via Proxy Protocol | ✅ Yes |
| **Health Checks** | HTTP/HTTPS | TCP, HTTP, HTTPS | TCP, HTTP, HTTPS, SSL | TCP, HTTP, HTTPS |
| **Cross-Zone LB** | Always enabled (free) | Disabled by default (charges apply) | Disabled by default (free) | N/A |
| **Price** | $0.0225/hour + $0.008/LCU | $0.0225/hour + $0.006/NLCU | $0.025/hour + $0.008/GB | $0.0125/hour + $0.004/GLCU |
| **Use Case** | Microservices, HTTP routing | Extreme performance, static IP | Legacy (not recommended) | Third-party appliances |

**Performance Comparison:**
```
ALB:
  - ~100,000 requests/second
  - Layer 7 features (path routing, etc.)
  - SSL termination

NLB:
  - Millions of requests/second
  - Ultra-low latency (< 100μs)
  - Static IP support
  - Best for TCP/UDP

CLB:
  - Legacy, lower performance
  - Not recommended for new apps

GWLB:
  - Deploy third-party appliances
  - Transparent network gateway
```

**Decision Matrix:**
```
Choose ALB if:
✅ HTTP/HTTPS traffic
✅ Need path-based or host-based routing
✅ Microservices architecture
✅ Need Lambda targets
✅ Need HTTP/2 or WebSocket

Choose NLB if:
✅ Extreme performance needed (millions req/sec)
✅ Ultra-low latency required
✅ Need static IP or Elastic IP
✅ TCP/UDP traffic
✅ Need to preserve source IP

Choose CLB if:
✅ Legacy applications (migrate to ALB/NLB)

Choose GWLB if:
✅ Deploy third-party security appliances
✅ Inline network inspection
✅ Transparent traffic routing
```

### 4.2 CloudFront vs Global Accelerator

| Feature | CloudFront | Global Accelerator |
|---------|------------|-------------------|
| **Type** | CDN (Content Delivery Network) | Network accelerator |
| **Layer** | Layer 7 (HTTP/HTTPS) | Layer 4 (TCP/UDP) |
| **Use Case** | Cache static/dynamic content | Accelerate TCP/UDP applications |
| **Caching** | ✅ Yes (edge caching) | ❌ No caching |
| **Protocols** | HTTP, HTTPS, WebSocket | TCP, UDP |
| **Edge Locations** | 600+ | AWS edge network |
| **Static IP** | ❌ No (use DNS) | ✅ Yes (2 Anycast IPs) |
| **Origin** | S3, ALB, NLB, EC2, custom | ALB, NLB, EC2, Elastic IP |
| **Performance Gain** | 50-500% (via caching) | 60% latency reduction |
| **SSL Termination** | ✅ Yes | ❌ No (pass-through) |
| **DDoS Protection** | ✅ Yes (Shield Standard) | ✅ Yes (Shield Standard) |
| **Health Checks** | Origin health checks | Endpoint health checks |
| **Failover** | Origin failover | Automatic endpoint failover |
| **Price** | $0.085/GB (data transfer) | $0.025/hour + $0.015/GB |

**When to Use:**
```
CloudFront:
✅ Static website (S3)
✅ Video streaming
✅ API acceleration (with caching)
✅ Dynamic content delivery
✅ Need SSL termination at edge

Global Accelerator:
✅ Non-HTTP applications (gaming, VoIP)
✅ Need static IP addresses
✅ Multi-region active-active
✅ Instant regional failover
✅ TCP/UDP optimization
```

**Performance Example:**
```
Scenario: User in Singapore accessing content in us-east-1

Without Optimization:
  - Direct connection: 250ms latency
  - Variable performance

With CloudFront:
  - First request: 250ms (cache miss)
  - Subsequent requests: 10ms (cache hit)
  - 96% faster for cached content

With Global Accelerator:
  - All requests: 100ms (60% reduction)
  - Consistent low latency
  - No caching, always fresh data
```

### 4.3 VPC Peering vs Transit Gateway vs VPN vs Direct Connect

| Feature | VPC Peering | Transit Gateway | Site-to-Site VPN | Direct Connect |
|---------|-------------|-----------------|------------------|----------------|
| **Connection Type** | VPC to VPC | Hub-and-spoke | Encrypted tunnel | Dedicated fiber |
| **Max VPCs** | 1-to-1 | Thousands | N/A | N/A |
| **Transitive Routing** | ❌ No | ✅ Yes | N/A | N/A |
| **Bandwidth** | No limit | 50 Gbps per AZ | Up to 1.25 Gbps | 1/10/100 Gbps |
| **Latency** | Low (same as intra-VPC) | Low | Variable (internet) | Low, consistent |
| **Encryption** | ❌ No (but private) | ✅ Yes (optional) | ✅ Yes (IPSec) | ❌ No (but private) |
| **Cost** | Data transfer only | $0.05/hour + data | $0.05/hour + data | $$$ (port + data) |
| **Setup Time** | Minutes | Minutes | Minutes | Weeks |
| **On-Premises** | ❌ No | ✅ Yes (via VPN/DX) | ✅ Yes | ✅ Yes |
| **Use Case** | Simple VPC-VPC | Hub-spoke, many VPCs | Quick on-prem connection | Production, high bandwidth |

**Architecture Comparison:**
```
VPC Peering (No Transitive):
VPC-A ←→ VPC-B
VPC-A ←→ VPC-C
VPC-B ←/→ VPC-C (need separate peering)

Transit Gateway (Transitive):
        Transit Gateway
       /       |       \
   VPC-A    VPC-B    VPC-C
(All VPCs can communicate through TGW)

VPN:
On-Premises ←IPSec→ Virtual Private Gateway ←→ VPC

Direct Connect:
On-Premises ←Fiber→ Direct Connect Location ←→ VPC
```

**Cost Comparison (us-east-1, 1 TB data/month):**
```
VPC Peering:
  - Peering: Free
  - Data transfer (same AZ): Free
  - Data transfer (different AZ): $0.01/GB × 1000 GB = $10

Transit Gateway:
  - Attachment: $0.05/hour × 730 hours = $36.50
  - Data processing: $0.02/GB × 1000 GB = $20
  - Total: $56.50/month

Site-to-Site VPN:
  - VPN connection: $0.05/hour × 730 hours = $36.50
  - Data transfer out: $0.09/GB × 1000 GB = $90
  - Total: $126.50/month

Direct Connect (1 Gbps):
  - Port fee: $0.30/hour × 730 hours = $219
  - Data transfer out: $0.02/GB × 1000 GB = $20
  - Total: $239/month (but consistent performance)
```

**Decision Matrix:**
```
VPC Peering:
✅ Simple VPC-to-VPC connection
✅ Low latency required
✅ Cost-sensitive
✅ No transitive routing needed

Transit Gateway:
✅ Many VPCs (> 5)
✅ Hub-and-spoke architecture
✅ Need transitive routing
✅ Centralized management

Site-to-Site VPN:
✅ Quick on-premises connection
✅ Encrypted connection required
✅ Temporary or backup connection
✅ Budget-friendly

Direct Connect:
✅ Production workloads
✅ High bandwidth (> 1 Gbps)
✅ Consistent low latency
✅ Large data transfers
✅ Hybrid architecture
```

### 4.4 Route 53 Routing Policies Comparison

| Policy | Use Case | How It Works | Health Check | Failover |
|--------|----------|--------------|--------------|----------|
| **Simple** | Single resource | Returns single IP | ❌ No | ❌ No |
| **Weighted** | A/B testing, gradual migration | Distributes traffic by weight (%) | ✅ Yes | ✅ Yes |
| **Latency-Based** | Minimize latency | Routes to lowest latency region | ✅ Yes | ✅ Yes |
| **Failover** | Active-passive DR | Primary/secondary setup | ✅ Yes (required) | ✅ Yes |
| **Geolocation** | Content localization | Routes based on user location | ✅ Yes | ✅ Yes |
| **Geoproximity** | Traffic bias | Routes based on proximity + bias | ✅ Yes | ✅ Yes |
| **Multi-Value** | Multiple IPs with health checks | Returns multiple healthy IPs | ✅ Yes | ✅ Yes |

**Examples:**

**1. Simple Routing:**
```
example.com → 192.0.2.1
(All traffic goes to single IP)
```

**2. Weighted Routing:**
```
example.com:
  - us-east-1 (weight 70) → 70% traffic
  - eu-west-1 (weight 30) → 30% traffic

Use case: Gradual migration, A/B testing
```

**3. Latency-Based Routing:**
```
User in Singapore:
  - us-east-1: 250ms latency
  - ap-southeast-1: 10ms latency
  → Routes to ap-southeast-1

User in New York:
  - us-east-1: 10ms latency
  - ap-southeast-1: 280ms latency
  → Routes to us-east-1
```

**4. Failover Routing:**
```
Primary: us-east-1 (health check: passing)
Secondary: eu-west-1 (standby)

If primary fails:
  → Automatically routes to secondary
```

**5. Geolocation Routing:**
```
Users from:
  - USA → us-east-1 (English content)
  - Europe → eu-west-1 (GDPR compliant)
  - Asia → ap-southeast-1 (localized content)
  - Default → us-east-1 (fallback)
```

**6. Geoproximity Routing:**
```
Resource in us-east-1 (bias +50):
  - Attracts more traffic from wider area

Resource in eu-west-1 (bias -50):
  - Attracts less traffic, smaller coverage area
```

**7. Multi-Value Routing:**
```
example.com returns up to 8 IPs:
  - 192.0.2.1 (healthy)
  - 192.0.2.2 (healthy)
  - 192.0.2.3 (unhealthy - excluded)
  - 192.0.2.4 (healthy)

Client gets 3 healthy IPs, tries each until success
```

---

## 5. SECURITY & IDENTITY

### 5.1 Security Groups vs Network ACLs

| Feature | Security Groups | Network ACLs |
|---------|-----------------|--------------|
| **Level** | Instance (ENI) | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow + Deny |
| **Rule Evaluation** | All rules | Numbered order (lowest first) |
| **Default** | Deny all inbound, allow all outbound | Allow all |
| **Response Traffic** | Automatically allowed | Must explicitly allow |
| **Rule Limit** | 60 inbound, 60 outbound | 20 inbound, 20 outbound |
| **Association** | Multiple instances | One subnet |
| **Use Case** | Primary security layer | Additional layer, explicit denies |

**Stateful vs Stateless:**
```
Security Group (Stateful):
  Inbound: Allow HTTP (port 80) from 0.0.0.0/0
  Outbound: Response automatically allowed
  
  Request: 0.0.0.0:12345 → Instance:80 ✅
  Response: Instance:80 → 0.0.0.0:12345 ✅ (automatic)

Network ACL (Stateless):
  Inbound: Allow HTTP (port 80) from 0.0.0.0/0
  Outbound: Must allow ephemeral ports (1024-65535)
  
  Request: 0.0.0.0:12345 → Instance:80 ✅
  Response: Instance:80 → 0.0.0.0:12345 ✅ (explicit rule needed)
```

**Example Configuration:**
```
Security Group:
  Inbound:
    - Type: HTTP, Port: 80, Source: 0.0.0.0/0
    - Type: HTTPS, Port: 443, Source: 0.0.0.0/0
    - Type: SSH, Port: 22, Source: 203.0.113.0/24
  Outbound:
    - Type: All, Port: All, Destination: 0.0.0.0/0

Network ACL:
  Inbound:
    100 - Allow HTTP (80) from 0.0.0.0/0
    110 - Allow HTTPS (443) from 0.0.0.0/0
    120 - Allow SSH (22) from 203.0.113.0/24
    130 - Deny all from 198.51.100.0/24 (blocked IP range)
    * - Deny all
  
  Outbound:
    100 - Allow HTTP (80) to 0.0.0.0/0
    110 - Allow HTTPS (443) to 0.0.0.0/0
    120 - Allow ephemeral (1024-65535) to 0.0.0.0/0
    * - Deny all
```

**When to Use NACLs:**
```
✅ Block specific IP addresses (explicit DENY)
✅ Additional layer of security (defense in depth)
✅ Subnet-level protection
✅ Stateless filtering requirements
```

### 5.2 IAM Policies vs Resource Policies vs SCPs

| Feature | IAM Policies | Resource Policies | SCPs (Service Control Policies) |
|---------|--------------|-------------------|--------------------------------|
| **Attach To** | Users, Groups, Roles | Resources (S3, SQS, etc.) | Accounts, OUs |
| **Scope** | Identity-based | Resource-based | Organization-wide |
| **Effect** | Grant permissions | Grant permissions | Set permission boundaries |
| **Cross-Account** | Via roles | ✅ Direct (has Principal) | ✅ Yes |
| **Evaluation** | Identity context | Resource context | Account context |
| **Use Case** | User/role permissions | Resource access control | Organizational governance |

**Example Scenarios:**

**1. IAM Policy (Identity-Based):**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::my-bucket/*"
  }]
}

Attached to: IAM User/Role
Grants: Permission to read objects from my-bucket
```

**2. S3 Bucket Policy (Resource-Based):**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {"AWS": "arn:aws:iam::123456789012:user/Alice"},
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::my-bucket/*"
  }]
}

Attached to: S3 Bucket
Grants: Alice (from Account 123456789012) can read objects
Key difference: Has "Principal" element
```

**3. SCP (Service Control Policy):**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": "ec2:*",
    "Resource": "*",
    "Condition": {
      "StringNotEquals": {
        "ec2:Region": ["us-east-1", "us-west-2"]
      }
    }
  }]
}

Attached to: AWS Organization OU
Effect: Prevents launching EC2 in regions other than us-east-1, us-west-2
Note: Does NOT grant permissions, only restricts
```

**Policy Evaluation Logic:**
```
1. Explicit DENY (in any policy) → DENY
2. SCP DENY → DENY
3. Resource Policy ALLOW → ALLOW
4. Identity Policy ALLOW → ALLOW
5. Default → DENY (implicit)
```

### 5.3 KMS vs CloudHSM vs ACM

| Feature | KMS | CloudHSM | ACM |
|---------|-----|----------|-----|
| **Purpose** | Key management | Hardware security module | SSL/TLS certificates |
| **Type** | Managed service | Dedicated hardware | Managed certificates |
| **Compliance** | FIPS 140-2 Level 2 | FIPS 140-2 Level 3 | N/A |
| **Key Control** | AWS manages hardware | You manage keys | AWS manages |
| **Multi-Tenancy** | ✅ Yes | ❌ No (dedicated) | ✅ Yes |
| **Pricing** | $1/key/month + API calls | $1.45/hour per HSM | Free (for AWS services) |
| **Use Case** | General encryption | Regulatory compliance, custom crypto | HTTPS/TLS certificates |
| **Integration** | Native AWS services | Custom applications | ELB, CloudFront, API Gateway |

**When to Use:**
```
KMS:
✅ Encrypt data in AWS services (S3, EBS, RDS)
✅ Envelope encryption
✅ Cost-effective key management
✅ Standard compliance requirements

CloudHSM:
✅ FIPS 140-2 Level 3 required
✅ Single-tenant hardware required
✅ Custom cryptographic operations
✅ Contractual/regulatory requirements

ACM:
✅ SSL/TLS certificates for websites
✅ Free certificates for AWS services
✅ Automatic renewal
✅ HTTPS on ELB, CloudFront, API Gateway
```

---

## 6. APPLICATION INTEGRATION

### 6.1 SQS vs SNS vs EventBridge vs Kinesis

| Feature | SQS | SNS | EventBridge | Kinesis Data Streams |
|---------|-----|-----|-------------|---------------------|
| **Type** | Message Queue | Pub/Sub | Event Bus | Data Streaming |
| **Pattern** | Point-to-point | Fan-out (1-to-many) | Event-driven | Real-time streaming |
| **Message Retention** | 1 min - 14 days | No retention | No retention | 1-365 days |
| **Throughput** | Unlimited | Unlimited | Unlimited | Limited by shards |
| **Ordering** | FIFO queues only | ❌ No | ❌ No | ✅ Yes (per shard) |
| **Duplicate Messages** | At-least-once (Standard) | At-least-once | At-least-once | Exactly-once (with processing) |
| **Consumers** | Single (poll-based) | Multiple (push-based) | Multiple (targets) | Multiple (shard-based) |
| **Message Size** | 256 KB | 256 KB | 256 KB | 1 MB |
| **Delay** | 0-15 minutes | None | None | None |
| **Use Case** | Decouple services, buffering | Notifications, fan-out | Event routing, integrations | Real-time analytics, logs |
| **Price** | $0.40/million requests | $0.50/million requests | $1/million events | $0.015/shard/hour + PUT |

**Message Flow Comparison:**

**SQS (Queue):**
```
Producer → SQS Queue → Consumer (polls)
                    → Consumer (polls)
(Multiple consumers compete for messages)
```

**SNS (Pub/Sub):**
```
Publisher → SNS Topic → Subscriber 1 (push)
                     → Subscriber 2 (push)
                     → Subscriber 3 (push)
(All subscribers receive copy of message)
```

**EventBridge (Event Bus):**
```
Event Source → EventBridge → Rule 1 → Target (Lambda)
                          → Rule 2 → Target (SQS)
                          → Rule 3 → Target (Step Functions)
(Rules filter and route events)
```

**Kinesis (Stream):**
```
Producers → Kinesis Stream (Shards) → Consumer 1 (reads)
                                    → Consumer 2 (reads)
(Multiple consumers read same data)
```

**Combined Pattern (Fan-Out with SQS):**
```
Producer → SNS Topic → SQS Queue 1 → Service A
                    → SQS Queue 2 → Service B
                    → SQS Queue 3 → Service C

Benefits:
- Fan-out to multiple services
- Each service processes at its own pace
- Guaranteed delivery (SQS retention)
```

**Decision Matrix:**
```
Choose SQS if:
✅ Need message buffering
✅ Decouple services
✅ Asynchronous processing
✅ Message retention required
✅ Single consumer per message

Choose SNS if:
✅ Publish to multiple subscribers
✅ Fan-out pattern
✅ Push notifications (email, SMS, mobile)
✅ Real-time alerts

Choose EventBridge if:
✅ Event-driven architecture
✅ SaaS integrations
✅ Complex event routing
✅ Event filtering required

Choose Kinesis if:
✅ Real-time data streaming
✅ Log/event aggregation
✅ Multiple consumers need same data
✅ Ordered data processing
✅ Replay capability needed
```

### 6.2 SQS Standard vs FIFO

| Feature | Standard Queue | FIFO Queue |
|---------|---------------|------------|
| **Ordering** | Best-effort | Guaranteed (strict order) |
| **Throughput** | Unlimited | 300 TPS (without batching), 3,000 TPS (with batching) |
| **Delivery** | At-least-once (possible duplicates) | Exactly-once |
| **Message Deduplication** | ❌ No | ✅ Yes (5-minute window) |
| **Message Groups** | ❌ No | ✅ Yes (parallel processing) |
| **Price** | $0.40/million requests | $0.50/million requests |
| **Use Case** | High throughput, order not critical | Order critical, no duplicates |

**Example Scenarios:**

**Standard Queue:**
```
Order of messages sent: A, B, C, D, E
Possible receive order: A, C, B, E, D, B (duplicate)

Use case: 
- Image processing (order doesn't matter)
- Log aggregation
- Email sending
```

**FIFO Queue:**
```
Order of messages sent: A, B, C, D, E
Receive order: A, B, C, D, E (guaranteed)
No duplicates

Use case:
- Financial transactions
- Order processing
- Command execution
```

**Message Groups (FIFO):**
```
Queue: orders.fifo

Messages:
- Order 1 (Group: customer-123)
- Order 2 (Group: customer-456)
- Order 3 (Group: customer-123)
- Order 4 (Group: customer-456)

Processing:
- Group customer-123: Order 1 → Order 3 (sequential)
- Group customer-456: Order 2 → Order 4 (sequential)
- Groups process in parallel
```

---

## 7. ANALYTICS & BIG DATA

### 7.1 Kinesis Data Streams vs Kinesis Data Firehose

| Feature | Data Streams | Data Firehose |
|---------|--------------|---------------|
| **Purpose** | Real-time streaming | Data delivery to destinations |
| **Retention** | 1-365 days | None (immediate delivery) |
| **Processing** | Custom (Lambda, KCL) | Built-in transformations |
| **Consumers** | Multiple | Single destination |
| **Latency** | < 1 second | 60 seconds minimum |
| **Scaling** | Manual (shards) | Automatic |
| **Destinations** | Custom | S3, Redshift, OpenSearch, HTTP, Splunk |
| **Replay** | ✅ Yes | ❌ No |
| **Pricing** | $0.015/shard/hour + PUT | $0.029/GB ingested |
| **Use Case** | Real-time analytics, custom processing | ETL, data lake ingestion |

**Architecture Comparison:**

**Kinesis Data Streams:**
```
Producers → Data Streams (Shards) → Consumer 1 (Lambda)
                                  → Consumer 2 (KCL App)
                                  → Consumer 3 (Kinesis Analytics)

Features:
- Multiple consumers
- Replay data (within retention)
- Real-time processing (< 1 sec)
- Custom logic
```

**Kinesis Data Firehose:**
```
Producers → Firehose → [Optional: Lambda Transform] → S3/Redshift/OpenSearch

Features:
- Single destination
- Automatic scaling
- Built-in transformations
- No shard management
- Near real-time (60 sec buffer)
```

**Decision:**
```
Choose Data Streams if:
✅ Need multiple consumers
✅ Need data replay
✅ Need < 1 second latency
✅ Custom processing logic
✅ Real-time analytics

Choose Data Firehose if:
✅ Simple data delivery to S3/Redshift
✅ Don't need data replay
✅ Want automatic scaling
✅ ETL to data lake
✅ No custom consumer logic needed
```

### 7.2 Athena vs Redshift vs EMR

| Feature | Athena | Redshift | EMR |
|---------|--------|----------|-----|
| **Type** | Serverless SQL | Data Warehouse | Managed Hadoop/Spark |
| **Query Language** | SQL (Presto) | SQL (PostgreSQL compatible) | Hadoop, Spark, Hive, Presto |
| **Data Source** | S3 (data lake) | Redshift storage | S3, HDFS |
| **Pricing** | $5/TB scanned | Cluster hours | Cluster hours |
| **Performance** | Moderate | High (for DW queries) | High (for big data) |
| **Setup** | None | Provision cluster | Provision cluster |
| **Use Case** | Ad-hoc queries on S3 | Data warehousing, BI | Big data processing, ML |
| **Best For** | Infrequent queries | Frequent queries, reports | Complex transformations |

**Cost Comparison (1 TB data, 100 queries/month):**
```
Athena:
  - $5/TB scanned × 1 TB × 100 queries = $500/month
  - Optimization: Use partitioning, columnar formats (Parquet)
  - With Parquet: $5/TB × 0.1 TB × 100 = $50/month

Redshift (dc2.large):
  - $0.25/hour × 730 hours = $182.50/month
  - Unlimited queries
  - Better for > 50 queries/month

EMR (m5.xlarge × 3 nodes):
  - $0.27/hour × 3 nodes × 730 hours = $591.30/month
  - For complex Spark/Hadoop jobs
```

**Decision:**
```
Choose Athena if:
✅ Ad-hoc queries on S3
✅ Infrequent queries (< 50/month)
✅ Serverless preferred
✅ No infrastructure management

Choose Redshift if:
✅ Data warehousing
✅ Frequent queries (> 50/month)
✅ Complex joins and aggregations
✅ BI tools integration

Choose EMR if:
✅ Big data processing (Hadoop, Spark)
✅ Machine learning (Spark MLlib)
✅ Complex ETL transformations
✅ Need specific big data frameworks
```

---

## 8. MANAGEMENT & GOVERNANCE

### 8.1 CloudWatch vs CloudTrail vs Config vs X-Ray

| Feature | CloudWatch | CloudTrail | Config | X-Ray |
|---------|------------|------------|--------|-------|
| **Purpose** | Monitoring & Logs | API Audit Trail | Config Tracking | Application Tracing |
| **What It Tracks** | Metrics, Logs, Alarms | API calls (who, what, when) | Resource configurations | Request traces |
| **Retention** | Configurable | 90 days (default) | Configurable | Configurable |
| **Use Case** | Performance monitoring | Security audit | Compliance, change tracking | Debug, performance analysis |
| **Real-Time** | ✅ Yes | ❌ No (15 min delay) | ❌ No | ✅ Yes |
| **Alerting** | ✅ Yes (Alarms) | ✅ Yes (via EventBridge) | ✅ Yes (Rules) | ❌ No |
| **Price** | $0.30/GB ingested | $2/100k events | $0.003/config item | $5/million traces |

**What Each Service Answers:**

**CloudWatch:**
```
"How is my system performing?"
- CPU utilization is 80%
- Disk space is 90% full
- Application error rate is 5%
- Lambda function duration is 3 seconds
```

**CloudTrail:**
```
"Who did what and when?"
- User Alice terminated EC2 instance i-123 at 2PM
- Role MyRole created S3 bucket at 3PM
- API call DeleteBucket was denied at 4PM
```

**Config:**
```
"How has my configuration changed?"
- EC2 instance i-123 changed from t3.micro to t3.large
- Security group sg-123 port 22 was opened to 0.0.0.0/0
- S3 bucket encryption was disabled
```

**X-Ray:**
```
"Where is my application slow?"
- Request took 5 seconds total
  - API Gateway: 100ms
  - Lambda: 200ms
  - DynamoDB: 50ms
  - External API: 4,650ms (bottleneck!)
```

**Combined Use Case:**
```
Scenario: Application performance issue

1. CloudWatch Alarm triggers (high latency)
2. X-Ray shows bottleneck in specific service
3. CloudTrail shows recent config change by user
4. Config shows security group was modified
5. Rollback change, performance restored
```

---

## 9. MIGRATION & TRANSFER

### 9.1 DataSync vs Storage Gateway vs Snow Family

| Feature | DataSync | Storage Gateway | Snow Family |
|---------|----------|-----------------|-------------|
| **Purpose** | Data transfer automation | Hybrid storage | Physical data transfer |
| **Connection** | Over network | Over network | Physical device |
| **Bandwidth** | Up to 10 Gbps | Depends on connection | N/A (offline) |
| **Use Case** | One-time/periodic migration | Continuous hybrid access | Large-scale migration (TBs-PBs) |
| **Pricing** | $0.0125/GB | Gateway + storage costs | Device rental + shipping |

**Snow Family Comparison:**

| Device | Capacity | Use Case | Compute | Price |
|--------|----------|----------|---------|-------|
| **Snowcone** | 8 TB HDD / 14 TB SSD | Edge computing, small transfers | 2 vCPUs, 4 GB RAM | $200 + $0.03/GB |
| **Snowball Edge Storage** | 80 TB | Large data migrations | 40 vCPUs, 80 GB RAM | $300 + $0.03/GB |
| **Snowball Edge Compute** | 42 TB + 7.68 TB SSD | Compute + storage | 52 vCPUs, 208 GB RAM, GPU | $300 + $0.03/GB |
| **Snowmobile** | 100 PB | Datacenter migration | N/A | Custom pricing |

**Decision Matrix:**
```
< 10 TB over good internet:
→ DataSync (10x faster than open-source tools)

10-80 TB:
→ Snowball Edge (1 week transfer time)

> 80 TB:
→ Multiple Snowballs or Snowmobile

Ongoing hybrid access:
→ Storage Gateway (File/Volume/Tape)

Edge computing + data transfer:
→ Snowcone or Snowball Edge Compute
```

---

## 10. DEVELOPER TOOLS

### 10.1 CodeCommit vs CodeBuild vs CodeDeploy vs CodePipeline

| Service | Purpose | Type | Equivalent |
|---------|---------|------|------------|
| **CodeCommit** | Source control | Git repository | GitHub, GitLab |
| **CodeBuild** | Build & test | CI service | Jenkins, Travis CI |
| **CodeDeploy** | Deployment | CD service | Jenkins, Octopus Deploy |
| **CodePipeline** | Orchestration | CI/CD pipeline | Jenkins Pipeline, GitLab CI |

**CI/CD Pipeline Flow:**
```
1. CodeCommit (Source)
   ↓
2. CodeBuild (Build & Test)
   ↓
3. CodeDeploy (Deploy)
   ↓
4. Production

Orchestrated by: CodePipeline
```

**CodeDeploy Deployment Types:**

| Type | Description | Downtime | Rollback | Use Case |
|------|-------------|----------|----------|----------|
| **In-Place** | Update existing instances | ✅ Yes | Manual | Simple updates |
| **Blue/Green** | New instances, switch traffic | ❌ No | Instant | Zero-downtime |
| **Canary** | Gradual traffic shift | ❌ No | Instant | Risk mitigation |
| **Linear** | Incremental traffic shift | ❌ No | Instant | Controlled rollout |

---

## ✅ Summary

This comprehensive comparison covers **80+ AWS services** across **10 categories**:

1. ✅ Compute (EC2, Lambda, Fargate, ECS, EKS)
2. ✅ Storage (S3, EBS, EFS, FSx)
3. ✅ Database (RDS, Aurora, DynamoDB, Redshift, ElastiCache)
4. ✅ Networking (ALB, NLB, CloudFront, Global Accelerator, Route 53, VPC)
5. ✅ Security (Security Groups, NACLs, IAM, KMS, CloudHSM)
6. ✅ Integration (SQS, SNS, EventBridge, Kinesis)
7. ✅ Analytics (Athena, Redshift, EMR, Kinesis)
8. ✅ Management (CloudWatch, CloudTrail, Config, X-Ray)
9. ✅ Migration (DataSync, Storage Gateway, Snow Family)
10. ✅ Developer Tools (CodeCommit, CodeBuild, CodeDeploy, CodePipeline)

### How to Use These Tables in the Exam

1. **Identify service category** from the question
2. **Find the comparison table** for that category
3. **Match requirements** to service features
4. **Eliminate wrong answers** using the decision matrix
5. **Select the best fit** based on use case

**Example:**
> Question: "Application needs message buffering with guaranteed order and no duplicates"
> 
> Category: Application Integration
> Table: SQS Standard vs FIFO
> Match: "guaranteed order" + "no duplicates" = FIFO
> Answer: SQS FIFO Queue

---

*Học thuộc các bảng so sánh này để chọn đúng service trong < 30 giây!*
