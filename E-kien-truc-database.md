# 🗄️ E. KIẾN TRÚC DATABASE

> **Mục tiêu:** Chọn đúng database cho đúng use case

## 📋 Database Decision Tree

```
Data structure?
│
├─ Relational (SQL)
│  ├─ Need high availability → RDS Multi-AZ
│  ├─ Global distribution → Aurora Global Database
│  ├─ Serverless → Aurora Serverless
│  └─ Migration → DMS
│
├─ Key-Value / Document (NoSQL)
│  ├─ Millisecond latency → DynamoDB
│  ├─ Microsecond latency → DynamoDB + DAX
│  └─ Flexible schema → DocumentDB
│
├─ In-Memory Caching
│  ├─ Complex data structures → ElastiCache Redis
│  └─ Simple caching → ElastiCache Memcached
│
├─ Graph
│  └─ Social networks, fraud detection → Neptune
│
├─ Time-Series
│  └─ IoT, metrics → Timestream
│
├─ Ledger (Immutable)
│  └─ Financial, supply chain → QLDB
│
└─ Data Warehouse
   └─ Analytics, BI → Redshift
```

---

## 🐘 Amazon RDS

### RDS Database Engines

| Engine | Use Case | Max Storage | Special Features |
|--------|----------|-------------|------------------|
| **MySQL** | General purpose | 64 TB | Most popular |
| **PostgreSQL** | Advanced features | 64 TB | JSON support, full-text search |
| **MariaDB** | MySQL fork | 64 TB | Open-source |
| **Oracle** | Enterprise | 64 TB | BYOL available |
| **SQL Server** | Microsoft stack | 16 TB | Windows integration |
| **Aurora** | AWS optimized | 128 TB | 5x faster than MySQL |

### RDS Deployment Options

#### **Single-AZ**
```
Primary DB in AZ-A
└─ Read Replicas (optional)
```

**Use Case:** Dev/test environments

#### **Multi-AZ**
```
Primary DB (AZ-A) ←→ Standby DB (AZ-B)
                 (Synchronous replication)
```

**Features:**
- ✅ Automatic failover (1-2 minutes)
- ✅ Synchronous replication
- ✅ Same endpoint (no app changes)
- ✅ Standby không serve read traffic

**Use Case:** Production databases

**Ví dụ trong đề thi:**
> **Q:** RDS database cần automatic failover với minimal downtime. Giải pháp nào?
> 
> ✅ **RDS Multi-AZ** (1-2 min failover)

#### **Read Replicas**
```
Primary DB (Write)
├─ Read Replica 1 (Read)
├─ Read Replica 2 (Read)
└─ Read Replica 3 (Read)
```

**Features:**
- ✅ Asynchronous replication
- ✅ Up to 15 read replicas (Aurora)
- ✅ Can be in different regions
- ✅ Can be promoted to primary

**Use Cases:**
- ✅ Scale read workloads
- ✅ Analytics queries (không ảnh hưởng primary)
- ✅ Cross-region disaster recovery

**Ví dụ trong đề thi:**
> **Q:** Application có heavy read traffic. Cách nào scale reads without affecting primary?
> 
> ✅ **RDS Read Replicas** (offload read traffic)

### RDS Backup & Restore

**Automated Backups:**
- ✅ Daily full backup
- ✅ Transaction logs every 5 minutes
- ✅ Retention: 0-35 days (default 7)
- ✅ Point-in-time recovery (PITR)

**Manual Snapshots:**
- ✅ User-initiated
- ✅ Retention: Forever (until deleted)
- ✅ Can copy across regions

**Restore:**
- ❌ Cannot restore to existing DB
- ✅ Creates new DB instance

### RDS Encryption

**At Rest:**
- ✅ KMS encryption
- ✅ Must enable at creation
- ✅ Cannot encrypt existing unencrypted DB

**To encrypt existing DB:**
```
1. Create snapshot of unencrypted DB
2. Copy snapshot with encryption enabled
3. Restore from encrypted snapshot
4. Switch application to new DB
```

**In Transit:**
- ✅ SSL/TLS certificates
- ✅ Force SSL in parameter group

---

## ⚡ Amazon Aurora

### Aurora Overview

**Advantages over RDS:**
- ✅ 5x faster than MySQL, 3x faster than PostgreSQL
- ✅ Up to 15 read replicas (RDS: 5)
- ✅ Auto-scaling storage (10 GB → 128 TB)
- ✅ 6 copies across 3 AZs
- ✅ Self-healing storage
- ✅ Automated backups to S3

