---
title: "Nhật ký tuần 6"
date: 2025-10-13
weight: 6
chapter: false
pre: " <b> 1.6 </b> "
---

### Mục tiêu tuần 6: Advanced Database & Authentication

Tập trung vào các dịch vụ dữ liệu nâng cao và bảo mật người dùng, hai thành phần quan trọng nhất của MapVibe (Geospatial search & User Accounts).

### Nhiệm vụ cần thực hiện trong tuần này:

| Thứ | Nhiệm vụ                                                                                                                                                                                                                                                          | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                               |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------------------------------------------------- |
| 2   | - **Amazon RDS Advanced**:<br>&emsp; + Multi-AZ Deployments (High Availability)<br>&emsp; + Read Replicas (Performance)<br>&emsp; + **RDS Proxy**: Tại sao cần dùng với Lambda?<br>&emsp; + Thực hành: Tạo RDS Postgres và kết nối từ máy local                   | 13/10/2025   | 13/10/2025      | [AWS RDS Docs](https://docs.aws.amazon.com/rds/)                 |
| 3   | - **PostgreSQL & PostGIS**:<br>&emsp; + Cài đặt extension PostGIS<br>&emsp; + Các kiểu dữ liệu không gian (Geometry, Geography)<br>&emsp; + Truy vấn không gian: Tìm điểm gần nhất (Nearest Neighbor)<br>&emsp; + Thực hành: Query tìm quán ăn trong bán kính 5km | 14/10/2025   | 14/10/2025      | [PostGIS Docs](https://postgis.net/)                             |
| 4   | - **Amazon ElastiCache (Redis)**:<br>&emsp; + Caching strategies (Lazy Loading vs Write Through)<br>&emsp; + Redis Cluster mode<br>&emsp; + Ứng dụng: Cache kết quả tìm kiếm, Session store                                                                       | 15/10/2025   | 15/10/2025      | [AWS ElastiCache Docs](https://docs.aws.amazon.com/elasticache/) |
| 5   | - **Amazon Cognito (Authentication)**:<br>&emsp; + User Pools vs Identity Pools<br>&emsp; + JWT Tokens (ID, Access, Refresh)<br>&emsp; + Hosted UI & Social Login (Google/Facebook)<br>&emsp; + Thực hành: Tạo trang đăng nhập với Cognito                        | 16/10/2025   | 16/10/2025      | [AWS Cognito Docs](https://docs.aws.amazon.com/cognito/)         |
| 6   | - **Bảo mật Database & Auth**:<br>&emsp; + Quản lý Secrets Manager (Lưu DB credentials)<br>&emsp; + IAM Authentication cho RDS<br>&emsp; + Tích hợp Cognito Authorizer vào API Gateway                                                                            | 17/10/2025   | 17/10/2025      |                                                                  |
| 7   | - **Ôn tập & Lab**:<br>&emsp; + Dựng mô hình: API Gateway (Cognito Auth) -> Lambda -> RDS Proxy -> PostgreSQL<br>&emsp; + Đây là kiến trúc backend thực tế của MapVibe                                                                                            | 18/10/2025   | 18/10/2025      |                                                                  |
| CN  | - Tổng kết tuần<br>- Chuẩn bị cho Infrastructure as Code                                                                                                                                                                                                          | 19/10/2025   | 19/10/2025      |                                                                  |

### Kết quả mong đợi tuần 6:

- Hiểu cách xử lý dữ liệu bản đồ với PostGIS.
- Nắm vững cơ chế xác thực người dùng hiện đại với Cognito.
- Hiểu tầm quan trọng của Caching và Connection Pooling trong hệ thống lớn.
