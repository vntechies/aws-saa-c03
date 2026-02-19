# 🌐 NETWORKING SERVICES - AWS SAA-C03

## 📌 Tổng Quan

Networking là **NỀN TẢNG** của AWS architecture, chiếm **20-25% câu hỏi** trong kỳ thi. Đây là phần **QUAN TRỌNG NHẤT** và **KHÓ NHẤT**.

---

## 1️⃣ AMAZON VPC (Virtual Private Cloud)

### 🎯 Khái Niệm

**VPC** = Isolated virtual network trong AWS. Foundation của mọi kiến trúc.

### 📊 VPC Basics

#### CIDR Block
- **IPv4**: /16 to /28 (65,536 to 16 IPs)
- **Private IP ranges**:
  - 10.0.0.0/8 (10.0.0.0 - 10.255.255.255)
  - 172.16.0.0/12 (172.16.0.0 - 172.31.255.255)
  - 192.168.0.0/16 (192.168.0.0 - 192.168.255.255)

**🎯 Exam Tip**: VPC CIDR không thể thay đổi sau khi tạo (nhưng có thể add thêm secondary CIDR)

### 🏢 VPC Components

#### 1. **Subnets**
- **Public Subnet**: Có route đến Internet Gateway
- **Private Subnet**: Không có route đến Internet Gateway

**Reserved IPs** (trong mỗi subnet):
- **First IP**: Network address
- **Second IP**: VPC router
- **Third IP**: DNS server
- **Fourth IP**: Reserved (future use)
- **Last IP**: Broadcast address

**Example**: 10.0.0.0/24 (256 IPs)
- Available: 256 - 5 = **251 IPs**

#### 2. **Internet Gateway (IGW)**
- **1 VPC = 1 IGW**
- Enables internet access for public subnets
- **Horizontally scaled**, redundant, HA

#### 3. **NAT Gateway**
- Allows **private subnets** to access internet
- **Managed** by AWS
- **Deployed in public subnet**
- **Elastic IP** required
- **Bandwidth**: 5 Gbps → 100 Gbps (auto-scale)

**NAT Gateway vs NAT Instance**:

| Feature | NAT Gateway | NAT Instance |
|---------|-------------|--------------|
| **Managed** | AWS | Customer |
| **Availability** | HA within AZ | Manual HA |
| **Bandwidth** | 5-100 Gbps | Instance type |
| **Cost** | Higher | Lower |
| **Maintenance** | None | Patching required |

**🎯 Exam Tip**: 
- "Private subnet internet access" = NAT Gateway
- "Cost-effective NAT" = NAT Instance
- "High availability NAT" = NAT Gateway in multiple AZs

#### 4. **Route Tables**
- Set of rules (routes) for traffic direction
- **Main route table**: Default for all subnets
- **Custom route tables**: Explicit subnet association

**Example Routes**:
```
Destination         Target
10.0.0.0/16        local (VPC CIDR)
0.0.0.0/0          igw-xxx (Internet Gateway)
```

### 🔒 VPC Security

#### 1. **Security Groups** (Stateful)

**Characteristics**:
- **Stateful**: Return traffic automatically allowed
- **Instance level** (ENI level)
- **Allow rules only** (no deny)
- **Default**: Deny all inbound, allow all outbound

**Example**:
```
Inbound:
- Type: HTTP, Port: 80, Source: 0.0.0.0/0
- Type: SSH, Port: 22, Source: 10.0.0.0/16

Outbound:
- All traffic allowed (default)
```

#### 2. **Network ACLs (NACLs)** (Stateless)

**Characteristics**:
- **Stateless**: Must explicitly allow return traffic
- **Subnet level**
- **Allow and Deny rules**
- **Rule numbers**: 1-32766 (lower = higher priority)
- **Default NACL**: Allow all inbound/outbound

**Example**:
```
Inbound:
100 - Allow HTTP (80) from 0.0.0.0/0
200 - Allow HTTPS (443) from 0.0.0.0/0
* - Deny all

Outbound:
100 - Allow all traffic to 0.0.0.0/0
* - Deny all
```

#### 🔄 Security Groups vs NACLs

