# 🚚 MIGRATION & TRANSFER - AWS SAA-C03
## 📌 Tổng Quan
Migration chiếm **5-7% câu hỏi**. Focus vào DMS, Snow Family, và DataSync.

## 1️⃣ AWS DMS (Database Migration Service)
**DMS** = Migrate databases với minimal downtime.
- **Homogeneous**: Same engine (MySQL → MySQL)
- **Heterogeneous**: Different engine (Oracle → Aurora) + **SCT**
- **CDC**: Continuous Data Capture (ongoing replication)
**🎯 Exam Tip**: "Database migration" = DMS

## 2️⃣ AWS SNOW FAMILY
**Snow Family** = Physical devices for data migration.
- **Snowcone**: 8-14 TB, portable
- **Snowball Edge**: 80 TB (Storage Optimized), 42 TB (Compute Optimized)
- **Snowmobile**: 100 PB, data center migration
**🎯 Exam Tip**: "Limited bandwidth" + "large data" = Snow Family

## 3️⃣ AWS DATASYNC
**DataSync** = Automated data transfer (on-prem ↔ AWS).
- **Destinations**: S3, EFS, FSx
- **Agent-based**, **Scheduled**, **Bandwidth throttling**
**🎯 Exam Tip**: "Automated sync" = DataSync

## 4️⃣ AWS TRANSFER FAMILY
**Transfer Family** = Managed file transfer (SFTP, FTPS, FTP).
- **Backend**: S3, EFS
- **Use case**: Legacy applications using FTP

## 5️⃣ AWS APPLICATION MIGRATION SERVICE (MGN)
**MGN** = Lift-and-shift migration (formerly CloudEndure).
- **Continuous replication**
- **Minimal downtime**
- **Use case**: Migrate servers to AWS

## 6️⃣ AWS MIGRATION HUB
**Migration Hub** = Track migrations across AWS tools.
- **Central dashboard**
- **Integration**: DMS, MGN, Server Migration Service

## 📋 MIGRATION COMPARISON
| Service | Purpose |
|---------|---------|
| **DMS** | Database migration |
| **Snow Family** | Physical data migration |
| **DataSync** | Automated file transfer |
| **Transfer Family** | SFTP/FTP to S3/EFS |
| **MGN** | Server migration |
| **Migration Hub** | Track migrations |

**Next**: [11-api-integration.md](11-api-integration.md) 🚀