### Aurora High Availability

**Storage Architecture:**
```
6 copies of data across 3 AZs
├─ AZ-A: 2 copies
├─ AZ-B: 2 copies
└─ AZ-C: 2 copies

Quorum:
- Write: 4/6 copies
- Read: 3/6 copies
```

**Failover:**
- ✅ Primary failure → Replica promoted (< 30 seconds)
- ✅ No replicas → New primary created (longer)

### Aurora Endpoints

| Endpoint | Purpose | Routing |
|----------|---------|---------|
| **Cluster (Writer)** | Write operations | Primary instance |
| **Reader** | Read operations | Load-balanced across replicas |
| **Custom** | Specific subset | Custom logic |

### Aurora Serverless

**Use Cases:**
- ✅ Infrequent, intermittent workloads
- ✅ Unpredictable workloads
- ✅ Development/test databases
- ✅ Multi-tenant applications

**Features:**
- ✅ Auto-scaling based on load
- ✅ Pay per second
- ✅ No capacity planning
- ✅ Pause when inactive (save cost)

**Ví dụ trong đề thi:**
> **Q:** Database cho dev/test với unpredictable usage. Cách nào cost-effective nhất?
> 
> ✅ **Aurora Serverless** (auto-scales, pauses when idle)

### Aurora Global Database

**Architecture:**
```
Primary Region (us-east-1)
├─ Primary DB
└─ Read Replicas

Secondary Region (eu-west-1)
└─ Read Replicas (up to 16)
```

**Features:**
- ✅ < 1 second replication lag
- ✅ Up to 5 secondary regions
- ✅ Promote secondary in < 1 minute (DR)
- ✅ Read replicas in secondary regions

**Use Cases:**
- ✅ Global applications
- ✅ Disaster recovery
- ✅ Low-latency reads globally

**Ví dụ trong đề thi:**
> **Q:** Application needs low-latency reads globally với disaster recovery capability. Giải pháp nào?
> 
> ✅ **Aurora Global Database** (< 1 sec replication, global reads)

---

## ⚡ Amazon DynamoDB

### DynamoDB Overview

**Characteristics:**
- ✅ Fully managed NoSQL
- ✅ Single-digit millisecond latency
- ✅ Auto-scaling
- ✅ Multi-AZ, multi-region replication
- ✅ Serverless (no servers to manage)

### DynamoDB Capacity Modes

| Mode | Billing | Use Case | Cost |
|------|---------|----------|------|
| **On-Demand** | Per request | Unpredictable traffic | Higher per request |
| **Provisioned** | Per hour | Predictable traffic | Lower per request |

**Provisioned Capacity:**
- 1 RCU (Read Capacity Unit) = 1 strongly consistent read/sec (4 KB)
- 1 WCU (Write Capacity Unit) = 1 write/sec (1 KB)

**Auto Scaling:**
- ✅ Automatically adjust RCU/WCU based on traffic
- ✅ Set target utilization (e.g., 70%)

### DynamoDB Indexes

#### **Global Secondary Index (GSI)**
- ✅ Different partition key + sort key
- ✅ Eventually consistent
- ✅ Can add after table creation
- ✅ Has own RCU/WCU

#### **Local Secondary Index (LSI)**
- ✅ Same partition key, different sort key
- ✅ Strongly or eventually consistent
- ✅ Must create at table creation
- ✅ Shares RCU/WCU with table

**Ví dụ trong đề thi:**
> **Q:** DynamoDB table cần query by different attribute. Index nào phù hợp?
> 
> ✅ **GSI** (different partition key, can add anytime)

### DynamoDB Accelerator (DAX)

**Purpose:** Microsecond latency caching layer

**Features:**
- ✅ 10x performance improvement
- ✅ Microsecond latency (vs millisecond)
- ✅ No application code changes
- ✅ Fully managed

**Use Cases:**
- ✅ Read-heavy workloads
- ✅ Gaming leaderboards
- ✅ Real-time bidding

**Ví dụ trong đề thi:**
> **Q:** DynamoDB application cần reduce latency from milliseconds to microseconds. Giải pháp nào?
> 
> ✅ **DynamoDB DAX** (microsecond latency)

### DynamoDB Global Tables

**Purpose:** Multi-region, multi-active database

**Features:**
- ✅ Active-active replication
- ✅ < 1 second replication
- ✅ Read/write in any region
- ✅ Automatic conflict resolution

**Use Cases:**
- ✅ Global applications
- ✅ Disaster recovery
- ✅ Low-latency access globally

