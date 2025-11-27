---
title: "Nhật ký tuần 5"
date: 2025-10-06
weight: 5
chapter: false
pre: " <b> 1.5 </b> "
---

### Mục tiêu tuần 5: Serverless Computing (Lambda & API Gateway)

Đây là kiến trúc cốt lõi của dự án MapVibe. Việc chuyển từ máy chủ truyền thống (EC2) sang Serverless giúp giảm chi phí vận hành và tăng khả năng mở rộng.

### Nhiệm vụ cần thực hiện trong tuần này:

| Thứ | Nhiệm vụ                                                                                                                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------- |
| 2   | - **AWS Lambda Cơ bản**:<br>&emsp; + Serverless là gì?<br>&emsp; + Tạo Lambda function đầu tiên (Node.js)<br>&emsp; + Lambda Triggers & Destinations<br>&emsp; + Thực hành: Lambda xử lý sự kiện từ S3 (Resize ảnh đơn giản) | 06/10/2025   | 06/10/2025      | [AWS Lambda Docs](https://docs.aws.amazon.com/lambda/)                    |
| 3   | - **Amazon API Gateway**:<br>&emsp; + REST API vs HTTP API<br>&emsp; + Tích hợp API Gateway với Lambda (Proxy integration)<br>&emsp; + Stages & Deployments<br>&emsp; + Thực hành: Xây dựng API CRUD đơn giản                | 07/10/2025   | 07/10/2025      | [AWS APIGW Docs](https://docs.aws.amazon.com/apigateway/)                 |
| 4   | - **DynamoDB (NoSQL)**:<br>&emsp; + Partition Key & Sort Key<br>&emsp; + Read/Write Capacity Units (RCU/WCU)<br>&emsp; + Global Secondary Indexes (GSI)<br>&emsp; + Thực hành: Lưu trữ dữ liệu từ Lambda vào DynamoDB        | 08/10/2025   | 08/10/2025      | [AWS DynamoDB Docs](https://docs.aws.amazon.com/dynamodb/)                |
| 5   | - **Serverless Framework / SAM**:<br>&emsp; + Giới thiệu về AWS SAM (Serverless Application Model)<br>&emsp; + Viết template.yaml đơn giản<br>&emsp; + Deploy ứng dụng Serverless bằng CLI                                   | 09/10/2025   | 09/10/2025      | [AWS SAM Docs](https://docs.aws.amazon.com/serverless-application-model/) |
| 6   | - **Lambda Advanced**:<br>&emsp; + Lambda Layers (Quản lý thư viện dùng chung)<br>&emsp; + Lambda trong VPC (Kết nối RDS sau này)<br>&emsp; + Cold Starts & Provisioned Concurrency                                          | 10/10/2025   | 10/10/2025      |                                                                           |
| 7   | - **Ôn tập & Mini Project**:<br>&emsp; + Xây dựng API "To-Do List" hoàn chỉnh:<br>&emsp; &emsp; API Gateway -> Lambda -> DynamoDB<br>&emsp; + Test bằng Postman                                                              | 11/10/2025   | 11/10/2025      |                                                                           |
| CN  | - Tổng kết tuần<br>- So sánh Serverless vs EC2                                                                                                                                                                               | 12/10/2025   | 12/10/2025      |                                                                           |

### Kết quả mong đợi tuần 5:

- Hiểu rõ luồng hoạt động của Serverless.
- Tự xây dựng được RESTful API không cần quản lý server.
- Chuẩn bị nền tảng để viết Backend cho MapVibe.
