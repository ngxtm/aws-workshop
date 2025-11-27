---
title: "Nhật ký tuần 7"
date: 2025-10-20
weight: 7
chapter: false
pre: " <b> 1.7 </b> "
---

### Mục tiêu tuần 7: Infrastructure as Code (IaC)

Thay vì click chuột trên Console (ClickOps), chúng ta sẽ dùng code để định nghĩa hạ tầng. Điều này giúp tái sử dụng, kiểm soát phiên bản và triển khai tự động. Với nền tảng TypeScript của dự án, **AWS CDK** là lựa chọn tối ưu.

### Nhiệm vụ cần thực hiện trong tuần này:

| Thứ | Nhiệm vụ                                                                                                                                                                                                                          | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                  |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------------------- |
| 2   | - **IaC Concepts**:<br>&emsp; + Declarative vs Imperative<br>&emsp; + Immutable Infrastructure<br>&emsp; + Giới thiệu AWS CloudFormation (nền tảng của CDK/SAM)                                                                   | 20/10/2025   | 20/10/2025      | [AWS IaC Docs](https://aws.amazon.com/what-is/iac/) |
| 3   | - **AWS CDK (Cloud Development Kit)**:<br>&emsp; + Cài đặt CDK CLI<br>&emsp; + Cấu trúc dự án CDK (App, Stack, Construct)<br>&emsp; + Bootstrap môi trường AWS<br>&emsp; + Thực hành: "Hello World" CDK (Tạo S3 Bucket bằng code) | 21/10/2025   | 21/10/2025      | [AWS CDK Workshop](https://cdkworkshop.com/)        |
| 4   | - **CDK Constructs (L1, L2, L3)**:<br>&emsp; + Phân biệt các level của Construct<br>&emsp; + Sử dụng L2 Constructs (S3, DynamoDB, Lambda)<br>&emsp; + Viết code TypeScript để tạo hạ tầng                                         | 22/10/2025   | 22/10/2025      |                                                     |
| 5   | - **Deploy & Manage Stacks**:<br>&emsp; + `cdk synth` (Tạo CloudFormation template)<br>&emsp; + `cdk deploy` (Triển khai)<br>&emsp; + `cdk diff` (Xem thay đổi)<br>&emsp; + `cdk destroy` (Xóa hạ tầng)                           | 23/10/2025   | 23/10/2025      |                                                     |
| 6   | - **Advanced CDK**:<br>&emsp; + Quản lý Environments (Dev/Prod)<br>&emsp; + Outputs & Exports (Chia sẻ thông tin giữa các Stack)<br>&emsp; + Import resources có sẵn vào CDK                                                      | 24/10/2025   | 24/10/2025      |                                                     |
| 7   | - **Thực hành Lab 7**:<br>&emsp; + Dùng CDK để deploy lại kiến trúc Serverless của tuần 5 (API GW + Lambda + DynamoDB)<br>&emsp; + Cảm nhận sự tiện lợi so với làm thủ công                                                       | 25/10/2025   | 25/10/2025      |                                                     |
| CN  | - Tổng kết tuần<br>- Chuẩn bị cho Container & Docker                                                                                                                                                                              | 26/10/2025   | 26/10/2025      |                                                     |

### Kết quả mong đợi tuần 7:

- Hiểu tư duy "Infrastructure as Code".
- Có thể dùng TypeScript để tạo và quản lý tài nguyên AWS.
- Sẵn sàng để tự động hóa việc triển khai MapVibe.
