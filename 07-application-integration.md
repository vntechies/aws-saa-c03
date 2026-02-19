# 🔗 APPLICATION INTEGRATION - AWS SAA-C03

## 📌 Tổng Quan
Application Integration chiếm **10-12% câu hỏi**. Focus vào SQS, SNS, và EventBridge.

---

## 1️⃣ AMAZON SQS (Simple Queue Service)

### 🎯 Khái Niệm
**SQS** = Managed message queue service, decouple applications.

### 📊 Queue Types

#### 1. **Standard Queue**
- **Unlimited throughput**
- **At-least-once delivery**: Possible duplicates
- **Best-effort ordering**: Not guaranteed FIFO
- **Use case**: High throughput, order not critical

#### 2. **FIFO Queue**
- **300 TPS** (transactions per second) without batching, 3,000 with batching
- **Exactly-once processing**: No duplicates
- **Strict ordering**: First-In-First-Out
- **Name**: Must end with `.fifo`
- **Use case**: Order critical, no duplicates

### 🎯 Key Features

#### 1. **Visibility Timeout**
- **Default**: 30 seconds (0 sec - 12 hours)
- **Purpose**: Prevent multiple consumers processing same message
- **ChangeMessageVisibility**: Extend timeout

#### 2. **Message Retention**
- **Default**: 4 days (1 min - 14 days)

#### 3. **Long Polling**
- **Wait for messages**: Reduce API calls
- **Wait time**: 1-20 seconds
- **Recommended**: Better than short polling

#### 4. **Dead Letter Queue (DLQ)**
- **Failed messages**: After max receives
- **Use case**: Debug, reprocess failed messages

#### 5. **Delay Queue**
- **Delay delivery**: Up to 15 minutes
- **Use case**: Delayed processing

### 🔄 SQS Patterns
- **Decouple applications**: Producer → SQS → Consumer
- **Load leveling**: Handle traffic spikes
- **Asynchronous processing**

**🎯 Exam Tip**: 
- "Decouple applications" = SQS
- "Order matters" + "no duplicates" = SQS FIFO
- "High throughput" = SQS Standard
- "Failed message handling" = Dead Letter Queue

---

## 2️⃣ AMAZON SNS (Simple Notification Service)

### 🎯 Khái Niệm
**SNS** = Pub/Sub messaging service, send messages to multiple subscribers.

### 📊 Key Features

#### 1. **Topics**
- **Publishers**: Send messages to topics
- **Subscribers**: Receive messages from topics

#### 2. **Subscribers**
- **Protocols**: HTTP/HTTPS, Email, SMS, SQS, Lambda, Kinesis, mobile push

#### 3. **Fan-Out Pattern**
- **SNS → Multiple SQS queues**
- **Use case**: Parallel processing, multiple consumers

#### 4. **Message Filtering**
- **Filter policies**: Subscribers receive subset of messages
- **JSON-based**: Filter by attributes

#### 5. **FIFO Topics**
- **Order preserved**: For SQS FIFO subscribers
- **Deduplication**: No duplicate messages

### 🔄 SNS vs SQS

| Feature | SNS | SQS |
|---------|-----|-----|
| **Pattern** | Pub/Sub (1:N) | Queue (1:1) |
| **Delivery** | Push | Pull |
| **Subscribers** | Multiple | Single consumer per message |
| **Use case** | Notifications, fan-out | Decoupling, buffering |

**🎯 Exam Tip**: 
- "Multiple subscribers" = SNS
- "Fan-out pattern" = SNS → SQS
- "Push notifications" = SNS
- "Email/SMS alerts" = SNS

---

## 3️⃣ AMAZON EVENTBRIDGE (CloudWatch Events)

### 🎯 Khái Niệm
**EventBridge** = Event bus service, route events to targets.

### 📊 Key Features

#### 1. **Event Sources**
- **AWS services**: EC2, S3, Lambda, etc.
- **SaaS partners**: Datadog, Zendesk, etc.
- **Custom applications**: Your own events

#### 2. **Event Buses**
- **Default**: AWS service events
- **Custom**: Your application events
- **Partner**: SaaS partner events

#### 3. **Rules**
- **Event patterns**: Match events (JSON)
- **Schedules**: Cron expressions
- **Targets**: Lambda, SQS, SNS, Step Functions, etc.

#### 4. **Schema Registry**
- **Discover schemas**: Auto-generate from events
- **Code bindings**: Generate code for events

