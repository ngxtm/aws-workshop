---
title: "Nhật ký tuần 8"
date: 2025-10-27
weight: 8
chapter: false
pre: " <b> 1.8 </b> "
---

### Mục tiêu tuần 8: Containers & Local Development

Dù MapVibe dùng Serverless, nhưng Docker là công cụ không thể thiếu để chạy môi trường phát triển cục bộ (Local DB, Redis). Ngoài ra, hiểu về ECS/EKS giúp có cái nhìn toàn diện về Compute.

### Nhiệm vụ cần thực hiện trong tuần này:

| Thứ | Nhiệm vụ                                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                                       |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------------------------------------------------------------------------- |
| 2   | - **Docker Fundamentals**:<br>&emsp; + Container vs VM<br>&emsp; + Dockerfile, Image, Container<br>&emsp; + Docker Compose (Quản lý multi-container)<br>&emsp; + Thực hành: Viết Dockerfile cho ứng dụng Node.js | 27/10/2025   | 27/10/2025      | [Docker Docs](https://docs.docker.com/)                                                  |
| 3   | - **Local Dev Environment**:<br>&emsp; + Dùng Docker Compose để chạy PostgreSQL + PostGIS<br>&emsp; + Dùng Docker Compose để chạy Redis<br>&emsp; + Kết nối ứng dụng Node.js local vào Docker services           | 28/10/2025   | 28/10/2025      |                                                                                          |
| 4   | - **Amazon ECR (Elastic Container Registry)**:<br>&emsp; + Push/Pull Docker images<br>&emsp; + Quản lý Image tags và lifecycle<br>&emsp; + Scan vulnerabilities                                                  | 29/10/2025   | 29/10/2025      | [AWS ECR Docs](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)  |
| 5   | - **Amazon ECS (Elastic Container Service)**:<br>&emsp; + EC2 Launch Type vs Fargate (Serverless Containers)<br>&emsp; + Task Definitions & Services<br>&emsp; + Cluster management                              | 30/10/2025   | 30/10/2025      | [AWS ECS Docs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) |
| 6   | - **Amazon EKS (Kubernetes) - Tổng quan**:<br>&emsp; + Kubernetes là gì? (Pods, Deployments, Services)<br>&emsp; + Khi nào dùng EKS vs ECS vs Lambda?<br>&emsp; + (Không cần đi sâu, chỉ cần hiểu concept)       | 31/10/2025   | 31/10/2025      |                                                                                          |
| 7   | - **Thực hành Lab 8**:<br>&emsp; + Đóng gói ứng dụng Web vào Docker Image<br>&emsp; + Push lên ECR<br>&emsp; + Deploy thử nghiệm lên ECS Fargate                                                                 | 01/11/2025   | 01/11/2025      |                                                                                          |
| CN  | - Tổng kết tuần<br>- Chuẩn bị cho CI/CD                                                                                                                                                                          | 02/11/2025   | 02/11/2025      |                                                                                          |

### Kết quả mong đợi tuần 8:

- Thành thạo Docker để setup môi trường dev nhanh chóng.
- Hiểu sự khác biệt giữa chạy code trên Lambda và Container.
- Biết cách đóng gói ứng dụng chuẩn hóa.
