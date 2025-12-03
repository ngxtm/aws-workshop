---
title: "Week 5 Worklog"
date: 2025-10-06
weight: 5
chapter: false
pre: " <b> 1.5 </b> "
---

### Week 5 Objectives:
- Learn Module 06: AWS Database Services (RDS, Aurora, Redshift, ElastiCache)
- Practice Lab 10: Static Website Hosting with Amazon S3 and CloudFront
- Practice Lab 11: Amazon RDS - deploy application with database
- Practice Lab 12: Auto Scaling Group with Load Balancer
- Team project meeting: write proposal, define problem statement

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2 | - Learn Module 06: AWS Database Services<br>&emsp; + 06-01: Database Concepts review<br>&emsp; + 06-02: Amazon RDS & Amazon Aurora<br>&emsp; + 06-03: Redshift - ElastiCache | 06/10/2025 | 06/10/2025 | [FCJ Youtube](https://www.youtube.com/watch?v=OOD2RwWuLRw) |
| 3 | - Practice Lab 10: Static Website Hosting with Amazon S3<br>&emsp; + Create S3 bucket and upload data<br>&emsp; + Enable static website feature<br>&emsp; + Configure Block Public Access and public object<br>&emsp; + Speed up with CloudFront<br>&emsp; + Bucket versioning, move Objects<br>&emsp; + Copy S3 Objects to another region | 07/10/2025 | 07/10/2025 | [Lab 10](https://000057.awsstudygroup.com/vi/) |
| 4 | - Team project meeting: Start writing proposal<br>&emsp; + Define problem statement<br>&emsp; + Define main flows of web app | 08/10/2025 | 08/10/2025 | [Proposal Template](https://workshop-sample.fcjuni.com/2-proposal/) |
| 5 | - Practice Lab 11: Amazon RDS Fundamentals<br>&emsp; + Create VPC, EC2 Security Group, RDS Security Group<br>&emsp; + Create DB Subnet Group<br>&emsp; + Create EC2 instance and RDS database instance<br>&emsp; + Deploy application connecting to RDS<br>&emsp; + Backup and restore database | 09/10/2025 | 09/10/2025 | [Lab 11](https://000005.awsstudygroup.com/vi/) |
| 6 | - Practice Lab 12: Deploy FCJ Management app with Auto Scaling Group<br>&emsp; + Setup network infrastructure, EC2, RDS<br>&emsp; + Install data for Database<br>&emsp; + Create Launch Template<br>&emsp; + Setup Load Balancer (Target Group, ALB)<br>&emsp; + Create Auto Scaling Group<br>&emsp; + Test: manual scaling, scheduled scaling, dynamic scaling, predictive scaling | 10/10/2025 | 10/10/2025 | [Lab 12](https://000006.awsstudygroup.com/vi/) |

### Week 5 Achievements:

#### **Module 06: AWS Database Services**
- **Database Concepts**:
  - OLTP vs OLAP workloads
  - Relational vs NoSQL databases
  - Managed vs Self-managed databases
- **Amazon RDS**:
  - Supported engines: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server
  - Multi-AZ deployment for High Availability
  - Read Replicas for performance
  - Automated backups and manual snapshots
- **Amazon Aurora**:
  - MySQL and PostgreSQL compatible
  - 5x throughput compared to MySQL, 3x compared to PostgreSQL
  - Aurora Serverless for variable workloads
- **Amazon Redshift**: Data warehouse for analytics
- **Amazon ElastiCache**: In-memory caching (Redis, Memcached)

#### **Lab 10: S3 Static Website with CloudFront**
- Create S3 bucket and configure static website hosting
- Configure Block Public Access settings
- Setup CloudFront distribution for faster delivery
- Use S3 versioning to manage object versions
- Cross-Region Replication to copy data

#### **Lab 11: Amazon RDS Fundamentals**
- Design network architecture for RDS:
  - VPC with public and private subnets
  - Separate Security Groups for EC2 and RDS
  - DB Subnet Group spanning multiple AZs
- Deploy RDS database instance
- Connect application from EC2 to RDS
- Practice backup and restore from snapshot

#### **Lab 12: Auto Scaling Group with Load Balancer**
- **Launch Template**: define AMI, instance type, security groups
- **Application Load Balancer**:
  - Target Group with health checks
  - Listener rules and routing
- **Auto Scaling Group**:
  - Manual scaling: change desired capacity
  - Scheduled scaling: scale by schedule (peak hours)
  - Dynamic scaling: scale based on CloudWatch metrics (CPU, requests)
  - Predictive scaling: ML-based prediction for workload

#### **Team Project Meeting**
- Started writing proposal for MapVibe project
- Defined problem statement: users have difficulty finding suitable restaurants
- Defined main flows: search, filter, view details, reviews
