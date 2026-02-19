# 🧪 T. HANDS-ON LABS - AWS SAA-C03

> **Comprehensive lab exercises with step-by-step instructions, CLI commands, and architecture diagrams**

---

## 📋 Mục Lục

1. [Lab Setup & Prerequisites](#lab-setup)
2. [Lab 1: VPC & Networking](#lab-1-vpc--networking)
3. [Lab 2: EC2 & Auto Scaling](#lab-2-ec2--auto-scaling)
4. [Lab 3: S3 & Storage](#lab-3-s3--storage)
5. [Lab 4: RDS & Databases](#lab-4-rds--databases)
6. [Lab 5: Load Balancing](#lab-5-load-balancing)
7. [Lab 6: IAM & Security](#lab-6-iam--security)
8. [Lab 7: Lambda & Serverless](#lab-7-lambda--serverless)
9. [Lab 8: CloudFront & CDN](#lab-8-cloudfront--cdn)
10. [Lab 9: High Availability](#lab-9-high-availability)
11. [Lab 10: Monitoring & Logging](#lab-10-monitoring--logging)

---

## 🔧 LAB SETUP & PREREQUISITES

### Prerequisites
- AWS Account (Free Tier eligible)
- AWS CLI installed and configured
- Basic understanding of Linux commands
- Text editor (VS Code, Sublime, etc.)

### AWS CLI Installation

**macOS:**
```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

**Linux:**
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**Windows:**
```powershell
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```

### Configure AWS CLI
```bash
aws configure
# AWS Access Key ID: YOUR_ACCESS_KEY
# AWS Secret Access Key: YOUR_SECRET_KEY
# Default region name: us-east-1
# Default output format: json
```

### Verify Installation
```bash
aws --version
aws sts get-caller-identity
```

---

## 🌐 LAB 1: VPC & NETWORKING

### Lab Objective
Create a production-ready VPC with public and private subnets across 2 Availability Zones.

### Architecture Diagram
```
┌────────────────────────────────────────────────────────────────┐
│                    VPC (10.0.0.0/16)                           │
│                                                                 │
│  ┌──────────────────────────┐  ┌──────────────────────────┐  │
│  │  Availability Zone A     │  │  Availability Zone B     │  │
│  │                          │  │                          │  │
│  │  ┌──────────────────┐   │  │  ┌──────────────────┐   │  │
│  │  │ Public Subnet    │   │  │  │ Public Subnet    │   │  │
│  │  │ 10.0.1.0/24      │   │  │  │ 10.0.2.0/24      │   │  │
│  │  │                  │   │  │  │                  │   │  │
│  │  │ ┌──────────────┐ │   │  │  │ ┌──────────────┐ │   │  │
│  │  │ │ NAT Gateway  │ │   │  │  │ │ NAT Gateway  │ │   │  │
│  │  │ └──────────────┘ │   │  │  │ └──────────────┘ │   │  │
│  │  └──────────────────┘   │  │  └──────────────────┘   │  │
│  │           │              │  │           │              │  │
│  │  ┌────────▼─────────┐   │  │  ┌────────▼─────────┐   │  │
│  │  │ Private Subnet   │   │  │  │ Private Subnet   │   │  │
│  │  │ 10.0.10.0/24     │   │  │  │ 10.0.11.0/24     │   │  │
│  │  │                  │   │  │  │                  │   │  │
│  │  │ ┌──────────────┐ │   │  │  │ ┌──────────────┐ │   │  │
│  │  │ │ EC2 Instance │ │   │  │  │ │ EC2 Instance │ │   │  │
│  │  │ └──────────────┘ │   │  │  │ └──────────────┘ │   │  │
│  │  └──────────────────┘   │  │  └──────────────────┘   │  │
│  └──────────────────────────┘  └──────────────────────────┘  │
│                                                                 │
│  ┌──────────────────┐                                          │
│  │ Internet Gateway │                                          │
│  └──────────────────┘                                          │
└────────────────────────────────────────────────────────────────┘
```

### Step 1: Create VPC

**Using AWS Console:**
1. Navigate to VPC Dashboard
2. Click "Create VPC"
3. Select "VPC and more"
4. Configure:
   - Name: `lab-vpc`
   - IPv4 CIDR: `10.0.0.0/16`
   - Number of AZs: `2`
   - Number of public subnets: `2`
   - Number of private subnets: `2`
   - NAT gateways: `1 per AZ`
   - VPC endpoints: `None`

**Using AWS CLI:**
```bash
# Create VPC
aws ec2 create-vpc \
    --cidr-block 10.0.0.0/16 \
    --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=lab-vpc}]' \
    --region us-east-1

# Save VPC ID
VPC_ID=$(aws ec2 describe-vpcs \
    --filters "Name=tag:Name,Values=lab-vpc" \
    --query 'Vpcs[0].VpcId' \
    --output text)

echo "VPC ID: $VPC_ID"
```

### Step 2: Create Subnets

```bash
# Public Subnet 1 (AZ-A)
aws ec2 create-subnet \
    --vpc-id $VPC_ID \
    --cidr-block 10.0.1.0/24 \
    --availability-zone us-east-1a \
    --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=lab-public-subnet-1a}]'

PUBLIC_SUBNET_1=$(aws ec2 describe-subnets \
    --filters "Name=tag:Name,Values=lab-public-subnet-1a" \
    --query 'Subnets[0].SubnetId' \
    --output text)

# Public Subnet 2 (AZ-B)
aws ec2 create-subnet \
    --vpc-id $VPC_ID \
    --cidr-block 10.0.2.0/24 \
    --availability-zone us-east-1b \
    --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=lab-public-subnet-1b}]'

PUBLIC_SUBNET_2=$(aws ec2 describe-subnets \
    --filters "Name=tag:Name,Values=lab-public-subnet-1b" \
    --query 'Subnets[0].SubnetId' \
    --output text)

# Private Subnet 1 (AZ-A)
aws ec2 create-subnet \
    --vpc-id $VPC_ID \
    --cidr-block 10.0.10.0/24 \
    --availability-zone us-east-1a \
    --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=lab-private-subnet-1a}]'

PRIVATE_SUBNET_1=$(aws ec2 describe-subnets \
    --filters "Name=tag:Name,Values=lab-private-subnet-1a" \
    --query 'Subnets[0].SubnetId' \
    --output text)

# Private Subnet 2 (AZ-B)
aws ec2 create-subnet \
    --vpc-id $VPC_ID \
    --cidr-block 10.0.11.0/24 \
    --availability-zone us-east-1b \
    --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=lab-private-subnet-1b}]'

PRIVATE_SUBNET_2=$(aws ec2 describe-subnets \
    --filters "Name=tag:Name,Values=lab-private-subnet-1b" \
    --query 'Subnets[0].SubnetId' \
    --output text)
```

### Step 3: Create Internet Gateway

```bash
# Create Internet Gateway
aws ec2 create-internet-gateway \
    --tag-specifications 'ResourceType=internet-gateway,Tags=[{Key=Name,Value=lab-igw}]'

IGW_ID=$(aws ec2 describe-internet-gateways \
    --filters "Name=tag:Name,Values=lab-igw" \
    --query 'InternetGateways[0].InternetGatewayId' \
    --output text)

# Attach to VPC
aws ec2 attach-internet-gateway \
    --vpc-id $VPC_ID \
    --internet-gateway-id $IGW_ID
```

### Step 4: Create NAT Gateways

```bash
# Allocate Elastic IPs
EIP_1=$(aws ec2 allocate-address \
    --domain vpc \
    --query 'AllocationId' \
    --output text)

EIP_2=$(aws ec2 allocate-address \
    --domain vpc \
    --query 'AllocationId' \
    --output text)

# Create NAT Gateway in Public Subnet 1
NAT_GW_1=$(aws ec2 create-nat-gateway \
    --subnet-id $PUBLIC_SUBNET_1 \
    --allocation-id $EIP_1 \
    --tag-specifications 'ResourceType=natgateway,Tags=[{Key=Name,Value=lab-nat-1a}]' \
    --query 'NatGateway.NatGatewayId' \
    --output text)

# Create NAT Gateway in Public Subnet 2
NAT_GW_2=$(aws ec2 create-nat-gateway \
    --subnet-id $PUBLIC_SUBNET_2 \
    --allocation-id $EIP_2 \
    --tag-specifications 'ResourceType=natgateway,Tags=[{Key=Name,Value=lab-nat-1b}]' \
    --query 'NatGateway.NatGatewayId' \
    --output text)

# Wait for NAT Gateways to become available
aws ec2 wait nat-gateway-available --nat-gateway-ids $NAT_GW_1 $NAT_GW_2
```

### Step 5: Create Route Tables

```bash
# Create Public Route Table
PUBLIC_RT=$(aws ec2 create-route-table \
    --vpc-id $VPC_ID \
    --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=lab-public-rt}]' \
    --query 'RouteTable.RouteTableId' \
    --output text)

