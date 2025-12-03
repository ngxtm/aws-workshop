---
title: "Nhật ký tuần 12"
date: 2025-11-24
weight: 12
chapter: false
pre: " <b> 1.12 </b> "
---

### Mục tiêu tuần 12:
- Migrate RAG System lên AWS với Infrastructure as Code
- Thực hành Lab 30-33: IAM Governance, VPC Flow Log, Grafana, CloudWatch
- Hoàn thiện và deploy RAG system cho MapVibe

### Các đầu việc của tuần 12:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2 | - Họp team project: Migrate RAG System lên AWS<br>&emsp; + Nghiên cứu best practices về dịch vụ AWS phục vụ RAG<br>&emsp; + Tiến hành migrate RAG system lên AWS<br>&emsp; + RDS PostgreSQL với pgvector<br>&emsp; + Amazon Bedrock cho LLM | 24/11/2025 | 24/11/2025 | [AWS RDS PostgreSQL](https://aws.amazon.com/vi/rds/postgresql/)<br>[pgvector](https://github.com/pgvector/pgvector) |
| 3 | - Thực hành Lab 30: Quản trị Chi phí và Sử dụng tài nguyên với IAM<br>&emsp; + Tạo IAM Group và User<br>&emsp; + Giới hạn theo Region, EC2 family, instance size<br>&emsp; + Giới hạn theo EBS volume | 25/11/2025 | 25/11/2025 | [Lab 30](https://000064.awsstudygroup.com/vi/) |
| 4 | - Thực hành Lab 31: Giám sát hạ tầng mạng với VPC Flow Log<br>&emsp; + Kích hoạt VPC Flow Logs<br>&emsp; + Giám sát hạ tầng mạng<br>- Thực hành Lab 32: Bắt đầu với Grafana trên AWS<br>&emsp; + Tạo VPC, Security Group, EC2, IAM<br>&emsp; + Cài đặt và giám sát với Grafana | 26/11/2025 | 26/11/2025 | [Lab 31](https://000074.awsstudygroup.com/vi/)<br>[Lab 32](https://000029.awsstudygroup.com/vi/) |
| 5 | - Họp team project: Hoàn thiện migrate RAG System<br>&emsp; + Migrate RAG system lên AWS với Infrastructure as Code<br>&emsp; + Sử dụng Terraform cho deployment | 27/11/2025 | 27/11/2025 | [Terraform AWS](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) |
| 6 | - Thực hành Lab 33: AWS CloudWatch Workshop<br>&emsp; + CloudWatch Metric: Viewing, Search, Math expressions<br>&emsp; + CloudWatch Logs và Logs Insights<br>&emsp; + CloudWatch Metric Filter<br>&emsp; + CloudWatch Alarms và Dashboards | 28/11/2025 | 28/11/2025 | [Lab 33](https://000036.awsstudygroup.com/vi/) |

### Thành tựu đạt được trong tuần 12:

#### **Team Project - RAG System Migration to AWS**
- **Migration Strategy**:
  - Local PostgreSQL → RDS PostgreSQL với pgvector
  - Local LLM calls → Amazon Bedrock
  - Local API → API Gateway + Lambda
  - **Why Terraform?**: Team quen Terraform hơn CDK, dễ maintain long-term
- **RDS PostgreSQL Setup**:
  - Instance: db.t3.micro (free tier), Multi-AZ disabled (cost saving cho dev)
  - **pgvector extension**: `CREATE EXTENSION vector;` - cần enable manually
  - Connection: Private subnet only, access qua VPC
  - **RDS Proxy**: Connection pooling, giảm connection overhead cho Lambda
  - **Tip**: Parameter Group custom: `shared_preload_libraries = 'pg_stat_statements,pgvector'`
- **Amazon Bedrock Integration**:
  - Enable model access trong console (Claude 3 Haiku, Sonnet)
  - IAM Role cho Lambda: `bedrock:InvokeModel` permission
  - **Code snippet**:
    ```python
    bedrock = boto3.client('bedrock-runtime')
    response = bedrock.invoke_model(
        modelId='anthropic.claude-3-haiku-20240307-v1:0',
        body=json.dumps({"prompt": prompt, "max_tokens": 500})
    )
    ```
  - **Rate limiting**: Bedrock có quota per minute, implement retry với exponential backoff
- **Terraform Infrastructure**:
  - Modules structure:
    ```
    terraform/
    ├── modules/
    │   ├── vpc/
    │   ├── rds/
    │   ├── lambda/
    │   └── api-gateway/
    ├── environments/
    │   ├── dev/
    │   └── prod/
    └── main.tf
    ```
  - State management: S3 bucket + DynamoDB lock table
  - **Workflow**: `terraform plan` → Review → `terraform apply`
- **CI/CD cho Infrastructure**:
  - GitLab CI: Terraform validate → Plan → Apply (manual approval cho prod)
  - **Cost impact**: Mỗi PR show estimated cost change

#### **Lab 30: IAM Cost Governance**
- **Cost Control Policies**:
  - **Region restriction**: Chỉ allow `ap-southeast-1` → Prevent accident deploy ở expensive regions
  - **Instance type restriction**: Dev account chỉ được t3.micro, t3.small
  - **Require tags**: Deny actions nếu không có `Environment`, `Project` tags
- **Practical IAM Policy**:
  ```json
  {
    "Condition": {
      "StringNotEquals": {
        "ec2:InstanceType": ["t3.micro", "t3.small"]
      }
    },
    "Effect": "Deny",
    "Action": "ec2:RunInstances",
    "Resource": "arn:aws:ec2:*:*:instance/*"
  }
  ```
- **Cost Allocation Tags**: Track cost per project, per environment, per team

#### **Lab 31 & 32: Network Monitoring**
- **VPC Flow Logs**:
  - Destination: CloudWatch Logs hoặc S3 (cheaper cho long-term)
  - Filter: ALL (accept + reject) cho security analysis
  - **Query pattern**: Find rejected traffic → Identify missing Security Group rules
  - **Use case**: Debug "Lambda không connect được RDS" → Check flow logs thấy reject → Fix SG
- **Grafana on AWS**:
  - EC2 t3.small, install Grafana OSS
  - Data source: CloudWatch metrics
  - **Dashboard cho MapVibe**:
    - API latency (p50, p95, p99)
    - Error rate
    - Lambda concurrent executions
    - RDS connections, CPU, storage
  - **Alerting**: Slack notification khi error rate > 5%

#### **Lab 33: CloudWatch Workshop**
- **Metrics Math**:
  - `m1/m2 * 100`: Error rate percentage
  - `RATE(m1)`: Requests per second
  - `ANOMALY_DETECTION_BAND(m1, 2)`: Auto anomaly detection
- **Logs Insights Advanced**:
  ```
  stats count(*) as requests, 
        avg(duration) as avg_latency,
        pct(duration, 95) as p95_latency
  by bin(5m)
  ```
- **Composite Alarms**:
  - Combine multiple conditions: High latency AND high error rate → Critical alert
  - Reduce alert noise: Single metric spike không trigger, phải multiple signals
- **Dashboard Best Practices**:
  - Group by service: API metrics, Database metrics, Lambda metrics
  - Time range: Default 3h, drill down khi investigate
  - **Annotation**: Mark deployments để correlate với metric changes
