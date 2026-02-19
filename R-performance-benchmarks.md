# ⚡ R. PERFORMANCE BENCHMARKS

> **Detailed performance metrics, limits, and benchmarks for AWS compute and database services**

---

## 📋 Mục Lục

1. [EC2 Instance Performance](#1-ec2-instance-performance)
2. [Lambda Performance](#2-lambda-performance)
3. [Container Services Performance](#3-container-services-performance)
4. [RDS Performance](#4-rds-performance)
5. [Aurora Performance](#5-aurora-performance)
6. [DynamoDB Performance](#6-dynamodb-performance)
7. [ElastiCache Performance](#7-elasticache-performance)
8. [Redshift Performance](#8-redshift-performance)
9. [Storage Performance](#9-storage-performance)
10. [Network Performance](#10-network-performance)

---

## 1. EC2 INSTANCE PERFORMANCE

### 1.1 Compute Performance by Instance Family

| Instance Type | vCPUs | Memory (GB) | Network (Gbps) | EBS Bandwidth (Mbps) | Baseline Performance | Benchmark Score* |
|---------------|-------|-------------|----------------|----------------------|---------------------|------------------|
| **t3.nano** | 2 | 0.5 | Up to 5 | Up to 2,085 | 5% CPU | 150 |
| **t3.micro** | 2 | 1 | Up to 5 | Up to 2,085 | 10% CPU | 300 |
| **t3.small** | 2 | 2 | Up to 5 | Up to 2,085 | 20% CPU | 600 |
| **t3.medium** | 2 | 4 | Up to 5 | Up to 2,085 | 20% CPU | 600 |
| **t3.large** | 2 | 8 | Up to 5 | Up to 2,780 | 30% CPU | 900 |
| **m5.large** | 2 | 8 | Up to 10 | 4,750 | 100% CPU | 2,000 |
| **m5.xlarge** | 4 | 16 | Up to 10 | 4,750 | 100% CPU | 4,000 |
| **m5.2xlarge** | 8 | 32 | Up to 10 | 4,750 | 100% CPU | 8,000 |
| **m5.4xlarge** | 16 | 64 | Up to 10 | 4,750 | 100% CPU | 16,000 |
| **c5.large** | 2 | 4 | Up to 10 | 4,750 | 100% CPU | 2,500 |
| **c5.xlarge** | 4 | 8 | Up to 10 | 4,750 | 100% CPU | 5,000 |
| **c5.2xlarge** | 8 | 16 | Up to 10 | 4,750 | 100% CPU | 10,000 |
| **c5.4xlarge** | 16 | 32 | Up to 10 | 4,750 | 100% CPU | 20,000 |
| **r5.large** | 2 | 16 | Up to 10 | 4,750 | 100% CPU | 2,000 |
| **r5.xlarge** | 4 | 32 | Up to 10 | 4,750 | 100% CPU | 4,000 |
| **r5.2xlarge** | 8 | 64 | Up to 10 | 4,750 | 100% CPU | 8,000 |

*Benchmark score: Relative compute performance (higher = better)

### 1.2 T3 Burstable Performance Details

| Instance | Baseline | CPU Credits/Hour | Max Credits | Burst Duration (at 100%) |
|----------|----------|------------------|-------------|--------------------------|
| **t3.nano** | 5% | 3 | 72 | 24 minutes |
| **t3.micro** | 10% | 6 | 144 | 24 minutes |
| **t3.small** | 20% | 12 | 288 | 24 minutes |
| **t3.medium** | 20% | 24 | 576 | 24 minutes |
| **t3.large** | 30% | 36 | 864 | 24 minutes |
| **t3.xlarge** | 40% | 96 | 2,304 | 24 minutes |
| **t3.2xlarge** | 40% | 192 | 4,608 | 24 minutes |

**CPU Credits Explained:**
```
1 CPU Credit = 1 vCPU at 100% for 1 minute

Example (t3.micro):
- Baseline: 10% of 2 vCPUs = 0.2 vCPU always available
- Earns: 6 credits/hour
- Max balance: 144 credits
- Burst: Can use 2 vCPUs at 100% for 24 minutes (144 credits ÷ 6 credits/min)

Scenario:
- Normal load (10%): Accumulates credits
- Spike (100%): Uses credits, can sustain for 24 minutes
- After burst: Falls back to baseline (10%)
```

### 1.3 Network Performance Benchmarks

| Instance Type | Network Bandwidth | Packets Per Second (PPS) | Enhanced Networking |
|---------------|-------------------|--------------------------|---------------------|
| **t3.nano - t3.medium** | Up to 5 Gbps | ~300,000 | ✅ Yes |
| **t3.large - t3.2xlarge** | Up to 5 Gbps | ~500,000 | ✅ Yes |
| **m5.large** | Up to 10 Gbps | ~800,000 | ✅ Yes |
| **m5.4xlarge** | Up to 10 Gbps | ~1,000,000 | ✅ Yes |
| **m5.8xlarge** | 10 Gbps | ~1,200,000 | ✅ Yes |
| **m5.12xlarge** | 12 Gbps | ~1,500,000 | ✅ Yes |
| **m5.16xlarge** | 20 Gbps | ~2,000,000 | ✅ Yes |
| **m5.24xlarge** | 25 Gbps | ~3,000,000 | ✅ Yes |
| **c5n.18xlarge** | 100 Gbps | ~15,000,000 | ✅ Yes (EFA) |

---

## 2. LAMBDA PERFORMANCE

### 2.1 Lambda Performance by Memory Configuration

| Memory (MB) | vCPU (approx) | Cold Start (ms) | Warm Execution (ms) | Cost per 1M requests* |
|-------------|---------------|-----------------|---------------------|----------------------|
| **128** | 0.08 | 800-1,200 | 5-50 | $0.20 + $0.21 |
| **256** | 0.17 | 600-900 | 5-50 | $0.20 + $0.42 |
| **512** | 0.33 | 400-700 | 5-50 | $0.20 + $0.83 |
| **1,024** | 0.67 | 300-500 | 5-50 | $0.20 + $1.67 |
| **1,792** | 1.00 | 250-400 | 5-50 | $0.20 + $2.92 |
| **3,008** | 1.67 | 200-350 | 5-50 | $0.20 + $5.00 |
| **5,120** | 2.67 | 180-300 | 5-50 | $0.20 + $8.50 |
| **10,240** | 6.00 | 150-250 | 5-50 | $0.20 + $17.00 |

*Assuming 100ms execution time

### 2.2 Lambda Limits and Quotas

| Limit | Value | Can Increase? |
|-------|-------|---------------|
| **Max execution time** | 15 minutes (900 seconds) | ❌ No |
| **Memory allocation** | 128 MB - 10,240 MB (1 MB increments) | ❌ No |
| **Deployment package size** | 50 MB (zipped), 250 MB (unzipped) | ❌ No |
| **/tmp storage** | 512 MB - 10,240 MB | ❌ No |
| **Concurrent executions** | 1,000 (default) | ✅ Yes |
| **Function environment variables** | 4 KB total | ❌ No |
| **Layers** | 5 per function | ❌ No |
| **Invocation payload** | 6 MB (synchronous), 256 KB (async) | ❌ No |

### 2.3 Lambda Cold Start Benchmarks

| Runtime | Package Size | Cold Start (ms) | Warm Start (ms) |
|---------|--------------|-----------------|-----------------|
| **Node.js 18** | 1 MB | 150-300 | 1-5 |
| **Node.js 18** | 10 MB | 300-500 | 1-5 |
| **Python 3.11** | 1 MB | 200-400 | 1-5 |
| **Python 3.11** | 10 MB | 400-700 | 1-5 |
| **Java 17** | 5 MB | 800-1,500 | 1-10 |
| **Java 17** | 50 MB | 2,000-4,000 | 1-10 |
| **.NET 6** | 5 MB | 600-1,000 | 1-10 |
| **Go 1.x** | 5 MB | 100-250 | 1-5 |

**Optimization Tips:**
```
Reduce Cold Starts:
✅ Use smaller deployment packages
✅ Minimize dependencies
✅ Use Lambda Layers for common code
✅ Enable Provisioned Concurrency (keeps functions warm)
✅ Choose faster runtimes (Go, Node.js)
✅ Avoid Java for latency-sensitive workloads
```

---

## 3. CONTAINER SERVICES PERFORMANCE

### 3.1 ECS Fargate Performance

| CPU (vCPU) | Memory Options (GB) | Task Startup Time | Cost per Hour* |
|------------|---------------------|-------------------|----------------|
| **0.25** | 0.5, 1, 2 | 30-60 seconds | $0.012 - $0.024 |
| **0.5** | 1, 2, 3, 4 | 30-60 seconds | $0.024 - $0.048 |
| **1** | 2, 3, 4, 5, 6, 7, 8 | 30-60 seconds | $0.048 - $0.096 |
| **2** | 4-16 (1 GB increments) | 30-60 seconds | $0.096 - $0.192 |
| **4** | 8-30 (1 GB increments) | 30-60 seconds | $0.192 - $0.384 |
| **8** | 16-60 (4 GB increments) | 30-60 seconds | $0.384 - $0.768 |
| **16** | 32-120 (8 GB increments) | 30-60 seconds | $0.768 - $1.536 |

*us-east-1 pricing

### 3.2 ECS vs EKS Performance Comparison

| Metric | ECS on EC2 | ECS on Fargate | EKS on EC2 | EKS on Fargate |
|--------|------------|----------------|------------|----------------|
| **Startup Time** | < 1 minute | 30-60 seconds | < 1 minute | 30-60 seconds |
| **Scaling Speed** | < 1 minute | 1-2 minutes | < 1 minute | 1-2 minutes |
| **Max Pods/Tasks per Node** | 120 | N/A | 110 | N/A |
| **Overhead** | ~5% (ECS agent) | None | ~10% (kubelet) | None |
| **Network Performance** | Instance-dependent | Up to 10 Gbps | Instance-dependent | Up to 10 Gbps |

---

## 4. RDS PERFORMANCE

### 4.1 RDS Instance Performance by Class

| Instance Class | vCPUs | Memory (GB) | Network (Gbps) | IOPS (Baseline) | Connections | Price/Hour* |
|----------------|-------|-------------|----------------|-----------------|-------------|-------------|
| **db.t3.micro** | 2 | 1 | Up to 5 | 3,000 | 45 | $0.017 |
| **db.t3.small** | 2 | 2 | Up to 5 | 3,000 | 90 | $0.034 |
| **db.t3.medium** | 2 | 4 | Up to 5 | 3,000 | 180 | $0.068 |
| **db.m5.large** | 2 | 8 | Up to 10 | 3,600 | 360 | $0.154 |
| **db.m5.xlarge** | 4 | 16 | Up to 10 | 6,000 | 720 | $0.308 |
| **db.m5.2xlarge** | 8 | 32 | Up to 10 | 12,000 | 1,440 | $0.616 |
| **db.m5.4xlarge** | 16 | 64 | Up to 10 | 18,750 | 2,880 | $1.232 |
| **db.r5.large** | 2 | 16 | Up to 10 | 3,600 | 1,000 | $0.240 |
| **db.r5.xlarge** | 4 | 32 | Up to 10 | 6,000 | 2,000 | $0.480 |
| **db.r5.2xlarge** | 8 | 64 | Up to 10 | 12,000 | 4,000 | $0.960 |
| **db.r5.4xlarge** | 16 | 128 | Up to 10 | 18,750 | 8,000 | $1.920 |

*MySQL/PostgreSQL pricing in us-east-1

### 4.2 RDS Performance Benchmarks (MySQL 8.0)

| Instance | Queries/Second (Read) | Queries/Second (Write) | Transactions/Second | Latency (ms) |
|----------|----------------------|------------------------|---------------------|--------------|
| **db.t3.small** | 500-1,000 | 200-400 | 100-200 | 10-50 |
| **db.t3.medium** | 1,000-2,000 | 400-800 | 200-400 | 5-30 |
| **db.m5.large** | 3,000-5,000 | 1,000-2,000 | 500-1,000 | 3-15 |
| **db.m5.xlarge** | 6,000-10,000 | 2,000-4,000 | 1,000-2,000 | 2-10 |
| **db.m5.2xlarge** | 12,000-20,000 | 4,000-8,000 | 2,000-4,000 | 1-8 |
| **db.r5.large** | 4,000-6,000 | 1,500-2,500 | 700-1,200 | 2-12 |
| **db.r5.xlarge** | 8,000-12,000 | 3,000-5,000 | 1,500-2,500 | 1-8 |
| **db.r5.2xlarge** | 16,000-24,000 | 6,000-10,000 | 3,000-5,000 | 1-5 |

**Benchmark Conditions:**
- Simple SELECT queries (indexed)
- 80% reads, 20% writes
- gp3 storage with 3,000 IOPS
- Multi-AZ disabled

### 4.3 RDS Storage Performance

| Storage Type | IOPS Range | Throughput | Latency | Use Case |
|--------------|------------|------------|---------|----------|
| **gp3** | 3,000-16,000 | 125-1,000 MB/s | Single-digit ms | General purpose |
| **gp2** | 100-16,000 (3 IOPS/GB) | 128-250 MB/s | Single-digit ms | Legacy |
| **io2** | 1,000-256,000 | 1,000-4,000 MB/s | Sub-millisecond | High performance |
| **io1** | 1,000-64,000 | 1,000 MB/s | Single-digit ms | Legacy |
| **Magnetic** | ~100 | ~50 MB/s | Higher | Not recommended |

---

## 5. AURORA PERFORMANCE

### 5.1 Aurora vs RDS Performance Comparison

| Metric | RDS MySQL | Aurora MySQL | Improvement |
|--------|-----------|--------------|-------------|
| **Throughput (reads)** | 10,000 QPS | 50,000 QPS | **5x faster** |
| **Throughput (writes)** | 4,000 QPS | 20,000 QPS | **5x faster** |
| **Replication Lag** | Seconds | < 100ms | **20x faster** |
| **Read Replicas** | 5 max | 15 max | **3x more** |
| **Failover Time** | 60-120 seconds | < 30 seconds | **4x faster** |
| **Storage Scaling** | Manual | Automatic (10 GB increments) | **Automatic** |
| **Max Storage** | 64 TB | 128 TB | **2x larger** |

### 5.2 Aurora Instance Performance

| Instance Class | vCPUs | Memory (GB) | Connections | Price/Hour* | Use Case |
|----------------|-------|-------------|-------------|-------------|----------|
| **db.t3.small** | 2 | 2 | 90 | $0.041 | Dev/test |
| **db.t3.medium** | 2 | 4 | 180 | $0.082 | Small production |
| **db.r5.large** | 2 | 16 | 1,000 | $0.290 | Medium production |
| **db.r5.xlarge** | 4 | 32 | 2,000 | $0.580 | Large production |
| **db.r5.2xlarge** | 8 | 64 | 3,000 | $1.160 | High performance |
| **db.r5.4xlarge** | 16 | 128 | 4,000 | $2.320 | Very high performance |
| **db.r5.12xlarge** | 48 | 384 | 6,000 | $6.960 | Mission-critical |

*Aurora MySQL pricing in us-east-1

### 5.3 Aurora Serverless v2 Performance

| ACU Range | Memory (GB) | Connections | Scaling Time | Price/Hour* |
|-----------|-------------|-------------|--------------|-------------|
| **0.5-1** | 1-2 | 90-180 | < 1 second | $0.06-$0.12 |
| **1-2** | 2-4 | 180-360 | < 1 second | $0.12-$0.24 |
| **2-4** | 4-8 | 360-720 | < 1 second | $0.24-$0.48 |
| **4-8** | 8-16 | 720-1,440 | < 1 second | $0.48-$0.96 |
| **8-16** | 16-32 | 1,440-2,880 | < 1 second | $0.96-$1.92 |
| **16-32** | 32-64 | 2,880-5,760 | < 1 second | $1.92-$3.84 |

*us-east-1 pricing

**Scaling Characteristics:**
```
Scale Up: Instant (< 1 second)
Scale Down: Gradual (after 15 minutes of low activity)
Min ACU: Can set to 0.5 (pause when idle)
Max ACU: Up to 128 ACU
```

---

## 6. DYNAMODB PERFORMANCE

### 6.1 DynamoDB Capacity Units

| Operation | Capacity Unit | Item Size | Operations/Second |
|-----------|---------------|-----------|-------------------|
| **Read (Eventually Consistent)** | 1 RCU | Up to 8 KB | 2 reads/sec |
| **Read (Strongly Consistent)** | 1 RCU | Up to 4 KB | 1 read/sec |
| **Write** | 1 WCU | Up to 1 KB | 1 write/sec |
| **Transactional Read** | 2 RCUs | Up to 4 KB | 1 read/sec |
| **Transactional Write** | 2 WCUs | Up to 1 KB | 1 write/sec |

**Calculation Examples:**
```
Example 1: Read 10 KB item (strongly consistent)
  - 10 KB ÷ 4 KB = 2.5 → round up to 3 RCUs

Example 2: Write 3.5 KB item
  - 3.5 KB ÷ 1 KB = 3.5 → round up to 4 WCUs

Example 3: 100 reads/sec of 5 KB items (eventually consistent)
  - 5 KB ÷ 8 KB = 0.625 → round up to 1 RCU per read
  - 100 reads/sec ÷ 2 reads per RCU = 50 RCUs
```

### 6.2 DynamoDB Performance Benchmarks

| Metric | On-Demand | Provisioned (Auto Scaling) |
|--------|-----------|----------------------------|
| **Max Request Rate** | Unlimited | Based on provisioned capacity |
| **Latency (p50)** | < 10ms | < 10ms |
| **Latency (p99)** | < 20ms | < 20ms |
| **Latency (p99.9)** | < 50ms | < 50ms |
| **Throughput** | Unlimited (throttles if too fast) | Up to 40,000 RCU/WCU per table |
| **Burst Capacity** | Automatic | 300 seconds of unused capacity |

### 6.3 DynamoDB DAX Performance

| Metric | DynamoDB (Direct) | DynamoDB + DAX |
|--------|-------------------|----------------|
| **Read Latency (p50)** | 5-10 ms | **< 1 ms** |
| **Read Latency (p99)** | 20 ms | **< 2 ms** |
| **Throughput** | 40,000 RCU/table | **Millions of requests/sec** |
| **Cache Hit Ratio** | N/A | 80-95% (typical) |
| **Cost** | $0.25/million reads | $0.25/million reads + DAX node cost |

**DAX Node Pricing (us-east-1):**
```
dax.t3.small:  $0.04/hour  (1 vCPU, 1.37 GB)
dax.r5.large:  $0.35/hour  (2 vCPU, 13.07 GB)
dax.r5.xlarge: $0.69/hour  (4 vCPU, 26.32 GB)
```

---

## 7. ELASTICACHE PERFORMANCE

### 7.1 ElastiCache Redis Performance

| Node Type | vCPUs | Memory (GB) | Network (Gbps) | Connections | Ops/Sec | Price/Hour* |
|-----------|-------|-------------|----------------|-------------|---------|-------------|
| **cache.t3.micro** | 2 | 0.5 | Up to 5 | 65,000 | 50,000 | $0.017 |
| **cache.t3.small** | 2 | 1.37 | Up to 5 | 65,000 | 80,000 | $0.034 |
| **cache.t3.medium** | 2 | 3.09 | Up to 5 | 65,000 | 100,000 | $0.068 |
| **cache.m5.large** | 2 | 6.38 | Up to 10 | 65,000 | 150,000 | $0.136 |
| **cache.m5.xlarge** | 4 | 12.93 | Up to 10 | 65,000 | 250,000 | $0.272 |
| **cache.r5.large** | 2 | 13.07 | Up to 10 | 65,000 | 150,000 | $0.188 |
| **cache.r5.xlarge** | 4 | 26.32 | Up to 10 | 65,000 | 300,000 | $0.376 |
| **cache.r5.2xlarge** | 8 | 52.82 | Up to 10 | 65,000 | 500,000 | $0.752 |

*us-east-1 pricing

### 7.2 ElastiCache Memcached Performance

| Node Type | vCPUs | Memory (GB) | Network (Gbps) | Connections | Ops/Sec | Price/Hour* |
|-----------|-------|-------------|----------------|-------------|---------|-------------|
| **cache.t3.micro** | 2 | 0.5 | Up to 5 | 65,000 | 100,000 | $0.017 |
| **cache.t3.small** | 2 | 1.37 | Up to 5 | 65,000 | 150,000 | $0.034 |
| **cache.t3.medium** | 2 | 3.09 | Up to 5 | 65,000 | 200,000 | $0.068 |
| **cache.m5.large** | 2 | 6.38 | Up to 10 | 65,000 | 300,000 | $0.136 |
| **cache.r5.large** | 2 | 13.07 | Up to 10 | 65,000 | 300,000 | $0.188 |

*us-east-1 pricing

**Redis vs Memcached Performance:**
```
Redis (Single-threaded):
  - ~100,000 ops/sec per core
  - Better for complex operations
  - Supports persistence

Memcached (Multi-threaded):
  - ~200,000 ops/sec per node (multi-core)
  - Better for simple GET/SET
  - No persistence
```

---

## 8. REDSHIFT PERFORMANCE

### 8.1 Redshift Node Types

| Node Type | vCPUs | Memory (GB) | Storage | Slices | Price/Hour* | Price/TB/Year |
|-----------|-------|-------------|---------|--------|-------------|---------------|
| **dc2.large** | 2 | 15 | 160 GB SSD | 2 | $0.25 | $1,000 |
| **dc2.8xlarge** | 32 | 244 | 2.56 TB SSD | 16 | $4.80 | $1,000 |
| **ra3.xlplus** | 4 | 32 | Managed | 2 | $1.086 | $1,280 |
| **ra3.4xlarge** | 12 | 96 | Managed | 4 | $3.26 | $1,280 |
| **ra3.16xlarge** | 48 | 384 | Managed | 16 | $13.04 | $1,280 |

*us-east-1 pricing

### 8.2 Redshift Performance Benchmarks

| Cluster Size | Nodes | Total vCPUs | Storage | Queries/Hour | Concurrent Queries | Scan Rate (GB/s) |
|--------------|-------|-------------|---------|--------------|-------------------|------------------|
| **Small** | 2 dc2.large | 4 | 320 GB | 100-500 | 5-10 | 0.5 |
| **Medium** | 4 ra3.xlplus | 16 | 4 TB | 500-2,000 | 10-20 | 2 |
| **Large** | 8 ra3.4xlarge | 96 | 32 TB | 2,000-10,000 | 20-50 | 8 |
| **X-Large** | 16 ra3.16xlarge | 768 | 512 TB | 10,000+ | 50-200 | 64 |

**Query Performance (TPC-H 3TB benchmark):**
```
Query Type: Complex analytical query with joins
Data Size: 3 TB
Cluster: 8-node ra3.4xlarge

Results:
- Simple aggregation: < 1 second
- Complex join (5 tables): 5-15 seconds
- Full table scan: 30-60 seconds
- Complex analytical query: 1-5 minutes
```

---

## 9. STORAGE PERFORMANCE

### 9.1 EBS Performance Comparison

| Volume Type | IOPS (Max) | Throughput (Max) | Latency | Durability | Price/GB* |
|-------------|------------|------------------|---------|------------|-----------|
| **gp3** | 16,000 | 1,000 MB/s | Single-digit ms | 99.8-99.9% | $0.08 |
| **gp2** | 16,000 | 250 MB/s | Single-digit ms | 99.8-99.9% | $0.10 |
| **io2 Block Express** | 256,000 | 4,000 MB/s | Sub-ms | 99.999% | $0.125 + IOPS |
| **io2** | 64,000 | 1,000 MB/s | Single-digit ms | 99.999% | $0.125 + IOPS |
| **st1** | 500 | 500 MB/s | Higher | 99.8-99.9% | $0.045 |
| **sc1** | 250 | 250 MB/s | Higher | 99.8-99.9% | $0.015 |

*us-east-1 pricing

### 9.2 S3 Performance

| Metric | Value | Notes |
|--------|-------|-------|
| **Request Rate (GET/HEAD)** | 5,500 req/sec per prefix | Can scale by adding prefixes |
| **Request Rate (PUT/COPY/POST/DELETE)** | 3,500 req/sec per prefix | Can scale by adding prefixes |
| **Bandwidth** | Unlimited | No throttling on bandwidth |
| **Latency (first byte)** | 100-200 ms | Typical for Standard class |
| **Throughput (single object)** | Up to 100 MB/s | Per connection |
| **Multipart Upload** | Up to 5 GB/s | Parallel uploads |

**S3 Performance Optimization:**
```
Single Prefix:
  - 5,500 GET/sec
  - 3,500 PUT/sec

10 Prefixes:
  - 55,000 GET/sec
  - 35,000 PUT/sec

100 Prefixes:
  - 550,000 GET/sec
  - 350,000 PUT/sec
```

### 9.3 EFS Performance

| Performance Mode | Throughput | IOPS | Latency | Use Case |
|------------------|------------|------|---------|----------|
| **General Purpose** | Up to 7,000 ops/sec | 7,000+ | Low | Web serving, CMS |
| **Max I/O** | > 7,000 ops/sec | 500,000+ | Higher | Big data, media |

**EFS Throughput Modes:**

| Mode | Throughput | Scaling | Cost |
|------|------------|---------|------|
| **Bursting** | 50 MB/s per TB (baseline), 100 MB/s burst | Scales with size | Standard |
| **Provisioned** | 1-1,024 MB/s (independent of size) | Fixed | Additional charge |
| **Elastic** | Scales automatically | Automatic | Highest per GB |

---

## 10. NETWORK PERFORMANCE

### 10.1 Load Balancer Performance

| Load Balancer | Max Throughput | Latency | Connections/Sec | Price* |
|---------------|----------------|---------|-----------------|--------|
| **ALB** | ~100,000 req/sec | 50-100 ms | ~10,000 | $0.0225/hour + $0.008/LCU |
| **NLB** | Millions req/sec | < 100 μs | ~100,000 | $0.0225/hour + $0.006/NLCU |
| **CLB** | ~10,000 req/sec | Higher | ~1,000 | $0.025/hour + $0.008/GB |

*us-east-1 pricing

### 10.2 CloudFront Performance

| Metric | Value |
|--------|-------|
| **Edge Locations** | 600+ worldwide |
| **Cache Hit Ratio** | 80-95% (typical) |
| **Latency Reduction** | 50-90% (vs origin) |
| **Throughput** | Unlimited |
| **Max Object Size** | 30 GB |
| **TTL** | 0 seconds - 1 year |

**Performance by Region:**
```
North America: 20-50 ms
Europe: 30-80 ms
Asia: 50-150 ms
South America: 100-200 ms
Africa: 150-300 ms
```

---

## ✅ Performance Optimization Checklist

### Compute Optimization
- [ ] Choose right instance family (T3 for burstable, M5 for general, C5 for compute, R5 for memory)
- [ ] Use Enhanced Networking for high throughput
- [ ] Enable Placement Groups for HPC workloads
- [ ] Use Lambda Provisioned Concurrency to eliminate cold starts

### Database Optimization
- [ ] Choose appropriate instance size based on workload
- [ ] Use Read Replicas for read-heavy workloads
- [ ] Enable Multi-AZ for high availability
- [ ] Use Aurora for 5x better performance than RDS
- [ ] Implement DynamoDB DAX for microsecond latency

### Storage Optimization
- [ ] Use gp3 for general purpose (better than gp2)
- [ ] Use io2 Block Express for > 16,000 IOPS
- [ ] Enable S3 Transfer Acceleration for global uploads
- [ ] Use CloudFront for static content delivery
- [ ] Implement multipart upload for large files

### Network Optimization
- [ ] Use NLB for ultra-low latency (< 100 μs)
- [ ] Enable cross-zone load balancing
- [ ] Use CloudFront for global content delivery
- [ ] Implement VPC endpoints for private connectivity
- [ ] Use Direct Connect for consistent network performance

---

*Sử dụng các benchmarks này để chọn đúng instance size và optimize performance!*
