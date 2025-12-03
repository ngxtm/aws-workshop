---
title: "Nhật ký tuần 8"
date: 2025-10-27
weight: 8
chapter: false
pre: " <b> 1.8 </b> "
---

### Mục tiêu tuần 8:
- Họp team project: định hướng thiết kế UI/UX, database, thu thập data
- Tiếp tục Lab 17: VPC Endpoints, Peering, Network Manager
- Thực hành Lab 18: Triển khai Wordpress trên AWS Cloud
- Thực hành Lab 19: AWS CloudFormation

### Các đầu việc của tuần 8:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2 | - Họp team project: Định hướng thiết kế UI, UX, thiết kế sơ bộ database, lên kế hoạch thu thập data | 27/10/2025 | 27/10/2025 | |
| 3 | - Tiếp tục Thực hành Lab 17:<br>&emsp; + VPC Endpoints cho AWS Services<br>&emsp; &emsp; * Triển khai và kiểm tra VPC Endpoint<br>&emsp; + VPC Endpoint Services<br>&emsp; + VPC Peering<br>&emsp; &emsp; * VPC Peering Request, Routing Configuration<br>&emsp; + Transit Gateway Network Manager<br>&emsp; &emsp; * Cài đặt Network Manager, Cấu hình Site<br>&emsp; &emsp; * Network Insights | 28/10/2025 | 28/10/2025 | [Lab 17](https://000092.awsstudygroup.com/vi/) |
| 4 | - Thực hành Lab 18: Triển khai Wordpress trên AWS Cloud<br>&emsp; + Chuẩn bị VPC, Subnet, Security Groups<br>&emsp; + Khởi tạo EC2 Instance và Database Instance<br>&emsp; + Cài đặt Wordpress trên EC2<br>&emsp; + Thực hiện tạo Autoscaling cho Wordpress<br>&emsp; &emsp; * Tạo AMI, Launch Template, Target Group<br>&emsp; &emsp; * Tạo Load Balancer, Auto Scaling Group<br>&emsp; + Sao lưu và phục hồi database<br>&emsp; + Khởi tạo CloudFront cho Web Server | 29/10/2025 | 29/10/2025 | [Lab 18](https://000101.awsstudygroup.com/vi/) |
| 5 | - Họp team project: Tiến hành thu thập data theo thiết kế sơ bộ của database | 30/10/2025 | 30/10/2025 | |
| 6 | - Thực hành Lab 19: AWS CloudFormation<br>&emsp; + Tạo IAM User và IAM Role<br>&emsp; + CloudFormation cơ bản<br>&emsp; &emsp; * Tạo Workspace, CloudFormation Template<br>&emsp; + CloudFormation nâng cao<br>&emsp; &emsp; * Các tài nguyên tùy chỉnh (Lambda Function, Stack)<br>&emsp; &emsp; * Ánh xạ và Stacksets<br>&emsp; &emsp; * Drift Detection | 31/10/2025 | 31/10/2025 | [Lab 19](https://000037.awsstudygroup.com/vi/) |

### Thành tựu đạt được trong tuần 8:

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