# Add route to Internet Gateway
aws ec2 create-route \
    --route-table-id $PUBLIC_RT \
    --destination-cidr-block 0.0.0.0/0 \
    --gateway-id $IGW_ID

# Associate with public subnets
aws ec2 associate-route-table \
    --subnet-id $PUBLIC_SUBNET_1 \
    --route-table-id $PUBLIC_RT

aws ec2 associate-route-table \
    --subnet-id $PUBLIC_SUBNET_2 \
    --route-table-id $PUBLIC_RT

# Create Private Route Table 1 (for AZ-A)
PRIVATE_RT_1=$(aws ec2 create-route-table \
    --vpc-id $VPC_ID \
    --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=lab-private-rt-1a}]' \
    --query 'RouteTable.RouteTableId' \
    --output text)

# Add route to NAT Gateway 1
aws ec2 create-route \
    --route-table-id $PRIVATE_RT_1 \
    --destination-cidr-block 0.0.0.0/0 \
    --nat-gateway-id $NAT_GW_1

# Associate with private subnet 1
aws ec2 associate-route-table \
    --subnet-id $PRIVATE_SUBNET_1 \
    --route-table-id $PRIVATE_RT_1

# Create Private Route Table 2 (for AZ-B)
PRIVATE_RT_2=$(aws ec2 create-route-table \
    --vpc-id $VPC_ID \
    --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=lab-private-rt-1b}]' \
    --query 'RouteTable.RouteTableId' \
    --output text)

# Add route to NAT Gateway 2
aws ec2 create-route \
    --route-table-id $PRIVATE_RT_2 \
    --destination-cidr-block 0.0.0.0/0 \
    --nat-gateway-id $NAT_GW_2

# Associate with private subnet 2
aws ec2 associate-route-table \
    --subnet-id $PRIVATE_SUBNET_2 \
    --route-table-id $PRIVATE_RT_2
```

### Step 6: Verify Configuration

```bash
# List all subnets
aws ec2 describe-subnets \
    --filters "Name=vpc-id,Values=$VPC_ID" \
    --query 'Subnets[*].[SubnetId,CidrBlock,AvailabilityZone,Tags[?Key==`Name`].Value|[0]]' \
    --output table

# Verify route tables
aws ec2 describe-route-tables \
    --filters "Name=vpc-id,Values=$VPC_ID" \
    --query 'RouteTables[*].[RouteTableId,Tags[?Key==`Name`].Value|[0],Routes[*].[DestinationCidrBlock,GatewayId,NatGatewayId]]' \
    --output table
```

### Cleanup

```bash
# Delete NAT Gateways
aws ec2 delete-nat-gateway --nat-gateway-id $NAT_GW_1
aws ec2 delete-nat-gateway --nat-gateway-id $NAT_GW_2

# Wait for deletion
aws ec2 wait nat-gateway-deleted --nat-gateway-ids $NAT_GW_1 $NAT_GW_2

# Release Elastic IPs
aws ec2 release-address --allocation-id $EIP_1
aws ec2 release-address --allocation-id $EIP_2

# Detach and delete Internet Gateway
aws ec2 detach-internet-gateway --vpc-id $VPC_ID --internet-gateway-id $IGW_ID
aws ec2 delete-internet-gateway --internet-gateway-id $IGW_ID

# Delete subnets
aws ec2 delete-subnet --subnet-id $PUBLIC_SUBNET_1
aws ec2 delete-subnet --subnet-id $PUBLIC_SUBNET_2
aws ec2 delete-subnet --subnet-id $PRIVATE_SUBNET_1
aws ec2 delete-subnet --subnet-id $PRIVATE_SUBNET_2

# Delete route tables (except main)
aws ec2 delete-route-table --route-table-id $PUBLIC_RT
aws ec2 delete-route-table --route-table-id $PRIVATE_RT_1
aws ec2 delete-route-table --route-table-id $PRIVATE_RT_2

# Delete VPC
aws ec2 delete-vpc --vpc-id $VPC_ID
```

---

## 💻 LAB 2: EC2 & AUTO SCALING

### Lab Objective
Deploy a web application with Auto Scaling across multiple Availability Zones.

### Architecture Diagram
```
                    ┌─────────────────────┐
                    │ Application Load    │
                    │     Balancer        │
                    └──────────┬──────────┘
                               │
            ┌──────────────────┼──────────────────┐
            │                  │                  │
    ┌───────▼────────┐ ┌──────▼────────┐ ┌──────▼────────┐
    │   EC2 (AZ-A)   │ │  EC2 (AZ-B)   │ │  EC2 (AZ-A)   │
    │   Web Server   │ │  Web Server   │ │  Web Server   │
    └────────────────┘ └───────────────┘ └───────────────┘
            ▲                  ▲                  ▲
            │                  │                  │
            └──────────────────┴──────────────────┘
                    Auto Scaling Group
                    (Min: 2, Max: 6, Desired: 3)
```

### Step 1: Create Security Group

```bash
# Create security group for web servers
WEB_SG=$(aws ec2 create-security-group \
    --group-name lab-web-sg \
    --description "Security group for web servers" \
    --vpc-id $VPC_ID \
    --query 'GroupId' \
    --output text)

# Allow HTTP from anywhere
aws ec2 authorize-security-group-ingress \
    --group-id $WEB_SG \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0

