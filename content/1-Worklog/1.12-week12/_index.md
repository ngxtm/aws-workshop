---
title: "Week 12 Worklog"
date: 2025-11-24
weight: 12
chapter: false
pre: " <b> 1.12 </b> "
---

### Week 12 Objectives:
- **Migrate RAG System to AWS** with Infrastructure as Code
- **Complete Infrastructure**: VPC, RDS, Lambda, API Gateway
- **Integrate Amazon Bedrock** for AI features

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2 | - **Team project meeting**: Finalize RAG System Migration to AWS solution<br>&emsp; + Serverless Architecture with Lambda and API Gateway<br>&emsp; + Database: RDS PostgreSQL with pgvector<br>&emsp; + AI Model: Amazon Bedrock (Claude 3) | 24/11/2025 | 24/11/2025 | [AWS Architecture](https://aws.amazon.com/architecture/) |
| 3 | - **Setup Infrastructure (Terraform)**:<br>&emsp; + VPC, Private/Public Subnets, Security Groups<br>&emsp; + RDS PostgreSQL instance (db.t3.micro)<br>&emsp; + Configure pgvector extension | 25/11/2025 | 25/11/2025 | [Terraform AWS Module](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest) |
| 4 | - **Develop Backend RAG**:<br>&emsp; + Write Lambda function for Embeddings (Titan Embeddings v2)<br>&emsp; + Write Lambda function for Retrieval & Generation (Claude 3)<br>&emsp; + Optimize prompt engineering for Bedrock | 26/11/2025 | 26/11/2025 | [Amazon Bedrock Docs](https://docs.aws.amazon.com/bedrock/) |
| 5 | - **Integration & Testing**:<br>&emsp; + Connect API Gateway with Lambda<br>&emsp; + Test flow from Frontend -> API -> Lambda -> Bedrock -> RDS<br>&emsp; + Debug connection timeout and permission errors (IAM) | 27/11/2025 | 27/11/2025 |  |
| 6 | - **Monitoring & Optimization**:<br>&emsp; + Setup CloudWatch Logs for Lambda<br>&emsp; + Monitor latency and cost of Bedrock<br>&emsp; + Review code and merge into dev branch | 28/11/2025 | 28/11/2025 |  |

### Week 12 Achievements:

#### **Team Project - RAG System Migration to AWS**
- **Migration Strategy**:
  - Successfully migrated from Local PostgreSQL to **RDS PostgreSQL** with `pgvector`.
  - Replaced Local LLM with **Amazon Bedrock** (Claude 3 Haiku/Sonnet) to reduce server load and improve response speed.
  - Built Serverless architecture with **API Gateway + Lambda**, optimizing costs (pay-per-request).
  
- **Infrastructure Implementation**:
  - Used **Terraform** for infrastructure management (IaC), facilitating easy deployment and rollback.
  - **RDS Setup**: Configured in Private Subnet for security, used RDS Proxy to manage connection pooling for Lambda.
  - **IAM Security**: Applied Least Privilege for Lambda roles (only invoke necessary Bedrock models).

- **RAG Technique (Retrieval Augmented Generation)**:
  - Successfully implemented flow: User Question -> Embedding -> Vector Search (RDS) -> Context -> Bedrock -> Answer.
  - Handled edge cases when no suitable context is found.