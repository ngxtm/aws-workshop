---
title: "Nhật ký tuần 11"
date: 2025-11-17
weight: 11
chapter: false
pre: " <b> 1.11 </b> "
---

### Mục tiêu tuần 11:
- **Phát triển Core RAG System**: Xử lý Embedding và Vector Search
- **Xây dựng Database**: Implement PostgreSQL với pgvector
- **API Development**: CRUD cơ bản cho Reviews

### Các đầu việc của tuần 11:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2 | - **Local Database Setup**: Cài đặt PostgreSQL docker<br>- Cấu hình pgvector extension trên local | 17/11/2025 | 17/11/2025 | |
| 3 | - **RAG Implementation (Local)**:<br>&emsp; + Viết script convert text sang vector (Embeddings)<br>&emsp; + Test search similarity trên local DB | 18/11/2025 | 18/11/2025 | |
| 4 | - **Backend API**: Viết API Create Review, Get Review List<br>- Tích hợp logic RAG vào luồng tạo review (tự động vector hóa khi save) | 19/11/2025 | 19/11/2025 | |
| 5 | - **Frontend Base UI**: Layout chung, Header, Footer<br>- Kết nối API Get Review hiển thị lên UI cơ bản | 20/11/2025 | 20/11/2025 | |
| 6 | - **Refine RAG**: Tối ưu prompt cho LLM (Local model/OpenAI test) để trả lời câu hỏi dựa trên context tìm được | 21/11/2025 | 21/11/2025 | |

### Thành tựu đạt được trong tuần 11:
- **Core Feature - RAG**: Đã chạy được luồng RAG cơ bản ở Local. Hệ thống có thể tìm kiếm context liên quan từ vector DB.
- **API Foundation**: Các API CRUD cơ bản đã hoạt động ổn định.
- **Frontend-Backend Integration**: Đã kết nối thành công FE và BE cho các màn hình list danh sách.