# Allow SSH from your IP
MY_IP=$(curl -s https://checkip.amazonaws.com)
aws ec2 authorize-security-group-ingress \
    --group-id $WEB_SG \
    --protocol tcp \
    --port 22 \
    --cidr $MY_IP/32
```

### Step 2: Create Launch Template

```bash
# Create user data script
cat > user-data.sh << 'EOF'
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd

# Get instance metadata
INSTANCE_ID=$(curl -s http://169.254.169.254/latest/meta-data/instance-id)
AVAILABILITY_ZONE=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone)

# Create simple webpage
cat > /var/www/html/index.html << HTML
<!DOCTYPE html>
<html>
<head>
    <title>Lab Web Server</title>
    <style>
        body { font-family: Arial; text-align: center; padding: 50px; }
        .info { background: #f0f0f0; padding: 20px; margin: 20px; border-radius: 5px; }
    </style>
</head>
<body>
    <h1>🚀 Lab Web Server</h1>
    <div class="info">
        <h2>Instance Information</h2>
        <p><strong>Instance ID:</strong> $INSTANCE_ID</p>
        <p><strong>Availability Zone:</strong> $AVAILABILITY_ZONE</p>
        <p><strong>Server Time:</strong> $(date)</p>
    </div>
</body>
</html>
HTML
EOF

# Encode user data
USER_DATA=$(base64 user-data.sh | tr -d '\n')

# Create launch template
aws ec2 create-launch-template \
    --launch-template-name lab-web-template \
    --version-description "v1" \
    --launch-template-data "{
        \"ImageId\": \"ami-0c55b159cbfafe1f0\",
        \"InstanceType\": \"t3.micro\",
        \"SecurityGroupIds\": [\"$WEB_SG\"],
        \"UserData\": \"$USER_DATA\",
        \"TagSpecifications\": [{
            \"ResourceType\": \"instance\",
            \"Tags\": [{\"Key\": \"Name\", \"Value\": \"lab-web-server\"}]
        }]
    }"
```

### Step 3: Create Auto Scaling Group

```bash
# Create Auto Scaling Group
aws autoscaling create-auto-scaling-group \
    --auto-scaling-group-name lab-asg \
    --launch-template "LaunchTemplateName=lab-web-template,Version=\$Latest" \
    --min-size 2 \
    --max-size 6 \
    --desired-capacity 3 \
    --vpc-zone-identifier "$PRIVATE_SUBNET_1,$PRIVATE_SUBNET_2" \
    --health-check-type ELB \
    --health-check-grace-period 300 \
    --tags "Key=Name,Value=lab-asg-instance,PropagateAtLaunch=true"
```

### Step 4: Create Target Tracking Scaling Policy

```bash
# CPU-based scaling policy
cat > scaling-policy.json << 'EOF'
{
    "TargetValue": 50.0,
    "PredefinedMetricSpecification": {
        "PredefinedMetricType": "ASGAverageCPUUtilization"
    }
}
EOF

aws autoscaling put-scaling-policy \
    --auto-scaling-group-name lab-asg \
    --policy-name cpu-target-tracking \
    --policy-type TargetTrackingScaling \
    --target-tracking-configuration file://scaling-policy.json
```

### Step 5: Test Auto Scaling

```bash
# Get instance IDs
aws autoscaling describe-auto-scaling-groups \
    --auto-scaling-group-names lab-asg \
    --query 'AutoScalingGroups[0].Instances[*].[InstanceId,AvailabilityZone,HealthStatus]' \
    --output table

# Simulate high CPU load (SSH into instance)
# stress --cpu 4 --timeout 300s

# Monitor scaling activity
aws autoscaling describe-scaling-activities \
    --auto-scaling-group-name lab-asg \
    --max-records 5 \
    --query 'Activities[*].[StartTime,Description,StatusCode]' \
    --output table
```

### Cleanup

```bash
# Delete Auto Scaling Group
aws autoscaling delete-auto-scaling-group \
    --auto-scaling-group-name lab-asg \
    --force-delete

# Delete Launch Template
aws ec2 delete-launch-template \
    --launch-template-name lab-web-template

# Delete Security Group
aws ec2 delete-security-group --group-id $WEB_SG
```

---

## 📦 LAB 3: S3 & STORAGE

### Lab Objective
Create an S3 bucket with versioning, lifecycle policies, and replication.

### Architecture Diagram
```
┌──────────────────────────────────────────────────────────────┐
│                    Primary Region (us-east-1)                │
│                                                               │
│  ┌────────────────────────────────────────────────────┐     │
│  │            S3 Bucket (Primary)                     │     │
│  │         lab-primary-bucket-xxxxx                   │     │
│  │                                                     │     │
│  │  ┌──────────────┐  ┌──────────────┐              │     │
│  │  │  Versioning  │  │  Lifecycle   │              │     │
│  │  │   Enabled    │  │   Policies   │              │     │
│  │  └──────────────┘  └──────────────┘              │     │
│  │                                                     │     │
│  │  Objects:                                          │     │
│  │  - Day 0-30:    S3 Standard                       │     │
│  │  - Day 31-90:   S3 Standard-IA                    │     │
│  │  - Day 91+:     Glacier                           │     │
│  └────────────────────────────────────────────────────┘     │
│                          │                                   │
│                          │ Replication                       │
└──────────────────────────┼───────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────────┐
│                  Secondary Region (us-west-2)                │
│                                                               │
│  ┌────────────────────────────────────────────────────┐     │
│  │          S3 Bucket (Replica)                       │     │
│  │       lab-replica-bucket-xxxxx                     │     │
│  │                                                     │     │
│  │  Cross-Region Replication (CRR)                   │     │
│  │  - Automatic replication                          │     │
│  │  - Versioning enabled                             │     │
│  └────────────────────────────────────────────────────┘     │
└──────────────────────────────────────────────────────────────┘
```

### Step 1: Create Primary S3 Bucket

```bash
# Generate unique bucket name
BUCKET_NAME="lab-primary-bucket-$(date +%s)"
REPLICA_BUCKET="lab-replica-bucket-$(date +%s)"

# Create primary bucket
aws s3api create-bucket \
    --bucket $BUCKET_NAME \
    --region us-east-1

# Enable versioning
aws s3api put-bucket-versioning \
    --bucket $BUCKET_NAME \
    --versioning-configuration Status=Enabled

# Enable encryption
aws s3api put-bucket-encryption \
    --bucket $BUCKET_NAME \
    --server-side-encryption-configuration '{
        "Rules": [{
            "ApplyServerSideEncryptionByDefault": {
                "SSEAlgorithm": "AES256"
            }
        }]
    }'
```

### Step 2: Create Lifecycle Policy

```bash
cat > lifecycle-policy.json << 'EOF'
{
    "Rules": [
        {
            "Id": "Move to IA after 30 days",
            "Status": "Enabled",
            "Transitions": [
                {
                    "Days": 30,
                    "StorageClass": "STANDARD_IA"
                },
                {
                    "Days": 90,
                    "StorageClass": "GLACIER"
                }
            ],
            "Expiration": {
                "Days": 365
            }
        },
        {
            "Id": "Delete old versions",
            "Status": "Enabled",
            "NoncurrentVersionTransitions": [
                {
                    "NoncurrentDays": 30,
                    "StorageClass": "STANDARD_IA"
                }
            ],
            "NoncurrentVersionExpiration": {
                "NoncurrentDays": 90
            }
        }
    ]
}
EOF

aws s3api put-bucket-lifecycle-configuration \
    --bucket $BUCKET_NAME \
    --lifecycle-configuration file://lifecycle-policy.json
```

### Step 3: Create Replica Bucket

```bash
# Create replica bucket in different region
aws s3api create-bucket \
    --bucket $REPLICA_BUCKET \
    --region us-west-2 \
    --create-bucket-configuration LocationConstraint=us-west-2