| Feature | Security Groups | NACLs |
|---------|----------------|-------|
| **Level** | Instance (ENI) | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow + Deny |
| **Processing** | All rules evaluated | Rules in order |
| **Default** | Deny all inbound | Allow all |

**🎯 Exam Tip**: 
- "Block specific IP" = NACL (deny rule)
- "Instance-level security" = Security Group
- "Subnet-level security" = NACL
- "Stateful" = Security Group
- "Stateless" = NACL

### 🌐 VPC Connectivity

#### 1. **VPC Peering**
- **Private connection** between 2 VPCs
- **Non-transitive**: A-B, B-C ≠ A-C
- **Cross-account, cross-region** supported
- **No overlapping CIDR**

**Limitations**:
- No transitive peering
- No edge-to-edge routing

#### 2. **VPC Endpoints**

**Purpose**: Private access to AWS services (không qua internet)

**Types**:

##### a) **Interface Endpoint** (PrivateLink)
- **ENI** with private IP
- **Powered by AWS PrivateLink**
- **Supports**: Most AWS services
- **Cost**: Per hour + per GB

##### b) **Gateway Endpoint**
- **Route table entry**
- **Free**
- **Supports**: S3, DynamoDB only

**🎯 Exam Tip**: 
- "Private access to S3/DynamoDB" = Gateway Endpoint (free)
- "Private access to other services" = Interface Endpoint
- "PrivateLink" = Interface Endpoint

#### 3. **AWS Transit Gateway**
- **Hub-and-spoke** topology
- Connect thousands of VPCs and on-premises
- **Transitive routing**
- **Cross-region** peering

**Use case**: 
- Complex network topologies
- Centralized routing
- Simplify VPC peering mesh

**🎯 Exam Tip**: 
- "Connect many VPCs" + "transitive" = Transit Gateway
- "Hub-and-spoke" = Transit Gateway

#### 4. **VPN (Virtual Private Network)**

##### a) **Site-to-Site VPN**
- Connect on-premises to AWS
- **Components**:
  - **Virtual Private Gateway (VGW)**: AWS side
  - **Customer Gateway (CGW)**: On-premises device
- **2 tunnels**: High availability
- **Encryption**: IPsec
- **Bandwidth**: Up to 1.25 Gbps per tunnel

##### b) **Client VPN**
- Remote users connect to VPC
- **OpenVPN-based**

**🎯 Exam Tip**: 
- "On-premises to AWS" + "encrypted" = Site-to-Site VPN
- "Remote users" = Client VPN

#### 5. **AWS Direct Connect (DX)**
- **Dedicated physical connection** to AWS
- **Private, high bandwidth, low latency**
- **Not encrypted by default** (use VPN over DX)
- **Speeds**: 50 Mbps - 100 Gbps

**Types**:
- **Dedicated Connection**: 1, 10, 100 Gbps
- **Hosted Connection**: 50 Mbps - 10 Gbps

**Direct Connect Gateway**: Connect to multiple VPCs in different regions

**🎯 Exam Tip**: 
- "High bandwidth" + "low latency" + "on-premises" = Direct Connect
- "Private connection" = Direct Connect
- "Encrypted connection" = VPN or VPN over Direct Connect

#### 🔄 VPN vs Direct Connect

| Feature | VPN | Direct Connect |
|---------|-----|----------------|
| **Setup** | Minutes | Weeks/months |
| **Bandwidth** | Up to 1.25 Gbps/tunnel | 50 Mbps - 100 Gbps |
| **Latency** | Higher (internet) | Lower (dedicated) |
| **Cost** | Low | High |
| **Encryption** | Yes (IPsec) | No (add VPN) |
| **Use case** | Quick setup, backup | High throughput, consistent |

---

## 2️⃣ ELASTIC LOAD BALANCING (ELB)

### 🎯 Khái Niệm

**ELB** = Distribute traffic across multiple targets (EC2, containers, IPs).

### 📊 Load Balancer Types

#### 1. **Application Load Balancer (ALB)** - Layer 7 (HTTP/HTTPS)

