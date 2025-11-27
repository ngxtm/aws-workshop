---
title: "Week 6 Worklog"
date: 2025-10-13
weight: 6
chapter: false
pre: " <b> 1.6 </b> "
---

### Week 6 Objective: Advanced Database & Authentication

Focus on advanced data services and user security, the two most critical components of MapVibe (Geospatial search & User Accounts).

### Tasks to complete this week:

| Day | Task                                                                                                                                                                                                                                             | Start Date | Completion Date | References                                                       |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ---------------------------------------------------------------- |
| Mon | - **Amazon RDS Advanced**:<br>&emsp; + Multi-AZ Deployments (High Availability)<br>&emsp; + Read Replicas (Performance)<br>&emsp; + **RDS Proxy**: Why use with Lambda?<br>&emsp; + Practice: Create RDS Postgres and connect from local machine | 13/10/2025 | 13/10/2025      | [AWS RDS Docs](https://docs.aws.amazon.com/rds/)                 |
| Tue | - **PostgreSQL & PostGIS**:<br>&emsp; + Install PostGIS extension<br>&emsp; + Spatial Data Types (Geometry, Geography)<br>&emsp; + Spatial Queries: Nearest Neighbor<br>&emsp; + Practice: Query to find restaurants within 5km radius           | 14/10/2025 | 14/10/2025      | [PostGIS Docs](https://postgis.net/)                             |
| Wed | - **Amazon ElastiCache (Redis)**:<br>&emsp; + Caching strategies (Lazy Loading vs Write Through)<br>&emsp; + Redis Cluster mode<br>&emsp; + Application: Cache search results, Session store                                                     | 15/10/2025 | 15/10/2025      | [AWS ElastiCache Docs](https://docs.aws.amazon.com/elasticache/) |
| Thu | - **Amazon Cognito (Authentication)**:<br>&emsp; + User Pools vs Identity Pools<br>&emsp; + JWT Tokens (ID, Access, Refresh)<br>&emsp; + Hosted UI & Social Login (Google/Facebook)<br>&emsp; + Practice: Create login page with Cognito         | 16/10/2025 | 16/10/2025      | [AWS Cognito Docs](https://docs.aws.amazon.com/cognito/)         |
| Fri | - **Database & Auth Security**:<br>&emsp; + Manage Secrets Manager (Store DB credentials)<br>&emsp; + IAM Authentication for RDS<br>&emsp; + Integrate Cognito Authorizer into API Gateway                                                       | 17/10/2025 | 17/10/2025      |                                                                  |
| Sat | - **Review & Lab**:<br>&emsp; + Build architecture: API Gateway (Cognito Auth) -> Lambda -> RDS Proxy -> PostgreSQL<br>&emsp; + This is the actual backend architecture of MapVibe                                                               | 18/10/2025 | 18/10/2025      |                                                                  |
| Sun | - Weekly Summary<br>- Prepare for Infrastructure as Code                                                                                                                                                                                         | 19/10/2025 | 19/10/2025      |                                                                  |

### Week 6 Achievements:

- Understood how to handle map data with PostGIS.
- Mastered modern user authentication with Cognito.
- Understood the importance of Caching and Connection Pooling in large systems.