# Enable versioning on replica
aws s3api put-bucket-versioning \
    --bucket $REPLICA_BUCKET \
    --versioning-configuration Status=Enabled \
    --region us-west-2
```

### Step 4: Configure Replication

```bash
# Create IAM role for replication
cat > trust-policy.json << 'EOF'
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Principal": {"Service": "s3.amazonaws.com"},
        "Action": "sts:AssumeRole"
    }]
}
EOF

aws iam create-role \
    --role-name S3ReplicationRole \
    --assume-role-policy-document file://trust-policy.json

# Create replication policy
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

cat > replication-policy.json << EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetReplicationConfiguration",
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::$BUCKET_NAME"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObjectVersionForReplication",
                "s3:GetObjectVersionAcl"
            ],
            "Resource": "arn:aws:s3:::$BUCKET_NAME/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ReplicateObject",
                "s3:ReplicateDelete"
            ],
            "Resource": "arn:aws:s3:::$REPLICA_BUCKET/*"
        }
    ]
}
EOF

aws iam put-role-policy \
    --role-name S3ReplicationRole \
    --policy-name S3ReplicationPolicy \
    --policy-document file://replication-policy.json

# Configure replication
cat > replication-config.json << EOF
{
    "Role": "arn:aws:iam::$ACCOUNT_ID:role/S3ReplicationRole",
    "Rules": [{
        "Status": "Enabled",
        "Priority": 1,
        "DeleteMarkerReplication": {"Status": "Enabled"},
        "Filter": {},
        "Destination": {
            "Bucket": "arn:aws:s3:::$REPLICA_BUCKET",
            "ReplicationTime": {
                "Status": "Enabled",
                "Time": {"Minutes": 15}
            },
            "Metrics": {
                "Status": "Enabled",
                "EventThreshold": {"Minutes": 15}
            }
        }
    }]
}
EOF

aws s3api put-bucket-replication \
    --bucket $BUCKET_NAME \
    --replication-configuration file://replication-config.json
```

### Step 5: Test Bucket Operations

```bash
# Upload test file
echo "Test content" > test-file.txt
aws s3 cp test-file.txt s3://$BUCKET_NAME/

# List objects
aws s3 ls s3://$BUCKET_NAME/

# Upload new version
echo "Updated content" > test-file.txt
aws s3 cp test-file.txt s3://$BUCKET_NAME/

# List versions
aws s3api list-object-versions \
    --bucket $BUCKET_NAME \
    --prefix test-file.txt

# Check replication status
sleep 60
aws s3 ls s3://$REPLICA_BUCKET/ --region us-west-2

# Download specific version
VERSION_ID=$(aws s3api list-object-versions \
    --bucket $BUCKET_NAME \
    --prefix test-file.txt \
    --query 'Versions[1].VersionId' \
    --output text)

aws s3api get-object \
    --bucket $BUCKET_NAME \
    --key test-file.txt \
    --version-id $VERSION_ID \
    test-file-old.txt
```

### Cleanup

```bash
# Delete all object versions
aws s3api delete-objects \
    --bucket $BUCKET_NAME \
    --delete "$(aws s3api list-object-versions \
        --bucket $BUCKET_NAME \
        --query '{Objects: Versions[].{Key:Key,VersionId:VersionId}}')"

# Delete bucket
aws s3 rb s3://$BUCKET_NAME --force

# Delete replica bucket
aws s3 rb s3://$REPLICA_BUCKET --force --region us-west-2

# Delete IAM role
aws iam delete-role-policy --role-name S3ReplicationRole --policy-name S3ReplicationPolicy
aws iam delete-role --role-name S3ReplicationRole
```

---

## 🗄️ LAB 4: RDS & DATABASES

### Lab Objective
Deploy a highly available RDS MySQL database with Multi-AZ and Read Replicas.

### Architecture Diagram
```
┌────────────────────────────────────────────────────────────────┐
│                         VPC                                     │
│                                                                 │
│  ┌──────────────────────────┐  ┌──────────────────────────┐  │
│  │  Availability Zone A     │  │  Availability Zone B     │  │
│  │                          │  │                          │  │
│  │  ┌──────────────────┐   │  │  ┌──────────────────┐   │  │
│  │  │  RDS Primary     │   │  │  │  RDS Standby     │   │  │
│  │  │  (Active)        │◄──┼──┼──┤  (Passive)       │   │  │
│  │  │                  │   │  │  │  Sync Replication│   │  │
│  │  └────────┬─────────┘   │  │  └──────────────────┘   │  │
│  │           │              │  │                          │  │
│  └───────────┼──────────────┘  └──────────────────────────┘  │
│              │                                                 │
│              │ Async Replication                              │
│              │                                                 │
│  ┌───────────▼──────────────┐                                 │
│  │  Read Replica 1 (AZ-A)   │                                 │
│  │  (Read-only)             │                                 │
│  └──────────────────────────┘                                 │
│                                                                 │
│  ┌──────────────────────────┐                                 │
│  │  Read Replica 2 (AZ-B)   │                                 │
│  │  (Read-only)             │                                 │
│  └──────────────────────────┘                                 │
└────────────────────────────────────────────────────────────────┘

Application Tier
├─ Writes → Primary
└─ Reads → Read Replicas (load balanced)
```

### Step 1: Create DB Subnet Group

```bash
# Create DB subnet group
aws rds create-db-subnet-group \
    --db-subnet-group-name lab-db-subnet-group \
    --db-subnet-group-description "Subnet group for lab RDS" \
    --subnet-ids $PRIVATE_SUBNET_1 $PRIVATE_SUBNET_2 \
    --tags Key=Name,Value=lab-db-subnet-group
```

### Step 2: Create Security Group for RDS

```bash
# Create security group
DB_SG=$(aws ec2 create-security-group \
    --group-name lab-db-sg \
    --description "Security group for RDS" \
    --vpc-id $VPC_ID \
    --query 'GroupId' \
    --output text)

# Allow MySQL from web servers
aws ec2 authorize-security-group-ingress \
    --group-id $DB_SG \
    --protocol tcp \
    --port 3306 \
    --source-group $WEB_SG
```

### Step 3: Create RDS Instance with Multi-AZ

```bash
# Create RDS instance
aws rds create-db-instance \
    --db-instance-identifier lab-mysql-db \
    --db-instance-class db.t3.micro \
    --engine mysql \
    --engine-version 8.0.35 \
    --master-username admin \
    --master-user-password MySecurePassword123! \
    --allocated-storage 20 \
    --storage-type gp3 \
    --storage-encrypted \
    --multi-az \
    --db-subnet-group-name lab-db-subnet-group \
    --vpc-security-group-ids $DB_SG \
    --backup-retention-period 7 \
    --preferred-backup-window "03:00-04:00" \
    --preferred-maintenance-window "mon:04:00-mon:05:00" \
    --enable-cloudwatch-logs-exports '["error","general","slowquery"]' \
    --tags Key=Name,Value=lab-mysql-db

# Wait for DB to become available (takes 5-10 minutes)
aws rds wait db-instance-available \
    --db-instance-identifier lab-mysql-db

# Get endpoint
DB_ENDPOINT=$(aws rds describe-db-instances \
    --db-instance-identifier lab-mysql-db \
    --query 'DBInstances[0].Endpoint.Address' \
    --output text)

