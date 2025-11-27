---
title: "Nhật ký tuần 9"
date: 2025-11-03
weight: 9
chapter: false
pre: " <b> 1.9 </b> "
---

### Mục tiêu tuần 9: CI/CD & DevOps Automation

Tự động hóa quy trình phát triển phần mềm. MapVibe sử dụng GitLab CI để build, test và deploy.

### Nhiệm vụ cần thực hiện trong tuần này:

| Thứ | Nhiệm vụ                                                                                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                               |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------ |
| 2   | - **CI/CD Concepts**:<br>&emsp; + Continuous Integration (Tích hợp liên tục)<br>&emsp; + Continuous Delivery/Deployment (Triển khai liên tục)<br>&emsp; + Pipeline, Stages, Jobs, Runners | 03/11/2025   | 03/11/2025      | [GitLab CI Docs](https://docs.gitlab.com/ee/ci/) |
| 3   | - **GitLab CI Cơ bản**:<br>&emsp; + File `.gitlab-ci.yml`<br>&emsp; + Tạo pipeline đơn giản (Build -> Test)<br>&emsp; + Variables & Secrets (Bảo vệ key AWS)                              | 04/11/2025   | 04/11/2025      |                                                  |
| 4   | - **Build & Test Automation**:<br>&emsp; + Cấu hình cache (node_modules) để tăng tốc build<br>&emsp; + Artifacts (Lưu kết quả build)<br>&emsp; + Chạy Unit Test tự động trong pipeline    | 05/11/2025   | 05/11/2025      |                                                  |
| 5   | - **Deployment Automation**:<br>&emsp; + Deploy to S3 (Frontend)<br>&emsp; + Deploy CDK/SAM (Backend) từ pipeline<br>&emsp; + Environments (Dev, Staging, Prod)                           | 06/11/2025   | 06/11/2025      |                                                  |
| 6   | - **Advanced CI/CD**:<br>&emsp; + Docker-in-Docker (dind) để build image<br>&emsp; + Scheduled Pipelines (Chạy định kỳ)<br>&emsp; + Manual Approval (Duyệt trước khi deploy Prod)         | 07/11/2025   | 07/11/2025      |                                                  |
| 7   | - **Thực hành Lab 9**:<br>&emsp; + Setup GitLab Repo cho dự án mẫu<br>&emsp; + Viết pipeline: Build React App -> Deploy lên S3 hosting                                                    | 08/11/2025   | 08/11/2025      |                                                  |
| CN  | - Tổng kết tuần<br>- Chuẩn bị cho Monitoring                                                                                                                                              | 09/11/2025   | 09/11/2025      |                                                  |

### Kết quả mong đợi tuần 9:

- Tự viết được pipeline CI/CD hoàn chỉnh.
- Hiểu cách bảo mật credentials trong quá trình build.
- Sẵn sàng setup pipeline cho Monorepo của MapVibe.
