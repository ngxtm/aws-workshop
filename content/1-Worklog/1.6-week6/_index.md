---
title: "Week 6 Worklog"
date: 2025-10-13
weight: 6
chapter: false
pre: " <b> 1.6 </b> "
---

### Week 6 Objectives:
- Practice Lab 13: Optimize EC2 costs with Lambda
- Practice Lab 14: Serverless - Lambda with S3 and DynamoDB
- Practice Lab 15: Getting started with AWS CLI
- Attend workshop: Data Science on AWS
- Team project meeting: define solution architecture, budget estimation

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2 | - Team project meeting: Define and research solution architecture for web app | 13/10/2025 | 13/10/2025 | [Proposal Template](https://workshop-sample.fcjuni.com/2-proposal/) |
| 3 | - Practice Lab 13: Optimize EC2 costs with Lambda<br>&emsp; + Create VPC, Security Group, EC2 instance<br>&emsp; + Configure Incoming Web-hooks Slack<br>&emsp; + Create Tag for Instance<br>&emsp; + Create Role for Lambda<br>&emsp; + Create Lambda Function: stop instance, start instance<br>&emsp; + Test results | 14/10/2025 | 14/10/2025 | [Lab 13](https://000022.awsstudygroup.com/vi/) |
| 4 | - Practice Lab 14: Serverless - Lambda interaction with S3 and DynamoDB<br>&emsp; + Process and Optimize Image Size on AWS<br>&emsp; &emsp; * Create Lambda Function for Image Processing<br>&emsp; &emsp; * Create S3 Bucket, IAM Policy for Lambda<br>&emsp; + Write data to Amazon DynamoDB<br>&emsp; &emsp; * Create and Manage Tables in DynamoDB<br>&emsp; &emsp; * Create Lambda Function to Write data | 15/10/2025 | 15/10/2025 | [Lab 14](https://000078.awsstudygroup.com/vi/) |
| 5 | - Attend workshop: Data Science on AWS - Unlock data power with cloud computing<br>- Team project meeting: Complete solution architecture, budget estimation for web app | 16/10/2025 | 16/10/2025 | [AWS Calculator](https://calculator.aws/#/) |
| 6 | - Practice Lab 15: Getting started with AWS CLI<br>&emsp; + Install AWS CLI<br>&emsp; + Check resources via CLI<br>&emsp; + AWS CLI with Amazon S3<br>&emsp; + AWS CLI with Amazon SNS<br>&emsp; + AWS CLI with IAM<br>&emsp; + AWS CLI with VPC and Internet Gateway<br>&emsp; + Create EC2 using AWS CLI<br>&emsp; + Troubleshooting | 17/10/2025 | 17/10/2025 | [Lab 15](https://000011.awsstudygroup.com/vi/) |

### Week 6 Achievements:

#### **Lab 13: Optimize EC2 Costs with Lambda**
- Understand how to use Lambda to automate EC2 management
- Create Lambda function to start/stop EC2 instances on schedule
- Integrate with Slack to receive notifications when EC2 is started/stopped
- Use Tags to categorize and manage instances
- Save costs by turning off instances outside working hours

#### **Lab 14: Serverless with Lambda, S3, DynamoDB**
- **Image Processing with Lambda**:
  - Create Lambda function triggered from S3 event
  - Auto-resize images when uploaded to S3
  - Configure IAM Policy for Lambda to access S3
- **DynamoDB Integration**:
  - Create and manage DynamoDB tables
  - Lambda function writes data to DynamoDB
  - Understand Partition Key and Sort Key

#### **Lab 15: AWS CLI Mastery**
- Install and configure AWS CLI with credentials
- Work with S3: create bucket, upload/download objects
- Manage SNS: create topic, subscribe, publish messages
- IAM operations: create users, groups, policies
- VPC and Networking: create VPC, subnets, Internet Gateway
- Create and manage EC2 instances entirely via CLI
- Troubleshooting common CLI errors

#### **Workshop: Data Science on AWS**
- Attended workshop on Data Science on AWS
- Learned about AWS ML/AI services
- Understood how to process and analyze data on cloud

#### **Team Project Meeting**
- Defined solution architecture for MapVibe:
  - Frontend: React + CloudFront
  - Backend: Lambda + API Gateway
  - Database: RDS PostgreSQL
  - Authentication: Cognito
- Calculated budget estimation with AWS Calculator
- Completed proposal document