echo "Database Endpoint: $DB_ENDPOINT"
```

### Step 4: Create Read Replicas

```bash
# Create Read Replica 1
aws rds create-db-instance-read-replica \
    --db-instance-identifier lab-mysql-replica-1 \
    --source-db-instance-identifier lab-mysql-db \
    --db-instance-class db.t3.micro \
    --availability-zone us-east-1a \
    --tags Key=Name,Value=lab-mysql-replica-1

# Create Read Replica 2
aws rds create-db-instance-read-replica \
    --db-instance-identifier lab-mysql-replica-2 \
    --source-db-instance-identifier lab-mysql-db \
    --db-instance-class db.t3.micro \
    --availability-zone us-east-1b \
    --tags Key=Name,Value=lab-mysql-replica-2

# Wait for replicas
aws rds wait db-instance-available \
    --db-instance-identifier lab-mysql-replica-1

aws rds wait db-instance-available \
    --db-instance-identifier lab-mysql-replica-2
```

### Step 5: Connect and Test Database

```bash
# Install MySQL client (on EC2 instance in same VPC)
sudo yum install -y mysql

# Connect to primary
mysql -h $DB_ENDPOINT -u admin -pMySecurePassword123!

# Run SQL commands
CREATE DATABASE testdb;
USE testdb;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO users (username, email) VALUES 
    ('user1', 'user1@example.com'),
    ('user2', 'user2@example.com'),
    ('user3', 'user3@example.com');

SELECT * FROM users;

# Test read replica
REPLICA_ENDPOINT=$(aws rds describe-db-instances \
    --db-instance-identifier lab-mysql-replica-1 \
    --query 'DBInstances[0].Endpoint.Address' \
    --output text)

mysql -h $REPLICA_ENDPOINT -u admin -pMySecurePassword123! -e "SELECT * FROM testdb.users;"
```

### Step 6: Test Failover

```bash
# Initiate failover
aws rds reboot-db-instance \
    --db-instance-identifier lab-mysql-db \
    --force-failover

# Monitor failover
aws rds describe-events \
    --source-identifier lab-mysql-db \
    --source-type db-instance \
    --duration 60

# Check new primary AZ
aws rds describe-db-instances \
    --db-instance-identifier lab-mysql-db \
    --query 'DBInstances[0].[AvailabilityZone,SecondaryAvailabilityZone]' \
    --output table
```

### Step 7: Create Manual Snapshot

```bash
# Create snapshot
aws rds create-db-snapshot \
    --db-instance-identifier lab-mysql-db \
    --db-snapshot-identifier lab-mysql-snapshot-$(date +%Y%m%d)

# List snapshots
aws rds describe-db-snapshots \
    --db-instance-identifier lab-mysql-db \
    --query 'DBSnapshots[*].[DBSnapshotIdentifier,SnapshotCreateTime,Status]' \
    --output table
```

### Cleanup

```bash
# Delete read replicas
aws rds delete-db-instance \
    --db-instance-identifier lab-mysql-replica-1 \
    --skip-final-snapshot

aws rds delete-db-instance \
    --db-instance-identifier lab-mysql-replica-2 \
    --skip-final-snapshot

# Delete primary instance
aws rds delete-db-instance \
    --db-instance-identifier lab-mysql-db \
    --skip-final-snapshot

# Delete subnet group
aws rds delete-db-subnet-group \
    --db-subnet-group-name lab-db-subnet-group