---

## 🔴 Amazon ElastiCache

### Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Data Types** | Strings, Lists, Sets, Sorted Sets, Hashes | Strings only |
| **Persistence** | ✅ Yes (AOF, RDB) | ❌ No |
| **Replication** | ✅ Multi-AZ with failover | ❌ No |
| **Backup/Restore** | ✅ Yes | ❌ No |
| **Pub/Sub** | ✅ Yes | ❌ No |
| **Lua Scripting** | ✅ Yes | ❌ No |
| **Multi-threaded** | ❌ No | ✅ Yes |
| **Use Case** | Advanced features, HA | Simple caching |

**Decision:**
```
Need persistence, replication, complex data? → Redis
Need simple caching, multi-threading? → Memcached
```

**Ví dụ trong đề thi:**
> **Q:** Application cần cache với high availability và automatic failover. Giải pháp nào?
> 
> ✅ **ElastiCache Redis with Multi-AZ**

### ElastiCache Strategies

**Lazy Loading (Cache-Aside):**
```
1. App requests data from cache
2. Cache miss → App reads from DB
3. App writes data to cache
4. Return data to user
```

**Pros:** ✅ Only requested data cached
**Cons:** ❌ Cache miss penalty, stale data

**Write-Through:**
```
1. App writes data to DB
2. App writes data to cache
3. Return success
```

**Pros:** ✅ Always up-to-date
**Cons:** ❌ Write penalty, unused data cached

**Best Practice:** ✅ Combine both strategies

---

## 📊 Amazon Redshift

### Redshift Overview

**Purpose:** Data warehouse for analytics

**Characteristics:**
- ✅ Columnar storage
- ✅ Massively parallel processing (MPP)
- ✅ SQL-based
- ✅ Petabyte-scale
- ✅ 10x faster than traditional data warehouses

### Redshift Architecture

```
Leader Node (SQL endpoint)
│
├─ Compute Node 1
├─ Compute Node 2
└─ Compute Node N
```

**Node Types:**
- **Dense Compute (DC2):** Fast queries, SSD
- **Dense Storage (DS2):** Large data, HDD (legacy)
- **RA3:** Managed storage, scale compute/storage independently

### Redshift Spectrum

**Purpose:** Query data directly in S3 (without loading)

**Use Cases:**
- ✅ Query historical data in S3
- ✅ Extend Redshift to exabytes
- ✅ Data lake analytics

**Ví dụ trong đề thi:**
> **Q:** Analyze petabytes of structured data với SQL queries. Database nào phù hợp?
> 
> ✅ **Amazon Redshift** (data warehouse, petabyte-scale)

---

## 🔄 Database Migration Service (DMS)

### DMS Overview

**Purpose:** Migrate databases to AWS

**Features:**
- ✅ Minimal downtime
- ✅ Continuous replication
- ✅ Supports homogeneous & heterogeneous migrations
- ✅ Schema conversion (AWS SCT)

### Migration Types

| Type | Description | Downtime |
|------|-------------|----------|
| **Full Load** | Migrate all data | High |
| **Full Load + CDC** | Migrate + ongoing changes | Low |
| **CDC Only** | Ongoing replication | Minimal |

**Ví dụ trong đề thi:**
> **Q:** Migrate Oracle database to Aurora với minimal downtime. Giải pháp nào?
> 
> ✅ **AWS DMS with CDC** (continuous replication, minimal downtime)

---

## 🎯 Specialized Databases

### Amazon Neptune (Graph Database)

**Use Cases:**
- Social networks
- Fraud detection
- Knowledge graphs
- Recommendation engines

### Amazon DocumentDB (MongoDB-compatible)

**Use Cases:**
- Content management
- Catalogs
- User profiles

### Amazon Timestream (Time-Series)

**Use Cases:**
- IoT applications
- Operational metrics
- Application monitoring

### Amazon QLDB (Ledger Database)

**Use Cases:**
- Financial transactions
- Supply chain
- Immutable history

---

## ✅ Checklist

- [ ] RDS Multi-AZ vs Read Replicas
- [ ] Aurora vs RDS (5x faster, 15 replicas, 128 TB)
- [ ] Aurora Serverless use cases
- [ ] DynamoDB GSI vs LSI
- [ ] DynamoDB DAX (microsecond latency)
- [ ] ElastiCache Redis vs Memcached
- [ ] Redshift for data warehousing
- [ ] DMS for database migration

*Tiếp theo: [F. Kiến Trúc Mạng](F-kien-truc-mang.md)*
