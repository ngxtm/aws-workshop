---
title: "Week 7 Worklog"
date: 2025-10-20
weight: 7
chapter: false
pre: " <b> 1.7 </b> "
---

### Week 7 Objective: Infrastructure as Code (IaC)

Instead of clicking on the Console (ClickOps), we will use code to define infrastructure. This enables reusability, version control, and automated deployment. With the project's TypeScript foundation, **AWS CDK** is the optimal choice.

### Tasks to complete this week:

| Day | Task                                                                                                                                                                                                                                   | Start Date | Completion Date | References                                          |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | --------------------------------------------------- |
| Mon | - **IaC Concepts**:<br>&emsp; + Declarative vs Imperative<br>&emsp; + Immutable Infrastructure<br>&emsp; + Intro to AWS CloudFormation (foundation of CDK/SAM)                                                                         | 20/10/2025 | 20/10/2025      | [AWS IaC Docs](https://aws.amazon.com/what-is/iac/) |
| Tue | - **AWS CDK (Cloud Development Kit)**:<br>&emsp; + Install CDK CLI<br>&emsp; + CDK Project Structure (App, Stack, Construct)<br>&emsp; + Bootstrap AWS Environment<br>&emsp; + Practice: "Hello World" CDK (Create S3 Bucket via code) | 21/10/2025 | 21/10/2025      | [AWS CDK Workshop](https://cdkworkshop.com/)        |
| Wed | - **CDK Constructs (L1, L2, L3)**:<br>&emsp; + Distinguish Construct levels<br>&emsp; + Use L2 Constructs (S3, DynamoDB, Lambda)<br>&emsp; + Write TypeScript code to create infrastructure                                            | 22/10/2025 | 22/10/2025      |                                                     |
| Thu | - **Deploy & Manage Stacks**:<br>&emsp; + `cdk synth` (Generate CloudFormation template)<br>&emsp; + `cdk deploy` (Deploy)<br>&emsp; + `cdk diff` (View changes)<br>&emsp; + `cdk destroy` (Delete infrastructure)                     | 23/10/2025 | 23/10/2025      |                                                     |
| Fri | - **Advanced CDK**:<br>&emsp; + Manage Environments (Dev/Prod)<br>&emsp; + Outputs & Exports (Share info between Stacks)<br>&emsp; + Import existing resources into CDK                                                                | 24/10/2025 | 24/10/2025      |                                                     |
| Sat | - **Practice Lab 7**:<br>&emsp; + Use CDK to redeploy Week 5 Serverless architecture (API GW + Lambda + DynamoDB)<br>&emsp; + Experience the convenience compared to manual work                                                       | 25/10/2025 | 25/10/2025      |                                                     |
| Sun | - Weekly Summary<br>- Prepare for Container & Docker                                                                                                                                                                                   | 26/10/2025 | 26/10/2025      |                                                     |

### Week 7 Achievements:

- Understood "Infrastructure as Code" mindset.
- Able to use TypeScript to create and manage AWS resources.
- Ready to automate MapVibe deployment.