# Delete security group
aws ec2 delete-security-group --group-id $DB_SG
```

---

## ⚖️ LAB 5: LOAD BALANCING

### Lab Objective
Configure an Application Load Balancer with path-based routing and health checks.

### Architecture Diagram
```
                    Internet
                       │
                       ▼
            ┌──────────────────────┐
            │ Application Load     │
            │    Balancer          │
            └──────────┬───────────┘
                       │
         ┌─────────────┼─────────────┐
         │             │             │
    /api/*        /images/*      /*
         │             │             │
         ▼             ▼             ▼
    ┌─────────┐  ┌─────────┐  ┌─────────┐
    │Target   │  │Target   │  │Target   │
    │Group 1  │  │Group 2  │  │Group 3  │
    │(API)    │  │(Images) │  │(Web)    │
    └─────────┘  └─────────┘  └─────────┘
         │             │             │
    ┌────▼────┐  ┌────▼────┐  ┌────▼────┐
    │EC2 (API)│  │EC2(IMG) │  │EC2 (Web)│
    └─────────┘  └─────────┘  └─────────┘
```

### Step 1: Create Target Groups

```bash
# Create Target Group 1 (API servers)
TG_API=$(aws elbv2 create-target-group \
    --name lab-tg-api \
    --protocol HTTP \
    --port 8080 \
    --vpc-id $VPC_ID \
    --health-check-enabled \
    --health-check-path /api/health \
    --health-check-interval-seconds 30 \
    --health-check-timeout-seconds 5 \
    --healthy-threshold-count 2 \
    --unhealthy-threshold-count 3 \
    --query 'TargetGroups[0].TargetGroupArn' \
    --output text)

# Create Target Group 2 (Image servers)
TG_IMAGES=$(aws elbv2 create-target-group \
    --name lab-tg-images \
    --protocol HTTP \
    --port 8081 \
    --vpc-id $VPC_ID \
    --health-check-enabled \
    --health-check-path /images/health \
    --health-check-interval-seconds 30 \
    --query 'TargetGroups[0].TargetGroupArn' \
    --output text)

# Create Target Group 3 (Web servers)
TG_WEB=$(aws elbv2 create-target-group \
    --name lab-tg-web \
    --protocol HTTP \
    --port 80 \
    --vpc-id $VPC_ID \
    --health-check-enabled \
    --health-check-path /health \
    --health-check-interval-seconds 30 \
    --query 'TargetGroups[0].TargetGroupArn' \
    --output text)
```

### Step 2: Create Application Load Balancer

```bash
# Create security group for ALB
ALB_SG=$(aws ec2 create-security-group \
    --group-name lab-alb-sg \
    --description "Security group for ALB" \
    --vpc-id $VPC_ID \
    --query 'GroupId' \
    --output text)

# Allow HTTP from anywhere
aws ec2 authorize-security-group-ingress \
    --group-id $ALB_SG \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0

# Allow HTTPS from anywhere
aws ec2 authorize-security-group-ingress \
    --group-id $ALB_SG \
    --protocol tcp \
    --port 443 \
    --cidr 0.0.0.0/0

# Create ALB
ALB_ARN=$(aws elbv2 create-load-balancer \
    --name lab-alb \
    --subnets $PUBLIC_SUBNET_1 $PUBLIC_SUBNET_2 \
    --security-groups $ALB_SG \
    --scheme internet-facing \
    --type application \
    --ip-address-type ipv4 \
    --query 'LoadBalancers[0].LoadBalancerArn' \
    --output text)

# Get ALB DNS name
ALB_DNS=$(aws elbv2 describe-load-balancers \
    --load-balancer-arns $ALB_ARN \
    --query 'LoadBalancers[0].DNSName' \
    --output text)

echo "ALB DNS: $ALB_DNS"
```

### Step 3: Create Listeners and Rules

```bash
# Create HTTP listener with default action
LISTENER_ARN=$(aws elbv2 create-listener \
    --load-balancer-arn $ALB_ARN \
    --protocol HTTP \
    --port 80 \
    --default-actions Type=forward,TargetGroupArn=$TG_WEB \
    --query 'Listeners[0].ListenerArn' \
    --output text)

# Create rule for /api/*
aws elbv2 create-rule \
    --listener-arn $LISTENER_ARN \
    --priority 1 \
    --conditions Field=path-pattern,Values='/api/*' \
    --actions Type=forward,TargetGroupArn=$TG_API

# Create rule for /images/*
aws elbv2 create-rule \
    --listener-arn $LISTENER_ARN \
    --priority 2 \
    --conditions Field=path-pattern,Values='/images/*' \
    --actions Type=forward,TargetGroupArn=$TG_IMAGES
```

### Step 4: Launch EC2 Instances and Register Targets

```bash
# Launch API server
API_INSTANCE=$(aws ec2 run-instances \
    --image-id ami-0c55b159cbfafe1f0 \
    --instance-type t3.micro \
    --subnet-id $PRIVATE_SUBNET_1 \
    --security-group-ids $WEB_SG \
    --user-data '#!/bin/bash
yum install -y python3
mkdir -p /opt/api
cat > /opt/api/server.py << "PYEOF"
from http.server import HTTPServer, BaseHTTPRequestHandler
class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == "/api/health":
            self.send_response(200)
            self.end_headers()
            self.wfile.write(b"OK")
        else:
            self.send_response(200)
            self.end_headers()
            self.wfile.write(b"API Server Response")
HTTPServer(("", 8080), Handler).serve_forever()
PYEOF
python3 /opt/api/server.py &' \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=lab-api-server}]' \
    --query 'Instances[0].InstanceId' \
    --output text)

# Wait for instance
aws ec2 wait instance-running --instance-ids $API_INSTANCE

# Register with target group
aws elbv2 register-targets \
    --target-group-arn $TG_API \
    --targets Id=$API_INSTANCE
```

### Step 5: Test Load Balancer

```bash
# Test default route
curl http://$ALB_DNS/

# Test API route
curl http://$ALB_DNS/api/test

# Test health check
curl http://$ALB_DNS/api/health

# Check target health
aws elbv2 describe-target-health \
    --target-group-arn $TG_API \
    --query 'TargetHealthDescriptions[*].[Target.Id,TargetHealth.State,TargetHealth.Reason]' \
    --output table
```

### Cleanup

```bash
# Deregister targets
aws elbv2 deregister-targets \
    --target-group-arn $TG_API \
    --targets Id=$API_INSTANCE

# Terminate instances
aws ec2 terminate-instances --instance-ids $API_INSTANCE

# Delete listener
aws elbv2 delete-listener --listener-arn $LISTENER_ARN

# Delete ALB
aws elbv2 delete-load-balancer --load-balancer-arn $ALB_ARN

# Wait for ALB deletion
sleep 60

# Delete target groups
aws elbv2 delete-target-group --target-group-arn $TG_API
aws elbv2 delete-target-group --target-group-arn $TG_IMAGES
aws elbv2 delete-target-group --target-group-arn $TG_WEB

# Delete security group
aws ec2 delete-security-group --group-id $ALB_SG
```

---

## 🔐 LAB 6: IAM & SECURITY

### Lab Objective
Implement least-privilege IAM policies and cross-account access.

### Architecture Diagram
```
┌─────────────────────────────────────────────────────────────┐
│                    AWS Account A (123456789012)             │
│                                                              │
│  ┌────────────────────────────────────────────────────┐    │
│  │              IAM User: Developer                   │    │
│  │              - MFA Enabled                         │    │
│  │              - Access Keys                         │    │
│  └──────────────────────┬─────────────────────────────┘    │
│                         │                                   │
│                         │ Assumes Role                      │
│                         ▼                                   │
│  ┌────────────────────────────────────────────────────┐    │
│  │         IAM Role: S3ReadOnlyRole                   │    │
│  │         - Policy: AmazonS3ReadOnlyAccess           │    │
│  │         - Trust: Account B                         │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                         │
                         │ Cross-Account Access
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    AWS Account B (987654321098)             │
│                                                              │
│  ┌────────────────────────────────────────────────────┐    │
│  │              S3 Bucket: shared-bucket              │    │
│  │              - Bucket Policy allows Account A      │    │
│  │              - Encryption enabled                  │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Step 1: Create IAM User with MFA

```bash
# Create IAM user
aws iam create-user --user-name lab-developer

# Create access key
aws iam create-access-key --user-name lab-developer

# Enable MFA (use AWS Console for this step)
# 1. Go to IAM Console
# 2. Select user "lab-developer"
# 3. Security credentials tab
# 4. Assign MFA device
# 5. Scan QR code with authenticator app
```

### Step 2: Create Custom IAM Policy

```bash
# Create policy for S3 read-only access to specific bucket
cat > s3-readonly-policy.json << 'EOF'
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListBucket",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": "arn:aws:s3:::lab-bucket-*"
        },
        {
            "Sid": "GetObjects",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetObjectVersion"
            ],
            "Resource": "arn:aws:s3:::lab-bucket-*/*"
        }
    ]
}
EOF

# Create policy
POLICY_ARN=$(aws iam create-policy \
    --policy-name LabS3ReadOnlyPolicy \
    --policy-document file://s3-readonly-policy.json \
    --query 'Policy.Arn' \
    --output text)

# Attach policy to user
aws iam attach-user-policy \
    --user-name lab-developer \
    --policy-arn $POLICY_ARN
```

### Step 3: Create IAM Role for EC2

```bash
# Create trust policy for EC2
cat > ec2-trust-policy.json << 'EOF'
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Principal": {"Service": "ec2.amazonaws.com"},
        "Action": "sts:AssumeRole"
    }]
}
EOF

# Create role
aws iam create-role \
    --role-name LabEC2Role \
    --assume-role-policy-document file://ec2-trust-policy.json

# Attach managed policy
aws iam attach-role-policy \
    --role-name LabEC2Role \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# Create instance profile
aws iam create-instance-profile \
    --instance-profile-name LabEC2Profile

# Add role to instance profile
aws iam add-role-to-instance-profile \
    --instance-profile-name LabEC2Profile \
    --role-name LabEC2Role
```

### Step 4: Create Cross-Account IAM Role

```bash
# Create trust policy for cross-account access
ACCOUNT_B_ID="987654321098"  # Replace with actual account ID

cat > cross-account-trust.json << EOF
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Principal": {"AWS": "arn:aws:iam::$ACCOUNT_B_ID:root"},
        "Action": "sts:AssumeRole",
        "Condition": {
            "StringEquals": {
                "sts:ExternalId": "UniqueExternalId123"
            }
        }
    }]
}
EOF

# Create cross-account role
aws iam create-role \
    --role-name CrossAccountS3Access \
    --assume-role-policy-document file://cross-account-trust.json

# Attach policy
aws iam attach-role-policy \
    --role-name CrossAccountS3Access \
    --policy-arn $POLICY_ARN
```

### Step 5: Test IAM Permissions

```bash
# Configure AWS CLI with developer credentials
aws configure --profile lab-developer
# Enter access key and secret key from Step 1

# Test S3 access
aws s3 ls --profile lab-developer

# Try to create bucket (should fail)
aws s3 mb s3://test-bucket-fail --profile lab-developer
# Expected: Access Denied