**Features**:
- **HTTP/HTTPS** traffic
- **Path-based routing**: /api → target group A, /web → target group B
- **Host-based routing**: api.example.com → A, web.example.com → B
- **Query string/header routing**
- **WebSockets** support
- **HTTP/2** support
- **Target types**: EC2, IP, Lambda, ECS

**Use case**: Web applications, microservices, containers

#### 2. **Network Load Balancer (NLB)** - Layer 4 (TCP/UDP)

**Features**:
- **TCP, UDP, TLS** traffic
- **Ultra-high performance**: Millions requests/second
- **Low latency**: ~100 ms (vs ~400 ms ALB)
- **Static IP**: Elastic IP per AZ
- **Preserve source IP**
- **Target types**: EC2, IP, ALB

**Use case**: Extreme performance, static IP, non-HTTP protocols

#### 3. **Gateway Load Balancer (GWLB)** - Layer 3 (IP)

**Features**:
- **Layer 3** (IP packets)
- **Transparent**: Source/destination unchanged
- **GENEVE protocol** (port 6081)
- **Use case**: Firewalls, IDS/IPS, deep packet inspection

#### 4. **Classic Load Balancer (CLB)** - Legacy

- **Layer 4 & 7**
- **Deprecated**: Use ALB/NLB instead

### 🔄 Load Balancer Comparison

| Feature | ALB | NLB | GWLB |
|---------|-----|-----|------|
| **Layer** | 7 (HTTP/HTTPS) | 4 (TCP/UDP/TLS) | 3 (IP) |
| **Latency** | ~400 ms | ~100 ms | Minimal |
| **Static IP** | No (use NLB) | Yes | No |
| **Preserve source IP** | Via headers | Yes | Yes |
| **WebSockets** | Yes | Yes | No |
| **Path routing** | Yes | No | No |
| **Use case** | Web apps | Performance, static IP | Security appliances |

### 🎯 Load Balancer Features

#### 1. **Cross-Zone Load Balancing**
- Distribute evenly across all AZs
- **ALB**: Enabled by default (free)
- **NLB/GWLB**: Disabled by default (charges for cross-AZ data)

#### 2. **Connection Draining / Deregistration Delay**
- Time to complete in-flight requests before deregistering
- **Default**: 300 seconds
- **Range**: 1-3600 seconds

#### 3. **Health Checks**
- **Protocol**: HTTP, HTTPS, TCP
- **Interval**: 5-300 seconds
- **Timeout**: 2-120 seconds
- **Healthy/Unhealthy threshold**: 2-10 checks

#### 4. **Sticky Sessions (Session Affinity)**
- Route same client to same target
- **Cookie-based**:
  - **Application cookie**: Custom cookie
  - **Duration cookie**: LB-generated cookie
- **Use case**: Stateful applications

#### 5. **SSL/TLS Termination**
- **Decrypt at LB**, forward HTTP to targets
- **Certificates**: ACM (AWS Certificate Manager)
- **SNI** (Server Name Indication): Multiple SSL certs on one LB

**🎯 Exam Tip**: 
- "Layer 7" + "HTTP" = ALB
- "Layer 4" + "TCP" = NLB
- "Static IP" = NLB
- "Extreme performance" = NLB
- "Path-based routing" = ALB
- "Security appliances" = GWLB

---

## 3️⃣ AMAZON ROUTE 53

### 🎯 Khái Niệm

**Route 53** = Managed DNS service (Domain Name System).

### 📊 Route 53 Basics

#### DNS Record Types
- **A**: IPv4 address
- **AAAA**: IPv6 address
- **CNAME**: Canonical name (alias for another domain)
  - **Cannot** create CNAME for zone apex (example.com)
- **ALIAS**: AWS-specific, can point to AWS resources
  - **Can** create for zone apex
  - **Free** queries
  - **Health checks** supported

**🎯 Exam Tip**: 
- "Zone apex" (example.com) → Use ALIAS, not CNAME
- "AWS resource" → ALIAS (free, health checks)

### 🎯 Routing Policies

#### 1. **Simple Routing**
- **Single resource**
- **No health checks**
- **Multiple IPs**: Random selection