### 🔄 EventBridge vs CloudWatch Events
- **EventBridge**: Newer, more features (SaaS, schema registry)
- **CloudWatch Events**: Legacy, same underlying service

**🎯 Exam Tip**: 
- "Event-driven architecture" = EventBridge
- "React to AWS events" = EventBridge
- "Scheduled tasks" = EventBridge (cron)
- "SaaS integration" = EventBridge

---

## 4️⃣ AWS STEP FUNCTIONS

### 🎯 Khái Niệm
**Step Functions** = Orchestrate workflows, coordinate distributed applications.

### 📊 Key Features

#### 1. **State Machines**
- **States**: Task, Choice, Parallel, Wait, etc.
- **Visual workflow**: Graphical representation

#### 2. **Workflow Types**
- **Standard**: Long-running (up to 1 year), exactly-once
- **Express**: Short-lived (up to 5 min), at-least-once, cheaper

#### 3. **Integrations**
- **AWS services**: Lambda, ECS, SNS, SQS, DynamoDB, etc.
- **Human approval**: Wait for manual approval

**🎯 Exam Tip**: 
- "Orchestrate workflows" = Step Functions
- "Long-running workflows" = Step Functions Standard
- "Coordinate Lambda functions" = Step Functions

---

## 5️⃣ AMAZON MQ

### 🎯 Khái Niệm
**Amazon MQ** = Managed message broker (ActiveMQ, RabbitMQ).

### 📊 Key Features
- **Protocols**: MQTT, AMQP, STOMP, OpenWire, WSS
- **Migration**: Lift-and-shift from on-premises
- **High availability**: Multi-AZ

### 🔄 Amazon MQ vs SQS/SNS
```
Amazon MQ:
- Migrating from on-premises (ActiveMQ, RabbitMQ)
- Need specific protocols (MQTT, AMQP)

SQS/SNS:
- Cloud-native applications
- Serverless, fully managed
```

**🎯 Exam Tip**: 
- "Migrate ActiveMQ/RabbitMQ" = Amazon MQ
- "On-premises message broker" = Amazon MQ
- "Cloud-native" = SQS/SNS

---

## 6️⃣ AWS APPFLOW

### 🎯 Khái Niệm
**AppFlow** = Integrate SaaS applications with AWS services.

### 📊 Key Features
- **Sources**: Salesforce, SAP, Slack, ServiceNow
- **Destinations**: S3, Redshift, Snowflake
- **Transformations**: Filter, validate, map
- **Encryption**: In transit and at rest

**🎯 Exam Tip**: 
- "Integrate Salesforce with S3" = AppFlow
- "SaaS data integration" = AppFlow

---

## 📋 INTEGRATION COMPARISON

| Service | Pattern | Use Case |
|---------|---------|----------|
| **SQS** | Queue (1:1) | Decouple, buffer |
| **SNS** | Pub/Sub (1:N) | Notifications, fan-out |
| **EventBridge** | Event bus | Event-driven, SaaS |
| **Step Functions** | Orchestration | Workflows |
| **Amazon MQ** | Message broker | Migrate on-premises |
| **AppFlow** | Data integration | SaaS to AWS |

---

## 🎯 EXAM SCENARIOS

### Scenario 1: Decouple
**Q**: "Decouple web tier from processing tier?"
**A**: SQS between tiers

### Scenario 2: Fan-Out
**Q**: "Send S3 upload event to 3 different processing pipelines?"
**A**: S3 → SNS → 3 SQS queues

### Scenario 3: Order Processing
**Q**: "Process orders in exact sequence, no duplicates?"
**A**: SQS FIFO

### Scenario 4: Event-Driven
**Q**: "Trigger Lambda when EC2 state changes?"
**A**: EventBridge rule

### Scenario 5: Workflow
**Q**: "Orchestrate multiple Lambda functions with error handling?"
**A**: Step Functions

---

## 📚 CHECKLIST
- [ ] Hiểu SQS Standard vs FIFO
- [ ] Biết SNS pub/sub pattern
- [ ] Nắm SNS + SQS fan-out pattern
- [ ] Hiểu EventBridge event-driven architecture
- [ ] Biết Step Functions orchestration
- [ ] Phân biệt Amazon MQ vs SQS/SNS

**Next**: [08-developer-tools.md](08-developer-tools.md) 🚀