# Assume role (from Account B)
CREDENTIALS=$(aws sts assume-role \
    --role-arn "arn:aws:iam::123456789012:role/CrossAccountS3Access" \
    --role-session-name "CrossAccountSession" \
    --external-id "UniqueExternalId123" \
    --query 'Credentials' \
    --output json)

# Extract credentials
export AWS_ACCESS_KEY_ID=$(echo $CREDENTIALS | jq -r '.AccessKeyId')
export AWS_SECRET_ACCESS_KEY=$(echo $CREDENTIALS | jq -r '.SecretAccessKey')
export AWS_SESSION_TOKEN=$(echo $CREDENTIALS | jq -r '.SessionToken')

# Test access with assumed role
aws s3 ls
```

### Step 6: Enable CloudTrail for Audit

```bash
# Create S3 bucket for CloudTrail logs
TRAIL_BUCKET="lab-cloudtrail-$(date +%s)"
aws s3 mb s3://$TRAIL_BUCKET

# Create bucket policy for CloudTrail
cat > cloudtrail-bucket-policy.json << EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailAclCheck",
            "Effect": "Allow",
            "Principal": {"Service": "cloudtrail.amazonaws.com"},
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::$TRAIL_BUCKET"
        },
        {
            "Sid": "AWSCloudTrailWrite",
            "Effect": "Allow",
            "Principal": {"Service": "cloudtrail.amazonaws.com"},
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::$TRAIL_BUCKET/*",
            "Condition": {
                "StringEquals": {"s3:x-amz-acl": "bucket-owner-full-control"}
            }
        }
    ]
}
EOF

aws s3api put-bucket-policy \
    --bucket $TRAIL_BUCKET \
    --policy file://cloudtrail-bucket-policy.json

# Create trail
aws cloudtrail create-trail \
    --name lab-trail \
    --s3-bucket-name $TRAIL_BUCKET \
    --is-multi-region-trail

# Start logging
aws cloudtrail start-logging --name lab-trail

# View recent events
aws cloudtrail lookup-events \
    --max-results 10 \
    --query 'Events[*].[EventTime,EventName,Username]' \
    --output table
```

### Cleanup

```bash
# Delete CloudTrail
aws cloudtrail stop-logging --name lab-trail
aws cloudtrail delete-trail --name lab-trail
aws s3 rb s3://$TRAIL_BUCKET --force

# Remove role from instance profile
aws iam remove-role-from-instance-profile \
    --instance-profile-name LabEC2Profile \
    --role-name LabEC2Role

# Delete instance profile
aws iam delete-instance-profile \
    --instance-profile-name LabEC2Profile

# Detach policies
aws iam detach-role-policy \
    --role-name LabEC2Role \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

aws iam detach-role-policy \
    --role-name CrossAccountS3Access \
    --policy-arn $POLICY_ARN

aws iam detach-user-policy \
    --user-name lab-developer \
    --policy-arn $POLICY_ARN

# Delete roles
aws iam delete-role --role-name LabEC2Role
aws iam delete-role --role-name CrossAccountS3Access

# Delete policy
aws iam delete-policy --policy-arn $POLICY_ARN

# Delete user access key
ACCESS_KEY_ID=$(aws iam list-access-keys \
    --user-name lab-developer \
    --query 'AccessKeyMetadata[0].AccessKeyId' \
    --output text)

aws iam delete-access-key \
    --user-name lab-developer \
    --access-key-id $ACCESS_KEY_ID

# Delete user
aws iam delete-user --user-name lab-developer
```

---

*Continued in next sections: Lambda, CloudFront, High Availability, and Monitoring labs...*

## 📝 Lab Completion Checklist

- [ ] Completed VPC & Networking lab
- [ ] Completed EC2 & Auto Scaling lab
- [ ] Completed S3 & Storage lab
- [ ] Completed RDS & Databases lab
- [ ] Completed Load Balancing lab
- [ ] Completed IAM & Security lab
- [ ] All resources cleaned up to avoid charges

---

*Use these labs to gain hands-on experience with AWS services for the SAA-C03 exam!*

---

## ⚡ LAB 7: LAMBDA & SERVERLESS

### Lab Objective
Build a serverless application with Lambda, API Gateway, and DynamoDB.

### Architecture Diagram
```
        Internet
           │
           ▼
    ┌──────────────┐
    │ API Gateway  │
    │  (REST API)  │
    └──────┬───────┘
           │
           ▼
    ┌──────────────┐
    │   Lambda     │
    │  Function    │
    └──────┬───────┘
           │
           ▼
    ┌──────────────┐
    │  DynamoDB    │
    │    Table     │
    └──────────────┘
```

### Step 1: Create DynamoDB Table

```bash
# Create table
aws dynamodb create-table \
    --table-name LabUsers \
    --attribute-definitions \
        AttributeName=userId,AttributeType=S \
    --key-schema \
        AttributeName=userId,KeyType=HASH \
    --billing-mode PAY_PER_REQUEST \
    --tags Key=Name,Value=lab-users-table

# Wait for table to be active
aws dynamodb wait table-exists --table-name LabUsers

# Add sample data
aws dynamodb put-item \
    --table-name LabUsers \
    --item '{
        "userId": {"S": "user001"},
        "name": {"S": "John Doe"},
        "email": {"S": "john@example.com"}
    }'
```

### Step 2: Create Lambda Execution Role

```bash
# Create trust policy
cat > lambda-trust-policy.json << 'EOFT'
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Principal": {"Service": "lambda.amazonaws.com"},
        "Action": "sts:AssumeRole"
    }]
}
EOFT

# Create role
aws iam create-role \
    --role-name LabLambdaRole \
    --assume-role-policy-document file://lambda-trust-policy.json

# Attach policies
aws iam attach-role-policy \
    --role-name LabLambdaRole \
    --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

# Create custom policy for DynamoDB
cat > lambda-dynamodb-policy.json << 'EOFT'
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": [
            "dynamodb:GetItem",
            "dynamodb:PutItem",
            "dynamodb:Query",
            "dynamodb:Scan"
        ],
        "Resource": "arn:aws:dynamodb:*:*:table/LabUsers"
    }]
}
EOFT

aws iam put-role-policy \
    --role-name LabLambdaRole \
    --policy-name DynamoDBAccess \
    --policy-document file://lambda-dynamodb-policy.json
```

### Step 3: Create Lambda Function

```bash
# Create function code
cat > lambda_function.py << 'EOFT'
import json
import boto3
import os

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table(os.environ['TABLE_NAME'])

def lambda_handler(event, context):
    http_method = event['httpMethod']
    
    if http_method == 'GET':
        # Get all users
        response = table.scan()
        return {
            'statusCode': 200,
            'headers': {'Content-Type': 'application/json'},
            'body': json.dumps(response['Items'])
        }
    
    elif http_method == 'POST':
        # Create user
        body = json.loads(event['body'])
        table.put_item(Item=body)
        return {
            'statusCode': 201,
            'headers': {'Content-Type': 'application/json'},
            'body': json.dumps({'message': 'User created'})
        }
    
    else:
        return {
            'statusCode': 400,
            'body': json.dumps({'error': 'Unsupported method'})
        }
EOFT

# Package function
zip function.zip lambda_function.py

# Get role ARN
LAMBDA_ROLE_ARN=$(aws iam get-role \
    --role-name LabLambdaRole \
    --query 'Role.Arn' \
    --output text)

# Create Lambda function
aws lambda create-function \
    --function-name LabUserAPI \
    --runtime python3.11 \
    --role $LAMBDA_ROLE_ARN \
    --handler lambda_function.lambda_handler \
    --zip-file fileb://function.zip \
    --environment Variables={TABLE_NAME=LabUsers} \
    --timeout 30 \
    --memory-size 256
```

### Step 4: Test Lambda Function

```bash
# Create test event
cat > test-event.json << 'EOFT'
{
    "httpMethod": "GET",
    "body": null
}
EOFT

# Invoke function
aws lambda invoke \
    --function-name LabUserAPI \
    --payload file://test-event.json \
    response.json

# View response
cat response.json
```

### Step 5: Create API Gateway

```bash
# Create REST API
API_ID=$(aws apigateway create-rest-api \
    --name LabUserAPI \
    --description "API for user management" \
    --endpoint-configuration types=REGIONAL \
    --query 'id' \
    --output text)

# Get root resource ID
ROOT_ID=$(aws apigateway get-resources \
    --rest-api-id $API_ID \
    --query 'items[0].id' \
    --output text)

# Create /users resource
USERS_RESOURCE=$(aws apigateway create-resource \
    --rest-api-id $API_ID \
    --parent-id $ROOT_ID \
    --path-part users \
    --query 'id' \
    --output text)

# Create GET method
aws apigateway put-method \
    --rest-api-id $API_ID \
    --resource-id $USERS_RESOURCE \
    --http-method GET \
    --authorization-type NONE

# Create POST method
aws apigateway put-method \
    --rest-api-id $API_ID \
    --resource-id $USERS_RESOURCE \
    --http-method POST \
    --authorization-type NONE

# Get Lambda ARN
LAMBDA_ARN=$(aws lambda get-function \
    --function-name LabUserAPI \
    --query 'Configuration.FunctionArn' \
    --output text)

ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
REGION=$(aws configure get region)

# Integrate GET method with Lambda
aws apigateway put-integration \
    --rest-api-id $API_ID \
    --resource-id $USERS_RESOURCE \
    --http-method GET \
    --type AWS_PROXY \
    --integration-http-method POST \
    --uri "arn:aws:apigateway:$REGION:lambda:path/2015-03-31/functions/$LAMBDA_ARN/invocations"

# Integrate POST method with Lambda
aws apigateway put-integration \
    --rest-api-id $API_ID \
    --resource-id $USERS_RESOURCE \
    --http-method POST \
    --type AWS_PROXY \
    --integration-http-method POST \
    --uri "arn:aws:apigateway:$REGION:lambda:path/2015-03-31/functions/$LAMBDA_ARN/invocations"

# Grant API Gateway permission to invoke Lambda
aws lambda add-permission \
    --function-name LabUserAPI \
    --statement-id apigateway-get \
    --action lambda:InvokeFunction \
    --principal apigateway.amazonaws.com \
    --source-arn "arn:aws:execute-api:$REGION:$ACCOUNT_ID:$API_ID/*/*/users"

# Deploy API
aws apigateway create-deployment \
    --rest-api-id $API_ID \
    --stage-name prod

# Get API endpoint
API_ENDPOINT="https://$API_ID.execute-api.$REGION.amazonaws.com/prod/users"
echo "API Endpoint: $API_ENDPOINT"
```

### Step 6: Test API

```bash
# Test GET
curl -X GET $API_ENDPOINT

# Test POST
curl -X POST $API_ENDPOINT \
    -H "Content-Type: application/json" \
    -d '{
        "userId": "user002",
        "name": "Jane Smith",
        "email": "jane@example.com"
    }'

