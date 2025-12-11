---
title: "Nhật ký tuần 12"
date: 2025-11-24
weight: 12
chapter: false
pre: " <b> 1.12 </b> "
---

### Mục tiêu tuần 12:
- **Migrate RAG System lên AWS** với Infrastructure as Code
- **Hoàn thiện Infrastructure**: VPC, RDS, Lambda, API Gateway
- **Tích hợp Amazon Bedrock** cho tính năng AI

### Các đầu việc của tuần 12:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2 | - **Họp team project**: Chốt giải pháp Migrate RAG System lên AWS<br>&emsp; + Kiến trúc Serverless với Lambda và API Gateway<br>&emsp; + Database: RDS PostgreSQL với pgvector<br>&emsp; + AI Model: Amazon Bedrock (Claude 3) | 24/11/2025 | 24/11/2025 | [AWS Architecture](https://aws.amazon.com/architecture/) |
| 3 | - **Setup Infrastructure (Terraform)**:<br>&emsp; + VPC, Private/Public Subnets, Security Groups<br>&emsp; + RDS PostgreSQL instance (db.t3.micro)<br>&emsp; + Cấu hình pgvector extension | 25/11/2025 | 25/11/2025 | [Terraform AWS Module](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest) |
| 4 | - **Phát triển Backend RAG**:<br>&emsp; + Viết Lambda function xử lý Embeddings (Titan Embeddings v2)<br>&emsp; + Viết Lambda function xử lý Retrieval & Generation (Claude 3)<br>&emsp; + Tối ưu prompt engineering cho Bedrock | 26/11/2025 | 26/11/2025 | [Amazon Bedrock Docs](https://docs.aws.amazon.com/bedrock/) |
| 5 | - **Integration & Testing**:<br>&emsp; + Kết nối API Gateway với Lambda<br>&emsp; + Test flow từ Frontend -> API -> Lambda -> Bedrock -> RDS<br>&emsp; + Debug lỗi connection timeout và permission (IAM) | 27/11/2025 | 27/11/2025 |  |
| 6 | - **Monitoring & Optimization**:<br>&emsp; + Setup CloudWatch Logs cho Lambda<br>&emsp; + Theo dõi latency và cost của Bedrock<br>&emsp; + Review code và merge vào branch dev | 28/11/2025 | 28/11/2025 |  |

### Thành tựu đạt được trong tuần 12:

#### **Team Project - RAG System Migration to AWS**
- **Migration Strategy**:
  - Chuyển đổi thành công từ Local PostgreSQL sang **RDS PostgreSQL** với `pgvector`.
  - Thay thế Local LLM bằng **Amazon Bedrock** (Claude 3 Haiku/Sonnet) giúp giảm tải cho server và tăng tốc độ phản hồi.
  - Xây dựng kiến trúc Serverless với **API Gateway + Lambda**, tối ưu chi phí (pay-per-request).
  
- **Infrastructure Implementation**:
  - Sử dụng **Terraform** để quản lý hạ tầng (IaC), giúp việc deploy và rollback dễ dàng.
  - **RDS Setup**: Cấu hình trong Private Subnet để bảo mật, sử dụng RDS Proxy để quản lý connection pooling cho Lambda.
  - **IAM Security**: Cấp quyền tối thiểu (Least Privilege) cho Lambda roles (chỉ invoke đúng model Bedrock cần thiết).

- **Kỹ thuật RAG (Retrieval Augmented Generation)**:
  - Implement thành công flow: User Question -> Embedding -> Vector Search (RDS) -> Context -> Bedrock -> Answer.
  - Xử lý các edge cases khi không tìm thấy context phù hợp.