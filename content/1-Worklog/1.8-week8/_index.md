---
title: "Week 8 Worklog"
date: 2025-10-27
weight: 8
chapter: false
pre: " <b> 1.8 </b> "
---

### Week 8 Objective: Containers & Local Development

Although MapVibe uses Serverless, Docker is indispensable for running local development environments (Local DB, Redis). Additionally, understanding ECS/EKS provides a comprehensive view of Compute.

### Tasks to complete this week:

| Day | Task                                                                                                                                                                                                               | Start Date | Completion Date | References                                                                               |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ---------------------------------------------------------------------------------------- |
| Mon | - **Docker Fundamentals**:<br>&emsp; + Container vs VM<br>&emsp; + Dockerfile, Image, Container<br>&emsp; + Docker Compose (Manage multi-container)<br>&emsp; + Practice: Write Dockerfile for Node.js app         | 27/10/2025 | 27/10/2025      | [Docker Docs](https://docs.docker.com/)                                                  |
| Tue | - **Local Dev Environment**:<br>&emsp; + Use Docker Compose to run PostgreSQL + PostGIS<br>&emsp; + Use Docker Compose to run Redis<br>&emsp; + Connect local Node.js app to Docker services                       | 28/10/2025 | 28/10/2025      |                                                                                          |
| Wed | - **Amazon ECR (Elastic Container Registry)**:<br>&emsp; + Push/Pull Docker images<br>&emsp; + Manage Image tags and lifecycle<br>&emsp; + Scan vulnerabilities                                                    | 29/10/2025 | 29/10/2025      | [AWS ECR Docs](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)  |
| Thu | - **Amazon ECS (Elastic Container Service)**:<br>&emsp; + EC2 Launch Type vs Fargate (Serverless Containers)<br>&emsp; + Task Definitions & Services<br>&emsp; + Cluster management                                | 30/10/2025 | 30/10/2025      | [AWS ECS Docs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) |
| Fri | - **Amazon EKS (Kubernetes) - Overview**:<br>&emsp; + What is Kubernetes? (Pods, Deployments, Services)<br>&emsp; + When to use EKS vs ECS vs Lambda?<br>&emsp; + (No need to dive deep, just understand concepts) | 31/10/2025 | 31/10/2025      |                                                                                          |
| Sat | - **Practice Lab 8**:<br>&emsp; + Package Web app into Docker Image<br>&emsp; + Push to ECR<br>&emsp; + Test deploy to ECS Fargate                                                                                 | 01/11/2025 | 01/11/2025      |                                                                                          |
| Sun | - Weekly Summary<br>- Prepare for CI/CD                                                                                                                                                                            | 02/11/2025 | 02/11/2025      |                                                                                          |

### Week 8 Achievements:

- Proficient in Docker for quick dev environment setup.
- Understood difference between running code on Lambda and Container.
- Known how to package standardized applications.
