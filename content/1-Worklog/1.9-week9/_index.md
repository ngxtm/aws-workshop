---
title: "Week 9 Worklog"
date: 2025-11-03
weight: 9
chapter: false
pre: " <b> 1.9 </b> "
---

### Week 9 Objective: CI/CD & DevOps Automation

Automate the software development process. MapVibe uses GitLab CI to build, test, and deploy.

### Tasks to complete this week:

| Day | Task                                                                                                                                                                                        | Start Date | Completion Date | References                                       |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------ |
| Mon | - **CI/CD Concepts**:<br>&emsp; + Continuous Integration<br>&emsp; + Continuous Delivery/Deployment<br>&emsp; + Pipeline, Stages, Jobs, Runners                                             | 03/11/2025 | 03/11/2025      | [GitLab CI Docs](https://docs.gitlab.com/ee/ci/) |
| Tue | - **GitLab CI Basics**:<br>&emsp; + `.gitlab-ci.yml` file<br>&emsp; + Create simple pipeline (Build -> Test)<br>&emsp; + Variables & Secrets (Protect AWS keys)                             | 04/11/2025 | 04/11/2025      |                                                  |
| Wed | - **Build & Test Automation**:<br>&emsp; + Configure cache (node_modules) to speed up build<br>&emsp; + Artifacts (Save build results)<br>&emsp; + Run Unit Tests automatically in pipeline | 05/11/2025 | 05/11/2025      |                                                  |
| Thu | - **Deployment Automation**:<br>&emsp; + Deploy to S3 (Frontend)<br>&emsp; + Deploy CDK/SAM (Backend) from pipeline<br>&emsp; + Environments (Dev, Staging, Prod)                           | 06/11/2025 | 06/11/2025      |                                                  |
| Fri | - **Advanced CI/CD**:<br>&emsp; + Docker-in-Docker (dind) to build images<br>&emsp; + Scheduled Pipelines<br>&emsp; + Manual Approval (Approve before Prod deploy)                          | 07/11/2025 | 07/11/2025      |                                                  |
| Sat | - **Practice Lab 9**:<br>&emsp; + Setup GitLab Repo for sample project<br>&emsp; + Write pipeline: Build React App -> Deploy to S3 hosting                                                  | 08/11/2025 | 08/11/2025      |                                                  |
| Sun | - Weekly Summary<br>- Prepare for Monitoring                                                                                                                                                | 09/11/2025 | 09/11/2025      |                                                  |

### Week 9 Achievements:

- Able to write complete CI/CD pipeline.
- Understood how to secure credentials during build.
- Ready to setup pipeline for MapVibe Monorepo.
