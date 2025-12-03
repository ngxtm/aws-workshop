---
title: "Week 4 Worklog"
date: 2025-09-29
weight: 4
chapter: false
pre: " <b> 1.4 </b> "
---

### Week 4 Objectives:
- Deep practice with Amazon EC2: create instances, connect, backup, AMI, IAM governance
- Learn Module 04: AWS Storage Services (S3, Glacier, Snow Family, Storage Gateway)
- Learn Module 05: AWS Security Services (IAM, Cognito, KMS, Security Hub)
- Attend event: AI-Driven Development Life Cycle

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2-3 | - Practice Lab 9: Amazon Elastic Compute Cloud (EC2)<br>&emsp; + Create VPC for Linux and Windows<br>&emsp; + Create Security Groups for Linux and Windows<br>&emsp; + Launch and connect Windows instance<br>&emsp; + Launch and connect Linux instance<br>&emsp; + EC2 basics: Change configuration, create snapshot, create Custom AMI<br>&emsp; + Access when losing Key Pair (SSM, User Data)<br>&emsp; + Configure Desktop for EC2-Ubuntu 22.04<br>&emsp; + Amazon EBS Snapshots Archive, Share AMI | 29/09/2025 | 30/09/2025 | [Lab 9](https://000004.awsstudygroup.com/vi/) |
| 3 | - Continue Lab 9:<br>&emsp; + Deploy Node.js application on Amazon Linux<br>&emsp; &emsp; * Install LAMP web server, configure database, phpMyAdmin<br>&emsp; &emsp; * Install Node.js on Linux<br>&emsp; + Node.js application on Amazon EC2 Windows<br>&emsp; &emsp; * Install XAMPP, Nodejs on Windows<br>&emsp; + Limit resource usage with IAM<br>&emsp; &emsp; * Limit by Region, Instance Family, Instance type<br>&emsp; &emsp; * Limit EBS volume storage type<br>&emsp; &emsp; * Limit delete permissions by IP, time | 30/09/2025 | 30/09/2025 | [Lab 9](https://000004.awsstudygroup.com/vi/) |
| 4 | - Learn Module 04: AWS Storage Services<br>&emsp; + 04-01: Amazon S3 - Access Point - Storage Class<br>&emsp; + 04-02: S3 Static Website & CORS - Control Access - Object Key & Performance - Glacier<br>&emsp; + 04-03: Snow Family - Storage Gateway - Backup | 01/10/2025 | 01/10/2025 | [FCJ Youtube](https://www.youtube.com/watch?v=_yunukwcAwc) |
| 5 | - Attend event: AI-Driven Development Life Cycle: Reimagining Software Engineering | 02/10/2025 | 02/10/2025 | |
| 6 | - Learn Module 05: AWS Security Services<br>&emsp; + 05-01: Shared Responsibility Model<br>&emsp; + 05-02: Amazon Identity and Access Management<br>&emsp; + 05-03: Amazon Cognito<br>&emsp; + 05-04: AWS Organization<br>&emsp; + 05-05: AWS Identity Center<br>&emsp; + 05-06: Amazon Key Management Service<br>&emsp; + 05-07: AWS Security Hub | 03/10/2025 | 03/10/2025 | [FCJ Youtube](https://www.youtube.com/watch?v=tsobAlSg19g) |

### Week 4 Achievements:

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

#### **Module 04: AWS Storage Services**
- **S3 Storage Classes** - Chọn đúng class tiết kiệm 60-95% cost:
  - **Standard**: Frequently accessed, ≥3 AZ, 99.999999999% durability
  - **Intelligent-Tiering**: Unknown access pattern, auto-move objects, không retrieval fee - best choice khi không chắc pattern
  - **Standard-IA**: Infrequent access (>30 days), minimum 128KB charge, 30-day minimum storage
  - **Glacier Instant**: Archive nhưng cần millisecond access, rẻ hơn Standard 68%
  - **Glacier Deep Archive**: $1/TB/month, 12-48h retrieval, compliance data 7-10 năm
- **S3 Performance Optimization**:
  - 3,500 PUT/POST/DELETE và 5,500 GET/HEAD per prefix per second
  - **Prefix strategy**: `bucket/user1/photos/` vs `bucket/user2/photos/` - mỗi prefix có separate throughput
  - Multipart upload: Bắt buộc >5GB, recommend >100MB, parallel upload faster
- **Snow Family** - Offline data transfer:
  - Snowball Edge: 80TB, có compute capability cho edge processing
  - Snowmobile: 100PB, literally a truck - dùng khi >10PB data
  - **Rule of thumb**: >1 week to transfer over network → use Snow Family
- **Storage Gateway** - Hybrid cloud storage:
  - File Gateway: NFS/SMB interface to S3, cache frequently accessed data locally
  - Volume Gateway: iSCSI block storage, Stored mode (full local) vs Cached mode (only cache local)

#### **Module 05: AWS Security Services**
- **Shared Responsibility Model**:
  - AWS: Security OF the cloud (hardware, software, networking, facilities)
  - Customer: Security IN the cloud (data, IAM, encryption, network config)
  - **Key insight**: EC2 → customer manage OS patches; RDS → AWS manage OS, customer manage DB config
- **IAM Advanced Concepts**:
  - **Policy Evaluation Logic**: Explicit Deny → Explicit Allow → Implicit Deny
  - **Permission Boundary**: Set maximum permissions cho IAM users/roles - prevent privilege escalation
  - **Resource-based Policy**: Attach to resource (S3 bucket policy), support cross-account access trực tiếp
- **Cognito** - User management:
  - **User Pools**: Authentication, sign-up/sign-in, MFA, password policies - return JWT tokens
  - **Identity Pools**: Authorization, exchange tokens for temporary AWS credentials
  - **Use case**: Mobile/web app cần user login + access AWS resources (S3, DynamoDB)
- **AWS Organizations & SCPs**:
  - SCP (Service Control Policies): Restrict actions across entire OU/account
  - SCP không grant permissions, chỉ restrict - vẫn cần IAM policies
  - **Example**: Block root user from doing anything except specific actions
- **KMS Encryption**:
  - AWS Managed Keys: Free, auto-rotate yearly, cannot control
  - Customer Managed Keys (CMK): $1/month + API calls, custom rotation, audit via CloudTrail
  - **Envelope encryption**: Data key encrypts data, CMK encrypts data key - efficient for large data

#### **Event: AI-Driven Development Life Cycle**
- Hands-on với Amazon CodeWhisperer (nay là Amazon Q Developer): Generate code từ comments, support Python, JS, Java, C#
- AI-assisted development workflow: Write comment → generate code → review → refine
- **Practical takeaway**: AI tools boost productivity 25-30% cho routine tasks, nhưng vẫn cần human review đặc biệt cho security-sensitive code
- Networking với các developers khác, discussion về AI integration trong real projects
