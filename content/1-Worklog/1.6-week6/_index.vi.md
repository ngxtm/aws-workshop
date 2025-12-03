---
title: "Nhật ký tuần 6"
date: 2025-10-13
weight: 6
chapter: false
pre: " <b> 1.6 </b> "
---

### Mục tiêu tuần 6:
- Thực hành Lab 13: Tối ưu chi phí EC2 với Lambda
- Thực hành Lab 14: Serverless - Lambda với S3 và DynamoDB
- Thực hành Lab 15: Làm quen với AWS CLI
- Tham gia workshop: Data Science on AWS
- Họp team project: xác định solution architecture, budget estimation

### Các đầu việc của tuần 6:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2 | - Họp team project: Xác định, nghiên cứu solution architecture cho web app | 13/10/2025 | 13/10/2025 | [Proposal Template](https://workshop-sample.fcjuni.com/2-proposal/) |
| 3 | - Thực hành Lab 13: Tối ưu chi phí EC2 với Lambda<br>&emsp; + Tạo VPC, Security Group, EC2 instance<br>&emsp; + Cấu hình Incoming Web-hooks Slack<br>&emsp; + Tạo Tag cho Instance<br>&emsp; + Tạo Role cho Lambda<br>&emsp; + Tạo Lambda Function: stop instance, start instance<br>&emsp; + Kiểm tra kết quả | 14/10/2025 | 14/10/2025 | [Lab 13](https://000022.awsstudygroup.com/vi/) |
| 4 | - Thực hành Lab 14: Serverless - Tương tác giữa Lambda với S3 và DynamoDB<br>&emsp; + Xử lý và Tối ưu Kích thước Ảnh trên AWS<br>&emsp; &emsp; * Tạo Lambda Function Xử Lý Ảnh<br>&emsp; &emsp; * Tạo S3 Bucket, IAM Policy cho Lambda<br>&emsp; + Ghi dữ liệu vào Amazon DynamoDB<br>&emsp; &emsp; * Tạo và Quản lý Bảng trong DynamoDB<br>&emsp; &emsp; * Tạo Lambda Function để Ghi dữ liệu | 15/10/2025 | 15/10/2025 | [Lab 14](https://000078.awsstudygroup.com/vi/) |
| 5 | - Tham gia workshop: Data Science on AWS - Mở khóa sức mạnh dữ liệu cùng điện toán đám mây<br>- Họp team project: Hoàn thiện solution architecture, budget estimation cho web app | 16/10/2025 | 16/10/2025 | [AWS Calculator](https://calculator.aws/#/) |
| 6 | - Thực hành Lab 15: Làm quen với AWS CLI<br>&emsp; + Cài đặt AWS CLI<br>&emsp; + Kiểm tra tài nguyên qua CLI<br>&emsp; + AWS CLI với Amazon S3<br>&emsp; + AWS CLI với Amazon SNS<br>&emsp; + AWS CLI với IAM<br>&emsp; + AWS CLI với VPC và Internet Gateway<br>&emsp; + Tạo EC2 sử dụng AWS CLI<br>&emsp; + Khắc phục lỗi | 17/10/2025 | 17/10/2025 | [Lab 15](https://000011.awsstudygroup.com/vi/) |

### Thành tựu đạt được trong tuần 6:

#### **Lab 13: Tối ưu chi phí EC2 với Lambda**
- **Cost Optimization Strategy**:
  - Dev/Test instances không cần chạy 24/7
  - Schedule: Start 8AM, Stop 6PM weekdays → Tiết kiệm 65% cost
  - **ROI calculation**: t3.medium $0.0416/hour × 14 hours/day saved × 22 days = ~$12.8/month/instance
- **Lambda Implementation**:
  - **Start Function**: `ec2.start_instances(InstanceIds=[...])`, trigger by EventBridge 8AM
  - **Stop Function**: `ec2.stop_instances(InstanceIds=[...])`, trigger by EventBridge 6PM
  - **Tag-based filtering**: `Environment: dev` để chỉ affect dev instances
- **Slack Integration**:
  - Incoming Webhook URL từ Slack App
  - Lambda post message khi start/stop thành công
  - **Benefit**: Team awareness, troubleshooting dễ hơn
- **IAM Role cho Lambda**:
  - `ec2:StartInstances`, `ec2:StopInstances`, `ec2:DescribeInstances`
  - **Least privilege**: Chỉ allow actions cần thiết, resource-level permissions

#### **Lab 14: Serverless với Lambda, S3, DynamoDB**
- **Image Processing Pipeline**:
  - S3 trigger: `s3:ObjectCreated:*` trên bucket `uploads/`
  - Lambda resize image → Save to `thumbnails/` bucket
  - **Sharp library**: Lambda Layer vì native bindings
  - **Memory setting**: 1024MB cho image processing (CPU scales với memory)
- **DynamoDB Design**:
  - **Partition Key**: Unique identifier, high cardinality (user_id, place_id)
  - **Sort Key**: Range queries (timestamp, rating)
  - **Single-table design**: `PK: PLACE#123, SK: META` cho place info, `SK: REVIEW#456` cho reviews
  - **GSI**: `GSI1PK: CITY#HCM, GSI1SK: RATING#4.5` cho query places by city sorted by rating
- **Lambda Best Practices**:
  - **Cold start**: Initialize DB connections outside handler
  - **Timeout**: Set reasonable timeout (default 3s, max 15 mins)
  - **Error handling**: Try-catch, DLQ for failed events

#### **Lab 15: AWS CLI Mastery**
- **Setup**: `aws configure` với Access Key, Secret Key, Region, Output format
- **S3 Commands**:
  ```bash
  aws s3 mb s3://my-bucket                    # Create bucket
  aws s3 cp file.txt s3://my-bucket/          # Upload
  aws s3 sync ./local s3://my-bucket/folder   # Sync directory
  aws s3 ls s3://my-bucket --recursive        # List all objects
  ```
- **EC2 Commands**:
  ```bash
  aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId'
  aws ec2 run-instances --image-id ami-xxx --instance-type t3.micro
  aws ec2 terminate-instances --instance-ids i-xxx
  ```
- **Troubleshooting**:
  - `--debug` flag để xem full request/response
  - `aws sts get-caller-identity` để verify credentials
  - **Common errors**: ExpiredToken, AccessDenied (check IAM permissions)

#### **Workshop: Data Science on AWS**
- Hands-on với SageMaker: Create notebook, train model, deploy endpoint
- Data processing với Glue: ETL jobs, Data Catalog
- Analytics với Athena: Query S3 data với SQL
- **Key insight**: AWS có full ML pipeline, không cần manage infrastructure

#### **Họp Team Project - Architecture & Budget**
- **Solution Architecture**:
  - Frontend: React (S3 + CloudFront) 
  - Backend: Lambda + API Gateway (serverless, pay per request)
  - Database: RDS PostgreSQL với PostGIS (geospatial queries)
  - Search: pgvector extension cho vector search
  - Auth: Cognito User Pools
- **Budget Estimation (AWS Calculator)**:
  - RDS db.t3.micro: ~$15/month
  - Lambda: ~$0 (free tier 1M requests)
  - S3 + CloudFront: ~$5/month
  - **Total dev environment**: ~$20-30/month
- **Architecture Decision**: Serverless-first để minimize ops và cost
