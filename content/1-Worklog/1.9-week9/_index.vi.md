---
title: "Nhật ký tuần 9"
date: 2025-11-03
weight: 9
chapter: false
pre: " <b> 1.9 </b> "
---

### Mục tiêu tuần 9:
- Họp team project: hoàn thiện database, thu thập data, feedback UI
- Thực hành Lab 19, 20: CDK cơ bản
- Thực hành Lab 21: Infrastructure as Code - Three-Tier Architecture

### Các đầu việc của tuần 9:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2 | - Họp team project: Hoàn thiện thiết kế database, tiếp tục thu thập data | 03/11/2025 | 03/11/2025 | |
| 3 | - Thực hành Lab 19: CDK cơ bản<br>&emsp; + Tạo IAM Role<br>&emsp; + CDK cơ bản: Tạo workspace, cấu hình Cloud9<br>&emsp; + Tạo CDK Template<br>&emsp; + Cập nhật CDK Template | 04/11/2025 | 04/11/2025 | [Lab 19 CDK](https://000038.awsstudygroup.com/vi/) |
| 4 | - Thực hành Lab 20: CDK Cơ bản - 2<br>&emsp; + Tạo IAM Role, chạy EC2, cấu hình VSCode<br>&emsp; + ECS, ALB và API Gateway<br>&emsp; + Lambda và S3<br>&emsp; + Nested stack: Tạo các stack lồng với CDK | 05/11/2025 | 05/11/2025 | [Lab 20](https://000076.awsstudygroup.com/vi/) |
| 5 | - Họp team project: Hoàn tất thu thập data, feedback cải thiện thiết kế UI figma | 06/11/2025 | 06/11/2025 | |
| 6 | - Thực hành Lab 21: Giới Thiệu về Infrastructure as Code<br>&emsp; + Tạo Lambda function<br>&emsp; + Tạo VPC và EC2<br>&emsp; + Triển khai Three-Tier Architecture<br>&emsp; &emsp; * Web Tier, Application Tier, Database Tier | 07/11/2025 | 07/11/2025 | [Lab 21](https://000102.awsstudygroup.com/vi/) |

### Thành tựu đạt được trong tuần 9:

#### **Team Project - Database Design & Data Collection**
- **Database Design hoàn thành (Quân + Đạt phụ trách)**:
  - **DB to Vector**: Convert text data sang vector embeddings cho semantic search
  - Cost tracking: **$16.58** cho embedding API calls + **$0.000028** cho additional operations
  - Dùng OpenAI text-embedding-3-small (cheaper, good enough cho use case)
- **PostgreSQL Schema Design**:
  - `places` table: id, name, address, lat, lng, description, embedding (vector)
  - `reviews` table: id, place_id, user_id, content, rating, created_at
  - **PostGIS extension**: `geometry(Point, 4326)` cho location queries
  - Index strategy: B-tree cho id/foreign keys, GiST cho geometry, IVFFlat cho vectors
- **Data Collection** (cả team tham gia):
  - Crawl thông tin quán từ các sources
  - Điền data vào Google Sheet chung, format chuẩn
  - **Lesson learned**: Data quality quan trọng hơn quantity - phải validate trước khi import
- **Daily Meeting (2/11)**: 8h sáng, Thông đi massage nên skip =)))
- **UI Feedback**: Review Figma designs, góp ý về UX flow

#### **Lab 19: AWS CDK Cơ bản**
- **CDK vs CloudFormation**:
  - CDK: Viết code (TypeScript/Python), generate CloudFormation
  - Benefit: IDE support, reusable constructs, loops/conditions dễ hơn YAML
  - **Trade-off**: Thêm abstraction layer, debug khó hơn khi có issues
- **CDK Concepts**:
  - **App**: Entry point, chứa nhiều Stacks
  - **Stack**: Unit of deployment, map 1:1 với CFN stack
  - **Construct**: Building blocks, có 3 levels (L1 = CFN, L2 = curated, L3 = patterns)
- **Workflow**:
  ```bash
  cdk init app --language typescript  # Init project
  cdk synth                           # Generate CFN template
  cdk diff                            # Preview changes
  cdk deploy                          # Deploy stack
  cdk destroy                         # Cleanup
  ```
- **Best Practices**:
  - Dùng L2 constructs khi có thể (sensible defaults)
  - Environment separation với cdk.json contexts
  - Stack naming convention: `{project}-{env}-{service}`

#### **Lab 20: CDK Nâng cao**
- **ECS với CDK**:
  - `ApplicationLoadBalancedFargateService` L3 construct - tạo ALB + ECS + Target Group trong 10 lines
  - So với raw CloudFormation: ~200 lines → 10 lines, huge productivity gain
- **Serverless với CDK**:
  - `NodejsFunction` construct: Auto bundle với esbuild, TypeScript support
  - Event sources: `S3EventSource`, `SqsEventSource`, `ApiGatewayEventSource`
  - **Tip**: `bundling.minify = true` cho production, reduce cold start
- **Nested Stacks Pattern**:
  - Main stack import child stacks
  - Cross-stack references với `CfnOutput` và `Fn.importValue`
  - **When to use**: >500 resources, separate team ownership, independent deployment

#### **Lab 21: Three-Tier Architecture IaC**
- **Complete Stack**:
  - Web tier: ALB + ASG với Launch Template
  - App tier: Lambda functions với API Gateway
  - Data tier: RDS PostgreSQL Multi-AZ
- **Security**:
  - Security Groups chained: ALB → App → DB
  - Each tier chỉ accept traffic từ tier trước
  - DB trong private subnet, không public access
- **Practical Implementation**:
  - Test end-to-end: User → ALB → Lambda → RDS → Response
  - Verify failover: Stop primary DB, confirm auto-failover to standby
  - **Cost estimate**: ~$50/month cho small setup (t3.micro + db.t3.micro)
