# ⚙️ MANAGEMENT & GOVERNANCE - AWS SAA-C03

## 📌 Tổng Quan
Management & Governance services chiếm **10-15% câu hỏi**. Focus vào CloudFormation, Systems Manager, và Organizations.

---

## 1️⃣ AWS CLOUDFORMATION

### 🎯 Khái Niệm
**CloudFormation** = Infrastructure as Code (IaC), define resources in templates.

### 📊 Key Features
- **Templates**: JSON or YAML
- **Stacks**: Collection of resources
- **Change Sets**: Preview changes before applying
- **StackSets**: Deploy across multiple accounts/regions

### 🔄 Template Components
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: My template
Parameters:  # Input values
Resources:   # AWS resources (REQUIRED)
Outputs:     # Export values
Mappings:    # Static variables
Conditions:  # Conditional logic
```

### 🎯 Key Concepts
- **Drift Detection**: Detect manual changes
- **Rollback**: Automatic on failure
- **Nested Stacks**: Reusable templates
- **Cross-Stack References**: Export/Import values

**🎯 Exam Tip**: 
- "Infrastructure as Code" = CloudFormation
- "Automate resource provisioning" = CloudFormation

---

## 2️⃣ AWS SYSTEMS MANAGER (SSM)

### 🎯 Khái Niệm
**Systems Manager** = Manage EC2 and on-premises at scale.

### 📊 Key Features

#### 1. **Session Manager**
- **Shell access** without SSH/RDP
- **No bastion host** needed
- **Audit**: CloudTrail logs

#### 2. **Run Command**
- **Execute commands** on multiple instances
- **No SSH** required

#### 3. **Patch Manager**
- **Automate patching** for OS and applications
- **Maintenance Windows**: Schedule patching

#### 4. **Parameter Store**
- **Store configuration** and secrets
- **Tiers**: Standard (free), Advanced ($0.05)
- **Hierarchy**: /app/dev/db-password
- **Integration**: KMS encryption

#### 5. **Automation**
- **Automation Documents**: Runbooks
- **Use case**: AMI creation, instance patching

#### 6. **Inventory**
- **Collect metadata** from instances
- **Use case**: Software inventory, compliance

**🎯 Exam Tip**: 
- "Shell access without SSH" = Session Manager
- "Store configs" = Parameter Store
- "Automate patching" = Patch Manager

---

## 3️⃣ AWS ORGANIZATIONS

### 🎯 Khái Niệm
**Organizations** = Manage multiple AWS accounts centrally.

### 📊 Key Features

#### 1. **Organization Units (OUs)**
- **Hierarchical structure**: Root → OUs → Accounts
- **Example**: Prod OU, Dev OU

#### 2. **Service Control Policies (SCPs)**
- **Guardrails**: Maximum permissions
- **Does NOT grant**: Only restricts
- **Apply to**: OUs or accounts

#### 3. **Consolidated Billing**
- **Single payment**: All accounts
- **Volume discounts**: Aggregate usage
- **Reserved Instance sharing**

#### 4. **Account Management**
- **Programmatic creation**: API-based
- **Invited accounts**: External accounts

**🎯 Exam Tip**: 
- "Multi-account management" = Organizations
- "Restrict permissions across accounts" = SCPs
- "Consolidated billing" = Organizations

---

## 4️⃣ AWS CONTROL TOWER

### 🎯 Khái Niệm
**Control Tower** = Setup multi-account environment with best practices.

### 📊 Key Features
- **Landing Zone**: Multi-account setup
- **Guardrails**: Preventive and detective controls
- **Account Factory**: Automated account provisioning
- **Dashboard**: Compliance overview

**🎯 Exam Tip**: 
- "Multi-account best practices" = Control Tower
- "Landing Zone" = Control Tower

---

## 5️⃣ AWS SERVICE CATALOG

### 🎯 Khái Niệm
**Service Catalog** = Create and manage approved product catalogs.

### 📊 Key Features
- **Products**: CloudFormation templates
- **Portfolios**: Collections of products
- **Constraints**: Launch constraints, template constraints
- **Use case**: Standardized deployments

**🎯 Exam Tip**: 
- "Approved product catalog" = Service Catalog
- "Self-service provisioning" = Service Catalog

---

## 6️⃣ AWS TRUSTED ADVISOR

### 🎯 Khái Niệm
**Trusted Advisor** = Best practice recommendations.

### 📊 Categories
1. **Cost Optimization**: Idle resources, Reserved Instances
2. **Performance**: Service limits, throughput
3. **Security**: Open ports, IAM usage
4. **Fault Tolerance**: Multi-AZ, backups
5. **Service Limits**: Usage vs limits

### 🔄 Support Plans
- **Basic/Developer**: 7 core checks
- **Business/Enterprise**: All checks + API access

**🎯 Exam Tip**: 
- "Best practice recommendations" = Trusted Advisor
- "Cost optimization" = Trusted Advisor

---

## 7️⃣ AWS HEALTH DASHBOARD

### 🎯 Khái Niệm
**Health Dashboard** = Service health and notifications.

### 📊 Types
1. **Service Health Dashboard**: Global AWS status
2. **Personal Health Dashboard**: Account-specific events

**🎯 Exam Tip**: 
- "AWS service outage" = Service Health Dashboard
- "Account-specific events" = Personal Health Dashboard

---

## 8️⃣ AWS LICENSE MANAGER

### 🎯 Khái Niệm
**License Manager** = Manage software licenses.

### 📊 Key Features
- **Track licenses**: BYOL (Bring Your Own License)
- **Prevent overages**: Enforce limits
- **Integration**: EC2, RDS

---

## 9️⃣ AWS COMPUTE OPTIMIZER

### 🎯 Khái Niệm
**Compute Optimizer** = ML-based resource optimization recommendations.

### 📊 Key Features
- **Recommendations**: EC2, EBS, Lambda, Auto Scaling
- **ML-based**: Analyze usage patterns
- **Cost savings**: Right-sizing

**🎯 Exam Tip**: 
- "Right-sizing recommendations" = Compute Optimizer

---

## 🔟 AWS COST EXPLORER & BUDGETS

### 🎯 Cost Explorer
- **Visualize costs**: Historical and forecast
- **Filter**: By service, tag, region
- **Recommendations**: Reserved Instances, Savings Plans

### 🎯 AWS Budgets
- **Set budgets**: Cost, usage, RI utilization
- **Alerts**: SNS notifications
- **Actions**: Automated responses

**🎯 Exam Tip**: 
- "Cost analysis" = Cost Explorer
- "Budget alerts" = AWS Budgets

---

## 📋 MANAGEMENT COMPARISON

| Service | Purpose |
|---------|---------|
| **CloudFormation** | Infrastructure as Code |
| **Systems Manager** | Manage instances at scale |
| **Organizations** | Multi-account management |
| **Control Tower** | Multi-account best practices |
| **Service Catalog** | Approved product catalog |
| **Trusted Advisor** | Best practice recommendations |
| **Compute Optimizer** | Right-sizing recommendations |

---

## 🎯 EXAM SCENARIOS

### Scenario 1: IaC
**Q**: "Automate infrastructure provisioning with version control?"
**A**: AWS CloudFormation

### Scenario 2: Multi-Account
**Q**: "Manage 50 AWS accounts with consolidated billing?"
**A**: AWS Organizations

### Scenario 3: Patch Management
**Q**: "Automate OS patching across 100 EC2 instances?"
**A**: AWS Systems Manager Patch Manager

---

## 📚 CHECKLIST
- [ ] Hiểu CloudFormation templates và stacks
- [ ] Biết Systems Manager components (Session Manager, Parameter Store)
- [ ] Nắm AWS Organizations và SCPs
- [ ] Hiểu Trusted Advisor categories
- [ ] Biết Cost Explorer và Budgets

**Next**: [07-application-integration.md](07-application-integration.md) 🚀