#### 2. **Weighted Routing**
- **Traffic distribution** by weight
- **Use case**: A/B testing, gradual migration
- **Example**: 70% to A, 30% to B

#### 3. **Latency-Based Routing**
- Route to **lowest latency** resource
- **Use case**: Global applications

#### 4. **Failover Routing**
- **Active-passive**
- **Primary** and **secondary** resources
- **Health checks** required

#### 5. **Geolocation Routing**
- Route based on **user's location**
- **Use case**: Content localization, compliance

#### 6. **Geoproximity Routing**
- Route based on **geographic proximity**
- **Bias**: Shift traffic (+/- 1 to 99)
- **Requires**: Traffic Flow

#### 7. **Multi-Value Answer Routing**
- Return **multiple IPs** (up to 8)
- **Health checks** for each
- **Not a substitute for ELB**

### 🏥 Route 53 Health Checks

**Types**:
1. **Endpoint Health Check**: Monitor endpoint (IP/domain)
2. **Calculated Health Check**: Combine multiple checks (AND, OR, NOT)
3. **CloudWatch Alarm**: Monitor CloudWatch metrics

**Health Check Features**:
- **Interval**: 30s (standard) or 10s (fast)
- **Threshold**: Number of failures
- **Protocols**: HTTP, HTTPS, TCP
- **String matching**: Check response body

**🎯 Exam Tip**: 
- "Failover" = Failover Routing + Health Checks
- "Lowest latency" = Latency-Based Routing
- "User location" = Geolocation Routing
- "A/B testing" = Weighted Routing

---

## 4️⃣ AMAZON CLOUDFRONT

### 🎯 Khái Niệm

**CloudFront** = Content Delivery Network (CDN), distribute content globally.

### 📊 CloudFront Basics

#### Components
- **Origin**: Source of content (S3, ALB, EC2, custom HTTP)
- **Distribution**: CDN configuration
- **Edge Locations**: Cache content globally (400+ locations)
- **Regional Edge Caches**: Larger cache between origin and edge

### 🎯 CloudFront Features

#### 1. **Origins**

**S3 as Origin**:
- **OAI** (Origin Access Identity): Restrict S3 to CloudFront only
- **OAC** (Origin Access Control): Newer, recommended

**Custom Origin** (HTTP):
- ALB, EC2, on-premises server
- **Must be publicly accessible**

