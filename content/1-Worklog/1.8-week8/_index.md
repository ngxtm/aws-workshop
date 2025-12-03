---
title: "Week 8 Worklog"
date: 2025-10-27
weight: 8
chapter: false
pre: " <b> 1.8 </b> "
---

### Week 8 Objectives:
- Team project meeting: plan UI/UX design, database, data collection
- Continue Lab 17: VPC Endpoints, Peering, Network Manager
- Practice Lab 18: Deploy Wordpress on AWS Cloud
- Practice Lab 19: AWS CloudFormation

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2 | - Team project meeting: Plan UI/UX design, preliminary database design, data collection plan | 27/10/2025 | 27/10/2025 | |
| 3 | - Continue Practice Lab 17:<br>&emsp; + VPC Endpoints for AWS Services<br>&emsp; &emsp; * Deploy and test VPC Endpoint<br>&emsp; + VPC Endpoint Services<br>&emsp; + VPC Peering<br>&emsp; &emsp; * VPC Peering Request, Routing Configuration<br>&emsp; + Transit Gateway Network Manager<br>&emsp; &emsp; * Setup Network Manager, Configure Site<br>&emsp; &emsp; * Network Insights | 28/10/2025 | 28/10/2025 | [Lab 17](https://000092.awsstudygroup.com/vi/) |
| 4 | - Practice Lab 18: Deploy Wordpress on AWS Cloud<br>&emsp; + Prepare VPC, Subnet, Security Groups<br>&emsp; + Launch EC2 Instance and Database Instance<br>&emsp; + Install Wordpress on EC2<br>&emsp; + Create Autoscaling for Wordpress<br>&emsp; &emsp; * Create AMI, Launch Template, Target Group<br>&emsp; &emsp; * Create Load Balancer, Auto Scaling Group<br>&emsp; + Backup and restore database<br>&emsp; + Create CloudFront for Web Server | 29/10/2025 | 29/10/2025 | [Lab 18](https://000101.awsstudygroup.com/vi/) |
| 5 | - Team project meeting: Start data collection based on preliminary database design | 30/10/2025 | 30/10/2025 | |
| 6 | - Practice Lab 19: AWS CloudFormation<br>&emsp; + Create IAM User and IAM Role<br>&emsp; + CloudFormation basics<br>&emsp; &emsp; * Create Workspace, CloudFormation Template<br>&emsp; + Advanced CloudFormation<br>&emsp; &emsp; * Custom resources (Lambda Function, Stack)<br>&emsp; &emsp; * Mappings and Stacksets<br>&emsp; &emsp; * Drift Detection | 31/10/2025 | 31/10/2025 | [Lab 19](https://000037.awsstudygroup.com/vi/) |

### Week 8 Achievements:

#### **Team Project - UI/UX & Database Planning**
- **Daily Meeting (20/10)**: Skip vì đang đợi feedback architecture từ mentor
- **UX/UI Design trên Figma** (Phú phụ trách):
  - User flow: Search → Browse results → View detail → Save/Review
  - Search UI: Map-based search với filters (khoảng cách, rating, cuisine type)
  - Responsive design cho mobile-first approach
- **Daily Meeting (23/10)**: Skip vì cả team đang focus làm UI =)))
- **Chụp ảnh nhóm**: Pending, chưa sắp xếp được lịch

#### **Lab 17 (tiếp): VPC Advanced Networking**
- **VPC Endpoints** - Private access to AWS services:
  - **Gateway Endpoint**: S3 và DynamoDB only, free, add route to route table
  - **Interface Endpoint**: Hầu hết services khác, tạo ENI trong subnet, có chi phí ($0.01/hour + data)
  - **Use case thực tế**: Lambda trong private subnet access S3 không cần NAT Gateway → tiết kiệm data transfer cost
- **VPC Peering**:
  - Non-transitive: VPC A ↔ VPC B và VPC B ↔ VPC C, nhưng A không thể reach C qua B
  - Cần update route tables ở cả 2 bên
  - CIDR không được overlap - plan IP ranges từ đầu!
- **Transit Gateway Network Manager**:
  - Global view của network topology
  - Route Analyzer: Check xem traffic đi đường nào
  - **Practical insight**: Dùng cho multi-region, multi-account architectures

#### **Lab 18: Wordpress High Availability**
- **Architecture Pattern - Classic 3-tier**:
  - Web tier: EC2 behind ALB, Auto Scaling Group (min 2, max 4)
  - App tier: Wordpress on EC2, shared storage với EFS
  - DB tier: RDS MySQL Multi-AZ
- **AMI Golden Image Strategy**:
  - Install Wordpress + plugins → Create AMI → Use trong Launch Template
  - User Data chỉ để mount EFS và start services
  - **Benefit**: Faster scaling, consistent deployments
- **CloudFront Integration**:
  - Cache static assets (images, CSS, JS)
  - Origin: ALB (dynamic) + S3 (uploads)
  - Cache behavior: `/wp-content/uploads/*` → S3, `*` → ALB
- **Backup Strategy**:
  - RDS automated backups: retention 7 days
  - Manual snapshot trước major changes
  - **Restore test**: Quan trọng! Đã thử restore từ snapshot để verify

#### **Lab 19: AWS CloudFormation**
- **Template Structure**:
  ```yaml
  AWSTemplateFormatVersion, Description, Parameters, Mappings, Resources, Outputs
  ```
  - Parameters: Input values khi create stack (instance type, environment name)
  - Mappings: Lookup tables (AMI ID per region)
  - Outputs: Export values for cross-stack references
- **Custom Resources với Lambda**:
  - Khi CloudFormation không support resource type
  - Lambda nhận event từ CFN, xử lý, send response về CFN
  - **Use case**: Create resources trong third-party services, run custom logic
- **StackSets - Multi-account deployment**:
  - Deploy cùng template ra multiple accounts/regions
  - Cần setup: Admin account + Target accounts trust
  - **Guard rails**: Deploy security baselines across organization
- **Drift Detection**:
  - Detect manual changes ngoài CloudFormation
  - `IN_SYNC` vs `MODIFIED` vs `DELETED`
  - **Best practice**: Run drift detection weekly, fix drift hoặc import changes vào template
