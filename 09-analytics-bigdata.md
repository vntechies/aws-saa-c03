# 📊 ANALYTICS & BIG DATA - AWS SAA-C03
## 📌 Tổng Quan
Analytics chiếm **8-10% câu hỏi**. Focus vào Kinesis, Athena, và EMR.

## 1️⃣ AMAZON KINESIS
**Kinesis** = Real-time data streaming.

### Kinesis Data Streams
- **Real-time** ingestion (200ms latency)
- **Shards**: Provisioned capacity
- **Retention**: 1-365 days
- **Use case**: Real-time analytics, log processing

### Kinesis Data Firehose
- **Near real-time** (60s latency)
- **Serverless**, **Auto-scaling**
- **Destinations**: S3, Redshift, OpenSearch, Splunk
- **Transformations**: Lambda
- **Use case**: Load data to destinations

### Kinesis Data Analytics
- **SQL queries** on streaming data
- **Use case**: Real-time dashboards, metrics

**🎯 Exam Tip**: 
- "Real-time streaming" = Kinesis Data Streams
- "Load to S3/Redshift" = Kinesis Firehose
- "SQL on streams" = Kinesis Data Analytics

## 2️⃣ AMAZON ATHENA
**Athena** = Serverless SQL queries on S3.
- **Pay per query** (per TB scanned)
- **Formats**: CSV, JSON, Parquet, ORC
- **Use case**: Ad-hoc queries, log analysis

**🎯 Exam Tip**: 
- "Query S3 with SQL" = Athena
- "Serverless analytics" = Athena

## 3️⃣ AWS GLUE
**Glue** = Serverless ETL (Extract, Transform, Load).
- **Glue Data Catalog**: Metadata repository
- **Glue Crawlers**: Discover schemas
- **Glue Jobs**: ETL transformations
- **Use case**: Prepare data for analytics

**🎯 Exam Tip**: "ETL" + "serverless" = Glue

## 4️⃣ AMAZON EMR (Elastic MapReduce)
**EMR** = Managed Hadoop/Spark cluster.
- **Big data frameworks**: Hadoop, Spark, HBase, Presto
- **Use case**: Big data processing, ML
- **Pricing**: EC2 instances + EMR charges

**🎯 Exam Tip**: "Hadoop/Spark" = EMR

## 5️⃣ AMAZON OPENSEARCH (Elasticsearch)
**OpenSearch** = Search and analytics engine.
- **Full-text search**, **Log analytics**
- **Kibana**: Visualization
- **Use case**: Log analysis, search

**🎯 Exam Tip**: "Search" + "log analytics" = OpenSearch

## 6️⃣ AMAZON QUICKSIGHT
**QuickSight** = Business Intelligence (BI), serverless.
- **Dashboards**, **Visualizations**
- **SPICE engine**: In-memory
- **Use case**: BI, reporting

**🎯 Exam Tip**: "BI dashboards" = QuickSight

## 7️⃣ AWS DATA PIPELINE
**Data Pipeline** = Orchestrate data workflows (legacy, use Glue instead).

## 8️⃣ AWS LAKE FORMATION
**Lake Formation** = Build data lakes on S3.
- **Centralized permissions**
- **Data catalog**: Glue Data Catalog
- **Use case**: Data lake governance

## 📋 ANALYTICS COMPARISON
| Service | Purpose |
|---------|---------|
| **Kinesis** | Real-time streaming |
| **Athena** | Query S3 with SQL |
| **Glue** | Serverless ETL |
| **EMR** | Big data (Hadoop/Spark) |
| **OpenSearch** | Search, log analytics |
| **QuickSight** | BI dashboards |
| **Lake Formation** | Data lake governance |

**Next**: [10-migration-transfer.md](10-migration-transfer.md) 🚀
