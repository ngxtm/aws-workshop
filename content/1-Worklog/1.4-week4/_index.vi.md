---
title: "Nhật ký tuần 4"
date: 2025-09-29
weight: 4
chapter: false
pre: " <b> 1.4 </b> "
---

### Mục tiêu tuần 4: Nắm vững về Networking (VPC)

Đây là kiến thức nền tảng quan trọng nhất để xây dựng hệ thống bảo mật và mở rộng trên AWS. Trước khi đi vào các dịch vụ Serverless hay Database nâng cao cho dự án MapVibe, cần phải hiểu rõ về mạng lưới.

### Nhiệm vụ cần thực hiện trong tuần này:

| Thứ | Nhiệm vụ                                                                                                                                                                                                                                        | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                                                          |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------------------------------------------------------------------------- |
| 2   | - **Học về VPC (Virtual Private Cloud)**:<br>&emsp; + CIDR block là gì?<br>&emsp; + Subnets (Public vs Private)<br>&emsp; + Route Tables & Internet Gateway (IGW)<br>&emsp; + Thực hành: Tạo VPC với 1 Public Subnet và truy cập Internet       | 29/09/2025   | 29/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[AWS VPC Docs](https://docs.aws.amazon.com/vpc/) |
| 3   | - **Kết nối Internet cho Private Subnet**:<br>&emsp; + NAT Gateway vs NAT Instance<br>&emsp; + Egress-only Internet Gateway (IPv6)<br>&emsp; + Thực hành: Cấu hình Private Subnet truy cập internet qua NAT Gateway                             | 30/09/2025   | 30/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| 4   | - **Bảo mật mạng lưới**:<br>&emsp; + Security Groups (Stateful)<br>&emsp; + Network ACLs (Stateless)<br>&emsp; + So sánh và áp dụng mô hình bảo mật nhiều lớp<br>&emsp; + Flow Logs (Giám sát lưu lượng mạng)                                   | 01/10/2025   | 01/10/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| 5   | - **Kết nối nâng cao**:<br>&emsp; + VPC Peering (Kết nối 2 VPC)<br>&emsp; + VPC Endpoints (Gateway vs Interface)<br>&emsp; + VPN (Site-to-Site, Client VPN)<br>&emsp; + Transit Gateway (Tổng quan)                                             | 02/10/2025   | 02/10/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| 6   | - **Load Balancing & Auto Scaling** (Kết hợp với Network):<br>&emsp; + Application Load Balancer (ALB) vs Network Load Balancer (NLB)<br>&emsp; + Auto Scaling Groups (ASG)<br>&emsp; + Thực hành: Deploy ứng dụng web với ALB và ASG trong VPC | 03/10/2025   | 03/10/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| 7   | - **Ôn tập & Lab tổng hợp**:<br>&emsp; + Xây dựng kiến trúc 3-tier (Web, App, DB) trong VPC<br>&emsp; + Kiểm tra kết nối và bảo mật                                                                                                             | 04/10/2025   | 04/10/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)                                                     |
| CN  | - Viết báo cáo tổng kết tuần<br>- Chuẩn bị kiến thức cho Serverless (tuần sau)                                                                                                                                                                  | 05/10/2025   | 05/10/2025      |                                                                                                             |

### Kết quả mong đợi tuần 4:

#### **Networking & Security**

- **VPC Mastery**:
  - Tự tay thiết kế và triển khai được mô hình mạng Custom VPC.
  - Hiểu rõ luồng đi của gói tin (Packet flow) trong AWS.
- **Security**:
  - Biết cách cấu hình Security Group để chỉ cho phép traffic cần thiết (Least Privilege).
  - Phân biệt được khi nào dùng NACL.
- **High Availability**:
  - Hiểu cách thiết kế hệ thống chịu lỗi (Fault Tolerant) dùng Multi-AZ và Load Balancer.

#### **Chuẩn bị cho MapVibe**:

- Kiến thức VPC này sẽ dùng để:
  - Cấu hình **RDS Proxy** (cần nằm trong VPC).
  - Cấu hình **Lambda** truy cập RDS (cần ENI trong VPC).
  - Bảo mật cho **ElastiCache**.