#### 2. **Cache Behaviors**
- **Path patterns**: /images/* → S3, /api/* → ALB
- **TTL**: Time To Live (0 sec - 1 year)
- **Cache keys**: Query strings, headers, cookies

#### 3. **Invalidations**
- Remove cached content before TTL
- **Cost**: First 1,000 paths/month free
- **Paths**: /images/*, /*, /index.html

#### 4. **Geo Restriction**
- **Whitelist**: Allow specific countries
- **Blacklist**: Block specific countries
- **Use case**: Copyright, compliance

#### 5. **Signed URLs / Signed Cookies**
- **Restrict access** to content
- **Signed URL**: Single file
- **Signed Cookie**: Multiple files
- **Use case**: Premium content, paid videos

#### 6. **Field-Level Encryption**
- Encrypt specific fields (credit card, SSN)
- **Public key** at edge, **private key** at origin

#### 7. **CloudFront Functions**
- **Lightweight** JavaScript functions
- **Edge locations** (faster, cheaper)
- **Use case**: URL rewrites, headers manipulation

#### 8. **Lambda@Edge**
- **Full Lambda** at edge locations
- **Regional Edge Caches** (slower, more powerful)
- **Use case**: Complex logic, API calls

### 🔄 CloudFront vs S3 Cross-Region Replication

| Feature | CloudFront | S3 CRR |
|---------|------------|--------|
| **Purpose** | Cache, global delivery | Replication |
| **TTL** | Yes (cache) | No (real-time) |
| **Edge locations** | 400+ | Specific regions |
| **Use case** | Static content, global | Compliance, low latency |

**🎯 Exam Tip**: 
- "Global content delivery" = CloudFront
- "Low latency globally" = CloudFront
- "Restrict S3 to CloudFront" = OAI/OAC
- "DDoS protection" = CloudFront + AWS Shield

---

## 5️⃣ AWS GLOBAL ACCELERATOR

### 🎯 Khái Niệm

**Global Accelerator** = Improve global application availability and performance using AWS global network.

### 🎯 Key Features

- **2 Anycast IPs**: Static, global
- **AWS global network**: Route traffic via AWS backbone
- **Health checks**: Automatic failover
- **Use case**: Non-HTTP protocols (TCP, UDP), gaming, IoT

### 🔄 Global Accelerator vs CloudFront

| Feature | Global Accelerator | CloudFront |
|---------|-------------------|------------|
| **Purpose** | Accelerate TCP/UDP | Cache HTTP/HTTPS |
| **Caching** | No | Yes |
| **Static IP** | Yes (2 Anycast IPs) | No |
| **Protocols** | TCP, UDP | HTTP, HTTPS, WebSockets |
| **Use case** | Gaming, IoT, VoIP | Static content, APIs |

**🎯 Exam Tip**: 
- "Static IP" + "global" = Global Accelerator
- "Non-HTTP" = Global Accelerator
- "Gaming" = Global Accelerator
- "Cache" = CloudFront

---

## 6️⃣ AWS API GATEWAY

### 🎯 Khái Niệm

**API Gateway** = Managed service to create, publish, maintain APIs.

### 📊 API Types

#### 1. **REST API**
- **HTTP API** + more features
- **API keys**, **usage plans**, **request validation**

#### 2. **HTTP API**
- **Simpler, cheaper** than REST API
- **OIDC, OAuth 2.0** support

#### 3. **WebSocket API**
- **Two-way communication**
- **Use case**: Chat, real-time dashboards

### 🎯 API Gateway Features

#### 1. **Integrations**
- **Lambda**: Serverless backend
- **HTTP**: Public HTTP endpoints
- **AWS Services**: Direct integration (DynamoDB, SNS, SQS)
- **VPC Link**: Private resources (ALB, NLB)

#### 2. **Security**
- **IAM**: AWS credentials
- **Cognito User Pools**: User authentication
- **Lambda Authorizer**: Custom authorization
- **Resource policies**: Cross-account, IP whitelist

#### 3. **Throttling**
- **Account limit**: 10,000 RPS (requests per second)
- **Burst**: 5,000 requests
- **Stage/method limits**: Custom throttling

#### 4. **Caching**
- **TTL**: 0-3600 seconds (default 300)
- **Capacity**: 0.5 GB - 237 GB
- **Invalidate**: Per request or all

#### 5. **Stages**
- **Environments**: dev, test, prod
- **Stage variables**: Environment-specific configs
- **Canary deployments**: Gradual rollout

**🎯 Exam Tip**: 
- "Serverless API" = API Gateway + Lambda
- "Throttling" = API Gateway (10,000 RPS default)
- "WebSocket" = API Gateway WebSocket API

---

## 7️⃣ AWS PRIVATELINK

### 🎯 Khái Niệm

**PrivateLink** = Private connectivity to services (VPC Endpoint Services).

### 🎯 Key Features

- **Private**: No internet, no VPC peering
- **Scalable**: Thousands of VPCs
- **Use case**: Expose services to customers

**Architecture**:
- **Service Provider**: NLB in provider VPC
- **Service Consumer**: Interface Endpoint in consumer VPC

**🎯 Exam Tip**: 
- "Expose service privately" = PrivateLink
- "No VPC peering" = PrivateLink

---

## 8️⃣ AWS CLOUD WAN

### 🎯 Khái Niệm

**Cloud WAN** = Managed wide-area network, connect on-premises and AWS.

### 🎯 Key Features

- **Global network**: Single dashboard
- **Centralized management**
- **Use case**: Large enterprises, complex networks

---

## 📋 NETWORKING SERVICES COMPARISON

| Service | Purpose | Use Case |
|---------|---------|----------|
| **VPC** | Isolated network | Foundation |
| **ELB** | Load balancing | Distribute traffic |
| **Route 53** | DNS | Domain management, routing |
| **CloudFront** | CDN | Global content delivery |
| **Global Accelerator** | Network acceleration | TCP/UDP, static IP |
| **API Gateway** | API management | Serverless APIs |
| **PrivateLink** | Private connectivity | Expose services |
| **Transit Gateway** | Network hub | Connect many VPCs |
| **Direct Connect** | Dedicated connection | High bandwidth, low latency |
| **VPN** | Encrypted connection | Quick setup |

---

## 🎯 EXAM SCENARIOS - NETWORKING

### Scenario 1: Private Subnet Internet Access
**Q**: "Private subnet needs internet access for updates?"

**A**: NAT Gateway in public subnet + route in private subnet route table

---

### Scenario 2: Block Specific IP
**Q**: "Block specific IP address from accessing application?"

**A**: Network ACL (NACL) with deny rule

---

### Scenario 3: High Availability Load Balancer
**Q**: "Distribute HTTP traffic across multiple AZs?"

**A**: Application Load Balancer (ALB) in multiple AZs

---

### Scenario 4: Static IP for Load Balancer
**Q**: "Load balancer with static IP for whitelisting?"

**A**: Network Load Balancer (NLB) with Elastic IP

---

### Scenario 5: Global Low Latency
**Q**: "Serve static content with low latency globally?"

**A**: S3 + CloudFront

---

### Scenario 6: Private Access to S3
**Q**: "Access S3 privately without internet?"

**A**: VPC Gateway Endpoint for S3

---

### Scenario 7: Connect Multiple VPCs
**Q**: "Connect 50 VPCs with transitive routing?"

**A**: AWS Transit Gateway

---

### Scenario 8: On-Premises to AWS
**Q**: "High bandwidth, low latency connection to AWS?"

**A**: AWS Direct Connect

---

## ⚠️ COMMON MISTAKES

1. ❌ **Quên NAT Gateway cho private subnet** → No internet access
2. ❌ **Dùng Security Group để block IP** → Use NACL
3. ❌ **Không enable Cross-Zone Load Balancing** → Uneven distribution
4. ❌ **CNAME cho zone apex** → Use ALIAS record
5. ❌ **Không dùng CloudFront cho global content** → High latency
6. ❌ **VPC Peering cho nhiều VPCs** → Use Transit Gateway (transitive)

---

## 🎓 TIPS ĐẶC BIỆT

### 1. **VPC Security Decision Tree**

```
Need security?
├─ Instance-level? → Security Group
├─ Subnet-level? → NACL
├─ Block specific IP? → NACL (deny rule)
└─ Allow specific traffic? → Security Group (allow rules)
```

### 2. **Load Balancer Selection**

```
HTTP/HTTPS + path routing → ALB
TCP/UDP + high performance → NLB
Static IP required → NLB
Security appliances → GWLB
```

### 3. **Connectivity Selection**

```
Quick setup + encrypted → VPN
High bandwidth + low latency → Direct Connect
Many VPCs + transitive → Transit Gateway
Private to AWS services → VPC Endpoints
Global acceleration → Global Accelerator
Content delivery → CloudFront
```

### 4. **Route 53 Routing Selection**

```
Single resource → Simple
A/B testing → Weighted
Lowest latency → Latency-Based
Failover → Failover + Health Checks
User location → Geolocation
Multiple IPs + health checks → Multi-Value
```

---

## 📚 CHECKLIST

- [ ] Hiểu VPC components (Subnets, IGW, NAT Gateway, Route Tables)
- [ ] Phân biệt Security Groups vs NACLs
- [ ] Nắm vững 3 types Load Balancers (ALB, NLB, GWLB)
- [ ] Biết Route 53 routing policies (7 types)
- [ ] Hiểu CloudFront origins và cache behaviors
- [ ] Phân biệt CloudFront vs Global Accelerator
- [ ] Biết VPC connectivity options (Peering, Transit Gateway, VPN, DX)
- [ ] Hiểu VPC Endpoints (Gateway vs Interface)
- [ ] Nắm API Gateway features và security

---

**Next**: [05-security-services.md](05-security-services.md) - Security là priority cao! 🚀
