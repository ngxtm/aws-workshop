---
title: "Nhật ký tuần 12"
date: 2025-11-24
weight: 12
chapter: false
pre: " <b> 1.12 </b> "
---

### Mục tiêu tuần 12: Sprint 1 - Authentication & Core UI

Tuần đầu tiên của giai đoạn phát triển (Development Phase). Mục tiêu là hoàn thiện khung UI và tính năng đăng nhập/đăng ký.

### Nhiệm vụ cần thực hiện trong tuần này:

| Thứ | Nhiệm vụ                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                       |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ---------------------------------------- |
| 2   | - **UI Components Foundation**:<br>&emsp; + Xây dựng Design System (Colors, Typography)<br>&emsp; + Tạo các component cơ bản: Button, Input, Card, Modal<br>&emsp; + Tích hợp Lucide React icons | 24/11/2025   | 24/11/2025      | [Tailwind CSS](https://tailwindcss.com/) |
| 3   | - **Authentication UI**:<br>&emsp; + Thiết kế trang Login / Register<br>&emsp; + Tạo form layout responsive<br>&emsp; + Validate form (React Hook Form + Zod)                                    | 25/11/2025   | 25/11/2025      |                                          |
| 4   | - **Cognito Integration**:<br>&emsp; + Cấu hình AWS Cognito User Pool<br>&emsp; + Tích hợp AWS SDK vào React App<br>&emsp; + Xử lý luồng Sign Up -> Confirm Code -> Sign In                      | 26/11/2025   | 26/11/2025      |                                          |
| 5   | - **Social Login (Google)**:<br>&emsp; + Cấu hình Google Console Project<br>&emsp; + Setup Cognito Hosted UI<br>&emsp; + Xử lý callback từ Google về App                                         | 27/11/2025   |                 |                                          |
| 6   | - **Homepage Layout**:<br>&emsp; + Xây dựng Header/Footer<br>&emsp; + Hero Section với thanh tìm kiếm<br>&emsp; + Responsive cho Mobile                                                          | 28/11/2025   |                 |                                          |
| 7   | - **Review & Demo Sprint 1**:<br>&emsp; + Kiểm thử luồng đăng nhập/đăng ký<br>&emsp; + Fix bugs UI/UX<br>&emsp; + Merge code vào nhánh `dev`                                                     | 29/11/2025   |                 |                                          |
| CN  | - Lên kế hoạch Sprint 2 (Restaurant Listing)                                                                                                                                                     | 30/11/2025   |                 |                                          |

### Kết quả mong đợi tuần 12:

- Người dùng có thể đăng ký, đăng nhập (Email & Google).
- Giao diện trang chủ cơ bản đã hình thành.
- Codebase sạch sẽ, tái sử dụng được các component.
