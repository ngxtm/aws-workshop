---
title: "Week 5 Worklog"
date: 2025-10-06
weight: 5
chapter: false
pre: " <b> 1.5 </b> "
---

### Week 5 Objective: Serverless Computing (Lambda & API Gateway)

This is the core architecture of the MapVibe project. Moving from traditional servers (EC2) to Serverless helps reduce operational costs and increase scalability.

### Tasks to complete this week:

| Day | Task                                                                                                                                                                                                                                | Start Date | Completion Date | References                                                                |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------- |
| Mon | - **AWS Lambda Basics**:<br>&emsp; + What is Serverless?<br>&emsp; + Create first Lambda function (Node.js)<br>&emsp; + Lambda Triggers & Destinations<br>&emsp; + Practice: Lambda processing events from S3 (Simple image resize) | 06/10/2025 | 06/10/2025      | [AWS Lambda Docs](https://docs.aws.amazon.com/lambda/)                    |
| Tue | - **Amazon API Gateway**:<br>&emsp; + REST API vs HTTP API<br>&emsp; + Integrate API Gateway with Lambda (Proxy integration)<br>&emsp; + Stages & Deployments<br>&emsp; + Practice: Build simple CRUD API                           | 07/10/2025 | 07/10/2025      | [AWS APIGW Docs](https://docs.aws.amazon.com/apigateway/)                 |
| Wed | - **DynamoDB (NoSQL)**:<br>&emsp; + Partition Key & Sort Key<br>&emsp; + Read/Write Capacity Units (RCU/WCU)<br>&emsp; + Global Secondary Indexes (GSI)<br>&emsp; + Practice: Store data from Lambda to DynamoDB                    | 08/10/2025 | 08/10/2025      | [AWS DynamoDB Docs](https://docs.aws.amazon.com/dynamodb/)                |
| Thu | - **Serverless Framework / SAM**:<br>&emsp; + Introduction to AWS SAM (Serverless Application Model)<br>&emsp; + Write simple template.yaml<br>&emsp; + Deploy Serverless application using CLI                                     | 09/10/2025 | 09/10/2025      | [AWS SAM Docs](https://docs.aws.amazon.com/serverless-application-model/) |
| Fri | - **Lambda Advanced**:<br>&emsp; + Lambda Layers (Manage shared libraries)<br>&emsp; + Lambda in VPC (Connect to RDS later)<br>&emsp; + Cold Starts & Provisioned Concurrency                                                       | 10/10/2025 | 10/10/2025      |                                                                           |
| Sat | - **Review & Mini Project**:<br>&emsp; + Build complete "To-Do List" API:<br>&emsp; &emsp; API Gateway -> Lambda -> DynamoDB<br>&emsp; + Test with Postman                                                                          | 11/10/2025 | 11/10/2025      |                                                                           |
| Sun | - Weekly Summary<br>- Compare Serverless vs EC2                                                                                                                                                                                     | 12/10/2025 | 12/10/2025      |                                                                           |

### Week 5 Achievements:

- Understood Serverless workflow.
- Built RESTful API without managing servers.
- Prepared foundation for MapVibe Backend.
