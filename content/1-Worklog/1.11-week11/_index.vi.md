---
title: "Nhật ký tuần 11"
date: 2025-11-17
weight: 11
chapter: false
pre: " <b> 1.11 </b> "
---

### Mục tiêu tuần 11: MapVibe Project Kickoff

Sau 10 tuần trang bị kiến thức nền tảng, đây là lúc bắt tay vào xây dựng dự án thực tế MapVibe. Tuần này tập trung vào thiết lập môi trường và cấu trúc dự án.

### Nhiệm vụ cần thực hiện trong tuần này:

| Thứ | Nhiệm vụ                                                                                                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                     |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------------------------------- |
| 2   | - **Phân tích yêu cầu & Kiến trúc**:<br>&emsp; + Review lại `TECH_STACK.md` và `ARCHITECTURE_DECISIONS.md`<br>&emsp; + Vẽ lại luồng dữ liệu (Data Flow)<br>&emsp; + Chốt lại các công nghệ sẽ dùng (Bun, Vite, Lambda, RDS) | 17/11/2025   | 17/11/2025      | [MapVibe Docs](../../docs/)            |
| 3   | - **Setup Monorepo**:<br>&emsp; + Khởi tạo dự án với Turborepo<br>&emsp; + Cấu hình Bun Workspaces<br>&emsp; + Tạo cấu trúc thư mục: `apps/web`, `packages/ui`, `packages/db`                                               | 18/11/2025   | 18/11/2025      | [Turborepo Docs](https://turbo.build/) |
| 4   | - **Frontend Setup**:<br>&emsp; + Init React 19 + Vite trong `apps/web`<br>&emsp; + Cài đặt Tailwind CSS v3<br>&emsp; + Setup UI Component Library cơ bản                                                                   | 19/11/2025   | 19/11/2025      |                                        |
| 5   | - **Backend & Database Setup**:<br>&emsp; + Cấu hình Kysely cho Database migrations<br>&emsp; + Viết migration đầu tiên (User table)<br>&emsp; + Setup Docker Compose chạy Postgres local                                   | 20/11/2025   | 20/11/2025      |                                        |
| 6   | - **CI/CD Initial Setup**:<br>&emsp; + Tạo repo trên GitLab<br>&emsp; + Viết `.gitlab-ci.yml` cơ bản (Install & Build)<br>&emsp; + Push code đầu tiên lên repo                                                              | 21/11/2025   | 21/11/2025      |                                        |
| 7   | - **Review & Planning**:<br>&emsp; + Lên kế hoạch chi tiết cho Sprint 1 (Tuần sau)<br>&emsp; + Phân chia task cụ thể                                                                                                        | 22/11/2025   | 22/11/2025      |                                        |
| CN  | - Nghỉ ngơi & Chuẩn bị tinh thần code dự án                                                                                                                                                                                 | 23/11/2025   | 23/11/2025      |                                        |

### Kết quả mong đợi tuần 11:

- Có một bộ khung dự án (Skeleton) hoàn chỉnh chạy được local.
- Pipeline CI/CD xanh (Pass).
- Sẵn sàng để code tính năng vào tuần sau.
