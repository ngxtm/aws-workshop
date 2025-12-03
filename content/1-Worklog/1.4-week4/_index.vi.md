---
title: "Nhật ký tuần 4"
date: 2025-09-29
weight: 4
chapter: false
pre: " <b> 1.4 </b> "
---

### Mục tiêu tuần 4:
- Thực hành chuyên sâu Amazon EC2: tạo instance, kết nối, backup, AMI, IAM governance
- Học Module 04: Dịch vụ lưu trữ trên AWS (S3, Glacier, Snow Family, Storage Gateway)
- Học Module 05: Dịch vụ bảo mật trên AWS (IAM, Cognito, KMS, Security Hub)
- Tham gia event: AI-Driven Development Life Cycle

### Các đầu việc của tuần 4:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2-3 | - Thực hành Lab 9: Amazon Elastic Compute Cloud (EC2)<br>&emsp; + Tạo VPC Linux và VPC Windows<br>&emsp; + Tạo Security Group cho Linux và Windows<br>&emsp; + Khởi tạo và kết nối Windows instance<br>&emsp; + Khởi tạo và kết nối Linux instance<br>&emsp; + Amazon EC2 cơ bản: Thay đổi cấu hình, tạo snapshot, tạo Custom AMI<br>&emsp; + Truy cập khi mất Key Pair (SSM, User Data)<br>&emsp; + Cấu hình Desktop cho EC2-Ubuntu 22.04<br>&emsp; + Amazon EBS Snapshots Archive, Chia sẻ AMI | 29/09/2025 | 30/09/2025 | [Lab 9](https://000004.awsstudygroup.com/vi/) |
| 3 | - Tiếp tục Lab 9:<br>&emsp; + Triển khai ứng dụng Node.js trên Amazon Linux<br>&emsp; &emsp; * Cài đặt LAMP web server, cấu hình database, phpMyAdmin<br>&emsp; &emsp; * Cài đặt Node.js trên Linux<br>&emsp; + Ứng dụng Node.js trên Amazon EC2 Windows<br>&emsp; &emsp; * Cài đặt XAMPP, Nodejs trên Windows<br>&emsp; + Giới hạn sử dụng tài nguyên bằng IAM<br>&emsp; &emsp; * Giới hạn theo Region, Instance Family, Instance type<br>&emsp; &emsp; * Giới hạn EBS volume storage type<br>&emsp; &emsp; * Giới hạn quyền xóa theo IP, thời gian | 30/09/2025 | 30/09/2025 | [Lab 9](https://000004.awsstudygroup.com/vi/) |
| 4 | - Học Module 04: Dịch vụ lưu trữ trên AWS<br>&emsp; + 04-01: Amazon S3 - Access Point - Storage Class<br>&emsp; + 04-02: S3 Static Website & CORS - Control Access - Object Key & Performance - Glacier<br>&emsp; + 04-03: Snow Family - Storage Gateway - Backup | 01/10/2025 | 01/10/2025 | [FCJ Youtube](https://www.youtube.com/watch?v=_yunukwcAwc) |
| 5 | - Tham gia event: AI-Driven Development Life Cycle: Reimagining Software Engineering | 02/10/2025 | 02/10/2025 | |
| 6 | - Học Module 05: Dịch vụ bảo mật trên AWS<br>&emsp; + 05-01: Share Responsibility Model<br>&emsp; + 05-02: Amazon Identity and Access Management<br>&emsp; + 05-03: Amazon Cognito<br>&emsp; + 05-04: AWS Organization<br>&emsp; + 05-05: AWS Identity Center<br>&emsp; + 05-06: Amazon Key Management Service<br>&emsp; + 05-07: AWS Security Hub | 03/10/2025 | 03/10/2025 | [FCJ Youtube](https://www.youtube.com/watch?v=tsobAlSg19g) |

### Thành tựu đạt được trong tuần 4:

#### **Lab 9: Amazon EC2 - Triển khai và quản lý**
- **Instance Management**:
  - Tạo VPC riêng cho Linux và Windows với Security Groups tương ứng - quan trọng để isolate environments
  - Windows dùng RDP (port 3389), Linux dùng SSH (port 22) - Security Group phải mở đúng port
  - **Tip thực tế**: Đổi instance type phải stop instance trước, EBS volume có thể resize online nhưng chỉ tăng không giảm
- **EBS Snapshots & AMI**:
  - Snapshot là incremental backup - chỉ lưu block thay đổi, tiết kiệm storage cost
  - Tạo AMI từ running instance được nhưng recommend stop để đảm bảo data consistency
  - **EBS Snapshots Archive**: Rẻ hơn 75% so với standard, nhưng restore mất 24-72h - chỉ dùng cho long-term backup
  - Share AMI cross-account: Modify permissions, thêm account ID - hữu ích cho multi-account strategy
- **Recover Access khi mất Key Pair**:
  - **SSM Session Manager**: Không cần mở port 22/3389, secure hơn, cần IAM Role với `AmazonSSMManagedInstanceCore`
  - **User Data method**: Mount root volume vào instance khác, thêm public key vào `~/.ssh/authorized_keys`
  - **Lesson learned**: Luôn enable SSM từ đầu, backup key pair ở secure location (Secrets Manager)
- **Deploy Applications**:
  - LAMP stack: Apache + MySQL + PHP, config trong `/var/www/html/`
  - Node.js trên Linux: Dùng nvm để manage versions, pm2 để process management
  - Windows XAMPP: Dễ setup hơn nhưng không recommend cho production

#### **IAM Governance - Cost & Security Control**
- **Resource Limitation Policies**:
  - **Limit by Region**: `"Condition": {"StringEquals": {"aws:RequestedRegion": "ap-southeast-1"}}` - chặn tạo resource ở region khác
  - **Limit EC2 Family**: Chỉ cho phép t3, t3a (burstable) cho dev environment, block c5, r5 (expensive)
  - **Limit Instance Type**: `ec2:InstanceType` condition key, dev chỉ được t3.micro/small
  - **EBS Volume Type**: Block io1, io2 (expensive IOPS), chỉ cho phép gp3
- **Advanced Conditions**:
  - **IP-based restriction**: `aws:SourceIp` condition, chỉ cho phép từ company IP range
  - **Time-based restriction**: `aws:CurrentTime` condition, block delete actions ngoài giờ hành chính
  - **Practical insight**: Combine multiple conditions với AND logic để tạo granular control

#### **Module 04: Dịch vụ lưu trữ trên AWS**
- **S3 Storage Classes** - Chọn đúng class tiết kiệm 60-95% cost:
  - **Standard**: Frequently accessed, ≥3 AZ, 99.999999999% durability
  - **Intelligent-Tiering**: Unknown access pattern, auto-move objects, không retrieval fee - best choice khi không chắc pattern
  - **Standard-IA**: Infrequent access (>30 days), minimum 128KB charge, 30-day minimum storage
  - **Glacier Instant**: Archive nhưng cần millisecond access, rẻ hơn Standard 68%
  - **Glacier Deep Archive**: $1/TB/month, 12-48h retrieval, compliance data 7-10 năm
- **S3 Performance**:
  - 3,500 PUT/POST/DELETE và 5,500 GET/HEAD per prefix per second
  - **Prefix strategy**: `bucket/user1/photos/` vs `bucket/user2/photos/` - mỗi prefix có separate throughput
  - Multipart upload: Bắt buộc >5GB, recommend >100MB, parallel upload faster
- **Snow Family** - Offline data transfer:
  - Snowball Edge: 80TB, có compute capability cho edge processing
  - Snowmobile: 100PB, literally a truck - dùng khi >10PB data
  - **Rule of thumb**: >1 week to transfer over network → use Snow Family
- **Storage Gateway** - Hybrid cloud storage:
  - File Gateway: NFS/SMB interface to S3, cache frequently accessed data locally
  - Volume Gateway: iSCSI block storage, Stored mode vs Cached mode

#### **Module 05: Dịch vụ bảo mật trên AWS**
- **Shared Responsibility Model**:
  - AWS: Security OF the cloud (hardware, software, networking, facilities)
  - Customer: Security IN the cloud (data, IAM, encryption, network config)
  - **Key insight**: EC2 → customer manage OS patches; RDS → AWS manage OS, customer manage DB config
- **IAM Advanced Concepts**:
  - **Policy Evaluation Logic**: Explicit Deny → Explicit Allow → Implicit Deny
  - **Permission Boundary**: Set maximum permissions cho IAM users/roles - prevent privilege escalation
  - **Resource-based Policy**: Attach to resource (S3 bucket policy), support cross-account access trực tiếp
- **Cognito** - User management:
  - **User Pools**: Authentication, sign-up/sign-in, MFA - return JWT tokens
  - **Identity Pools**: Authorization, exchange tokens for temporary AWS credentials
  - **Use case**: Mobile/web app cần user login + access AWS resources
- **AWS Organizations & SCPs**:
  - SCP (Service Control Policies): Restrict actions across entire OU/account
  - SCP không grant permissions, chỉ restrict - vẫn cần IAM policies
- **KMS Encryption**:
  - AWS Managed Keys: Free, auto-rotate yearly, cannot control
  - Customer Managed Keys (CMK): $1/month + API calls, custom rotation, audit via CloudTrail
  - **Envelope encryption**: Data key encrypts data, CMK encrypts data key - efficient for large data

#### **Event: AI-Driven Development Life Cycle**
- Hands-on với Amazon CodeWhisperer (nay là Amazon Q Developer): Generate code từ comments
- AI-assisted development workflow: Write comment → generate code → review → refine
- **Practical takeaway**: AI tools boost productivity 25-30% cho routine tasks, nhưng vẫn cần human review
- Networking với các developers khác, discussion về AI integration trong real projects
