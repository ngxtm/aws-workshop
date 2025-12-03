---
title: "Week 7 Worklog"
date: 2025-10-20
weight: 7
chapter: false
pre: " <b> 1.7 </b> "
---

### Week 7 Objectives:
- Receive feedback on architecture solution from FCJ mentors
- Practice Lab 16: Amazon ElastiCache - Redis
- Practice Lab 17: AWS Networking and Content Delivery
- Complete and submit proposal

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2 | - Receive feedback on project architecture solution from FCJ mentors<br>- Listen and adjust architecture based on feedback | 20/10/2025 | 20/10/2025 | |
| 3-4 | - Practice Lab 16: Amazon ElastiCache - Redis<br>&emsp; + Create AWS Access Key, install and configure AWS CLI<br>&emsp; + Create ElastiCache cluster<br>&emsp; &emsp; * Create subnet group (Console & CLI)<br>&emsp; &emsp; * Create cluster (mode disabled & enabled)<br>&emsp; &emsp; * Grant access to cluster<br>&emsp; &emsp; * Connect to cluster nodes<br>&emsp; + Use AWS SDK to write and read data<br>&emsp; &emsp; * Set and Get strings, hash<br>&emsp; &emsp; * Publish/subscribe<br>&emsp; &emsp; * Write and read from stream | 21/10/2025 | 22/10/2025 | [Lab 16](https://000061.awsstudygroup.com/vi/) |
| 5 | - Complete entire proposal, deploy to GitHub and submit proposal | 23/10/2025 | 23/10/2025 | |
| 6 | - Practice Lab 17: AWS Networking and Content Delivery<br>&emsp; + Deploy VPC, Cisco CSR Agreement<br>&emsp; + VPC Components Deep Dive<br>&emsp; + Transit Gateway and Site-to-Site VPNs<br>&emsp; &emsp; * Access Cisco CSR Router with Cloud9<br>&emsp; &emsp; * Setup Site-to-site VPN<br>&emsp; &emsp; * Add ECMP Path<br>&emsp; &emsp; * Transit Gateway Routing<br>&emsp; + Route53 DNS Endpoints and Internal Hosted Zones | 24/10/2025 | 24/10/2025 | [Lab 17](https://000092.awsstudygroup.com/vi/) |

### Week 7 Achievements:

#### **Team Project - Research & Architecture Phase**
- **Daily Meeting (13/10)**: Kick-off meeting, phân chia công việc research
- **Research Tech Stack (hoàn thành 16/10)**:
  - Frontend: React + Vite + TailwindCSS - chọn vì ecosystem lớn, performance tốt
  - Backend: Node.js + Express hoặc Bun + Hono - đang cân nhắc Bun vì faster runtime
  - Database: PostgreSQL + PostGIS cho geospatial data (quán ăn cần location)
  - CI/CD: GitHub Actions hoặc GitLab CI - team quen GitLab hơn
- **Draw Demo Architecture**: Vẽ sơ đồ kiến trúc ban đầu trên draw.io, gồm:
  - 3-tier architecture: Frontend (S3 + CloudFront) → API Gateway → Lambda/ECS → RDS
  - Search service riêng cho vector search (hybrid search)
- **Complete SRS (19/10)**: 
  - Viết tài liệu SRS với các phần: Business Requirements, Functional Requirements, Non-functional Requirements
  - Phân công: Thông viết SRS chính, Đạt + Minh viết Business Processes, Quân + Phú document architecture chi tiết

#### **Lab 16: Amazon ElastiCache - Redis**
- **Cluster Mode Disabled vs Enabled**:
  - Disabled: Đơn giản, 1 primary + replicas, max 5 replicas, auto-failover
  - Enabled: Sharding data across multiple nodes, horizontal scaling, higher throughput
  - **Chọn mode nào?**: Disabled cho simple caching, Enabled khi data > 100GB hoặc cần > 1 triệu requests/sec
- **Redis Operations thực hành**:
  - `SET/GET`: Basic key-value, dùng cho session storage
  - `HSET/HGETALL`: Hash cho user profiles, multiple fields per key
  - `PUBLISH/SUBSCRIBE`: Real-time notifications - subscribe channel, publish message
  - `XADD/XREAD`: Streams cho event sourcing, message queue alternative
- **Connection Management**:
  - Security Group phải allow port 6379 từ application
  - Dùng connection pooling (ioredis) để tránh create connection mỗi request
  - **TTL strategy**: Set expiration cho cached data, tránh stale data

#### **Lab 17: AWS Networking Deep Dive**
- **Transit Gateway Architecture**:
  - Hub-and-spoke model: TGW là central hub, mỗi VPC attach như spoke
  - Route propagation: Tự động học routes từ attached VPCs
  - **ECMP**: Multiple VPN tunnels, load balance traffic - tăng throughput lên 2.5Gbps per tunnel × số tunnels
- **Site-to-Site VPN with Cisco CSR**:
  - Cisco CSR 1000v trên EC2 làm Customer Gateway
  - IPSec tunnel với BGP routing
  - **Lesson learned**: BGP convergence mất 30-60s khi failover, plan cho downtime
- **Route53 DNS Integration**:
  - Inbound Endpoint: On-prem resolve AWS private DNS
  - Outbound Endpoint: AWS resolve on-prem DNS
  - Private Hosted Zone: Internal DNS không expose ra internet

#### **Submit Proposal**
- Hoàn thành proposal document với architecture diagram
- Deploy lên GitHub Pages để mentor review
- **Feedback pending**: Đợi mentor review architecture trước khi tiếp tục implementation