# Test GET again to see new user
curl -X GET $API_ENDPOINT
```

### Cleanup

```bash
# Delete API Gateway
aws apigateway delete-rest-api --rest-api-id $API_ID

# Delete Lambda function
aws lambda delete-function --function-name LabUserAPI

# Delete DynamoDB table
aws dynamodb delete-table --table-name LabUsers

# Detach policies from role
aws iam detach-role-policy \
    --role-name LabLambdaRole \
    --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

aws iam delete-role-policy \
    --role-name LabLambdaRole \
    --policy-name DynamoDBAccess

# Delete role
aws iam delete-role --role-name LabLambdaRole

# Clean up files
rm function.zip lambda_function.py test-event.json response.json
```

---

## 🌐 LAB 8: CLOUDFRONT & CDN

### Lab Objective
Deploy a static website with CloudFront CDN for global content delivery.

### Architecture Diagram
```
                    Global Users
                         │
         ┌───────────────┼───────────────┐
         │               │               │
         ▼               ▼               ▼
    ┌─────────┐    ┌─────────┐    ┌─────────┐
    │Edge Loc │    │Edge Loc │    │Edge Loc │
    │(Tokyo)  │    │(London) │    │(Sydney) │
    └────┬────┘    └────┬────┘    └────┬────┘
         │              │              │
         └──────────────┼──────────────┘
                        │
                        ▼
                ┌───────────────┐
                │  CloudFront   │
                │ Distribution  │
                └───────┬───────┘
                        │
                        ▼
                ┌───────────────┐
                │  S3 Bucket    │
                │ (us-east-1)   │
                │               │
                │ - index.html  │
                │ - style.css   │
                │ - images/     │
                └───────────────┘
```

### Step 1: Create S3 Bucket for Website

```bash
# Create bucket
WEBSITE_BUCKET="lab-website-$(date +%s)"
aws s3 mb s3://$WEBSITE_BUCKET

# Create website files
cat > index.html << 'EOFT'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lab Static Website</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>🚀 CloudFront Lab Website</h1>
        <p>This website is served through Amazon CloudFront CDN</p>
        <div class="info">
            <h2>Benefits of CloudFront:</h2>
            <ul>
                <li>Low latency content delivery</li>
                <li>Global edge locations</li>
                <li>DDoS protection</li>
                <li>HTTPS support</li>
            </ul>
        </div>
    </div>
</body>
</html>
EOFT

cat > style.css << 'EOFT'
body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    margin: 0;
    padding: 20px;
}

.container {
    max-width: 800px;
    margin: 50px auto;
    background: white;
    padding: 40px;
    border-radius: 10px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

h1 {
    color: #667eea;
    text-align: center;
}

.info {
    background: #f5f5f5;
    padding: 20px;
    border-radius: 5px;
    margin-top: 20px;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    padding: 10px;
    margin: 5px 0;
    background: white;
    border-left: 4px solid #667eea;
}
EOFT

# Upload files
aws s3 cp index.html s3://$WEBSITE_BUCKET/
aws s3 cp style.css s3://$WEBSITE_BUCKET/

# Configure bucket for website hosting
aws s3 website s3://$WEBSITE_BUCKET/ \
    --index-document index.html \
    --error-document error.html
```

### Step 2: Create CloudFront Distribution

```bash
# Create Origin Access Identity
OAI_ID=$(aws cloudfront create-cloud-front-origin-access-identity \
    --cloud-front-origin-access-identity-config \
    CallerReference=$(date +%s),Comment="OAI for lab website" \
    --query 'CloudFrontOriginAccessIdentity.Id' \
    --output text)

# Create bucket policy for CloudFront
cat > cf-bucket-policy.json << EOF
{
    "Version": "2012-10-17",
    "Statement": [{
        "Sid": "CloudFrontAccess",
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity $OAI_ID"
        },
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::$WEBSITE_BUCKET/*"
    }]
}
