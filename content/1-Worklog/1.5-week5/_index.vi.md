---
title: "Nhật ký tuần 5"
date: 2025-10-06
weight: 5
chapter: false
pre: " <b> 1.5 </b> "
---

### Mục tiêu tuần 5:
- Học Module 06: Dịch vụ cơ sở dữ liệu trên AWS (RDS, Aurora, Redshift, ElastiCache)
- Thực hành Lab 10: Hosting Website tĩnh với Amazon S3 và CloudFront
- Thực hành Lab 11: Amazon RDS - triển khai ứng dụng với database
- Thực hành Lab 12: Auto Scaling Group với Load Balancer
- Họp team project: viết proposal, xác định problem statement

### Các đầu việc của tuần 5:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2 | - Học Module 06: Dịch vụ cơ sở dữ liệu trên AWS<br>&emsp; + 06-01: Database Concepts review<br>&emsp; + 06-02: Amazon RDS & Amazon Aurora<br>&emsp; + 06-03: Redshift - ElastiCache | 06/10/2025 | 06/10/2025 | [FCJ Youtube](https://www.youtube.com/watch?v=OOD2RwWuLRw) |
| 3 | - Thực hành Lab 10: Hosting Website tĩnh với Amazon S3<br>&emsp; + Tạo S3 bucket và tải dữ liệu<br>&emsp; + Bật tính năng static website<br>&emsp; + Cấu hình Block Public Access và public object<br>&emsp; + Tăng tốc với CloudFront<br>&emsp; + Bucket versioning, di chuyển Object<br>&emsp; + Sao chép S3 Object sang region khác | 07/10/2025 | 07/10/2025 | [Lab 10](https://000057.awsstudygroup.com/vi/) |
| 4 | - Họp team project: Tiến hành viết proposal<br>&emsp; + Xác định problem statement<br>&emsp; + Định nghĩa các main flow của web app | 08/10/2025 | 08/10/2025 | [Proposal Template](https://workshop-sample.fcjuni.com/2-proposal/) |
| 5 | - Thực hành Lab 11: Kiến thức cơ bản về Amazon RDS<br>&emsp; + Tạo VPC, EC2 Security Group, RDS Security Group<br>&emsp; + Tạo DB Subnet Group<br>&emsp; + Tạo EC2 instance và RDS database instance<br>&emsp; + Triển khai ứng dụng kết nối RDS<br>&emsp; + Backup và restore database | 09/10/2025 | 09/10/2025 | [Lab 11](https://000005.awsstudygroup.com/vi/) |
| 6 | - Thực hành Lab 12: Triển khai ứng dụng FCJ Management với Auto Scaling Group<br>&emsp; + Thiết lập hạ tầng mạng, EC2, RDS<br>&emsp; + Cài đặt dữ liệu cho Database<br>&emsp; + Tạo Launch Template<br>&emsp; + Thiết lập Load Balancer (Target Group, ALB)<br>&emsp; + Tạo Auto Scaling Group<br>&emsp; + Kiểm thử: manual scaling, scheduled scaling, dynamic scaling, predictive scaling | 10/10/2025 | 10/10/2025 | [Lab 12](https://000006.awsstudygroup.com/vi/) |

### Thành tựu đạt được trong tuần 5:

#### **Module 06: Dịch vụ cơ sở dữ liệu trên AWS**
- **Database Concepts**:
  - **OLTP** (Online Transaction Processing): Nhiều writes nhỏ, low latency - dùng RDS/Aurora
  - **OLAP** (Online Analytical Processing): Complex queries, large data - dùng Redshift
  - **Rule of thumb**: OLTP cho application database, OLAP cho reporting/BI
- **Amazon RDS**:
  - Engines: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora
  - **Multi-AZ**: Synchronous replication, automatic failover ~60s, cho HA
  - **Read Replicas**: Asynchronous replication, scale read workload, cross-region cho DR
  - **Backup**: Automated (retention 0-35 days), Manual snapshots (không expire)
  - **Tip**: Enable Multi-AZ cho production, Read Replica cho heavy read workloads
- **Amazon Aurora**:
  - Storage auto-scales từ 10GB đến 128TB, 6 copies across 3 AZs
  - **Aurora Serverless v2**: Scale từ 0.5 đến 128 ACUs, pay per second
  - **Use case**: Variable workloads, dev/test environments, cost optimization
- **Amazon Redshift**: 
  - Columnar storage, MPP (Massively Parallel Processing)
  - **Redshift Spectrum**: Query S3 data without loading
  - Cost: ~$0.25/hour cho dc2.large
- **ElastiCache**: Redis cho complex data structures + persistence, Memcached cho simple caching

#### **Lab 10: S3 Static Website với CloudFront**
- **S3 Static Website Hosting**:
  - Bucket name phải unique globally
  - Index document: `index.html`, Error document: `error.html`
  - **Gotcha**: Block Public Access phải disable để website public, nhưng use CloudFront OAI cho security
- **CloudFront Integration**:
  - Edge locations: cache content gần users, reduce latency từ 200ms → 20ms
  - **OAI (Origin Access Identity)**: S3 bucket private, chỉ CloudFront access được
  - Cache behaviors: TTL settings, query string forwarding
  - **Invalidation**: `/images/*` để clear cache khi update content, cost $0.005/path
- **S3 Versioning**:
  - Enable để track changes, recover deleted objects
  - **MFA Delete**: Require MFA để delete permanently - prevent accidental deletion
- **Cross-Region Replication**:
  - Async replication, cần enable versioning
  - **Use case**: Compliance (data in specific region), DR, latency reduction

#### **Lab 11: Amazon RDS Fundamentals**
- **Network Architecture cho RDS**:
  - RDS trong private subnet (không public IP)
  - EC2 Security Group → RDS Security Group (chỉ allow port 3306/5432 từ EC2 SG)
  - **DB Subnet Group**: Minimum 2 subnets trong 2 AZs cho Multi-AZ
- **RDS Instance Setup**:
  - Instance class: db.t3.micro (free tier), db.r5.large (production)
  - Storage: gp3 (general purpose), io1 (high IOPS)
  - **Parameter Group**: Custom settings (max_connections, slow_query_log)
- **Backup & Restore**:
  - Point-in-time recovery: Restore to any second trong retention period
  - Snapshot restore: Tạo new instance từ snapshot
  - **Tip**: Test restore process định kỳ, đừng đợi disaster mới test

#### **Lab 12: Auto Scaling Group với Load Balancer**
- **Launch Template**:
  - AMI + Instance type + Key pair + Security Groups + User Data
  - **Version control**: Mỗi lần update tạo new version, easy rollback
- **Application Load Balancer**:
  - **Target Group**: Health check path `/health`, interval 30s, threshold 2
  - **Listener**: Port 80 (HTTP), Port 443 (HTTPS với ACM certificate)
  - **Path-based routing**: `/api/*` → Backend TG, `/*` → Frontend TG
- **Auto Scaling Policies**:
  - **Target Tracking**: Maintain CPU at 70% - simplest, AWS auto-adjusts
  - **Step Scaling**: CPU > 80% add 2, CPU > 90% add 4 - more control
  - **Scheduled**: Scale up 9AM, scale down 6PM - predictable patterns
  - **Predictive**: ML forecast dựa trên historical patterns - best cho recurring patterns
- **Testing**: 
  - `stress --cpu 4 --timeout 300` để trigger scale out
  - Observe scale out ~3 mins, scale in ~15 mins (cooldown)

#### **Họp Team Project**
- Bắt đầu viết proposal cho MapVibe
- **Problem Statement**: Người dùng khó tìm quán ăn phù hợp với preferences, thiếu personalized recommendations
- **Main Flows**: Search by location/keyword → Filter (cuisine, price, rating) → View details → Read/Write reviews
- **Target Users**: Gen Z, millennials thích explore quán mới
