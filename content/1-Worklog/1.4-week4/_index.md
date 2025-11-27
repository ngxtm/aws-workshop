---
title: "Week 4 Worklog"
date: 2025-09-29
weight: 4
chapter: false
pre: " <b> 1.4 </b> "
---

### Week 4 Objective: Master Networking (VPC)

This is the most critical foundational knowledge for building secure and scalable systems on AWS. Before diving into Serverless or Advanced Databases for the MapVibe project, understanding the network is essential.

### Tasks to complete this week:

| Day | Task                                                                                                                                                                                                                                         | Start Date | Completion Date | References                                                                                                  |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------------------------------------------------------------------------- |
| Mon | - **Learn VPC (Virtual Private Cloud)**:<br>&emsp; + What is CIDR block?<br>&emsp; + Subnets (Public vs Private)<br>&emsp; + Route Tables & Internet Gateway (IGW)<br>&emsp; + Practice: Create VPC with 1 Public Subnet and Internet access | 29/09/2025 | 29/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[AWS VPC Docs](https://docs.aws.amazon.com/vpc/) |
| Tue | - **Internet Connection for Private Subnet**:<br>&emsp; + NAT Gateway vs NAT Instance<br>&emsp; + Egress-only Internet Gateway (IPv6)<br>&emsp; + Practice: Configure Private Subnet internet access via NAT Gateway                         | 30/09/2025 | 30/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| Wed | - **Network Security**:<br>&emsp; + Security Groups (Stateful)<br>&emsp; + Network ACLs (Stateless)<br>&emsp; + Compare and apply multi-layer security models<br>&emsp; + Flow Logs (Monitor network traffic)                                | 01/10/2025 | 01/10/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| Thu | - **Advanced Connectivity**:<br>&emsp; + VPC Peering (Connect 2 VPCs)<br>&emsp; + VPC Endpoints (Gateway vs Interface)<br>&emsp; + VPN (Site-to-Site, Client VPN)<br>&emsp; + Transit Gateway (Overview)                                     | 02/10/2025 | 02/10/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| Fri | - **Load Balancing & Auto Scaling** (Combined with Network):<br>&emsp; + Application Load Balancer (ALB) vs Network Load Balancer (NLB)<br>&emsp; + Auto Scaling Groups (ASG)<br>&emsp; + Practice: Deploy web app with ALB and ASG in VPC   | 03/10/2025 | 03/10/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| Sat | - **Review & Comprehensive Lab**:<br>&emsp; + Build 3-tier architecture (Web, App, DB) in VPC<br>&emsp; + Verify connectivity and security                                                                                                   | 04/10/2025 | 04/10/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| Sun | - Write weekly report<br>- Prepare knowledge for Serverless (next week)                                                                                                                                                                      | 05/10/2025 | 05/10/2025      |                                                                                                             |

### Week 4 Achievements:

#### **Networking & Security**

- **VPC Mastery**:
  - Designed and deployed Custom VPC models.
  - Understood packet flow in AWS.
- **Security**:
  - Configured Security Groups for Least Privilege access.
  - Distinguished when to use NACLs.
- **High Availability**:
  - Designed Fault Tolerant systems using Multi-AZ and Load Balancers.

#### **Preparation for MapVibe**:

- VPC knowledge will be used to:
  - Configure **RDS Proxy** (requires VPC).
  - Configure **Lambda** to access RDS (requires ENI in VPC).
  - Secure **ElastiCache**.
