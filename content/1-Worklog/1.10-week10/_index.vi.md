---
title: "Nhật ký tuần 10"
date: 2025-11-10
weight: 10
chapter: false
pre: " <b> 1.10 </b> "
---

### Mục tiêu tuần 10:
- Phát triển backend - Hybrid Search Engine (Text search + semantic search)
- Thực hành Lab 22-25: Serverless, API Gateway, CI/CD, CloudWatch, X-Ray
- Nghiên cứu và triển khai hybrid search cho MapVibe

### Các đầu việc của tuần 10:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2 | - Họp team project: Phát triển backend - Hybrid Search Engine<br>&emsp; + Nghiên cứu các best practices, paper về hybrid search engine<br>&emsp; + Thiết kế kiến trúc sơ bộ hybrid search engine | 10/11/2025 | 10/11/2025 | [Hybrid Search Paper](https://arxiv.org/pdf/2408.09236) |
| 3 | - Thực hành Lab 22: Serverless - Tương tác giữa Lambda với S3 và DynamoDB<br>&emsp; + Xử lý và Tối ưu Kích thước Ảnh trên AWS<br>&emsp; + Tạo Lambda Function Xử Lý Ảnh, S3 Bucket<br>&emsp; + Ghi dữ liệu vào Amazon DynamoDB | 11/11/2025 | 11/11/2025 | [Lab 22](https://000078.awsstudygroup.com/vi/) |
| 4 | - Thực hành Lab 23: Serverless - Hướng dẫn viết Front-end gọi API Gateway<br>&emsp; + Triển khai Lambda function (ghi, đọc, xóa dữ liệu)<br>&emsp; + Thiết lập API Gateway, tạo các method<br>&emsp; + Cài đặt và kích hoạt CORS<br>&emsp; + Kiểm tra API với Postman và front-end | 12/11/2025 | 12/11/2025 | [Lab 23](https://000079.awsstudygroup.com/vi/) |
| 5 | - Họp team project: Phát triển backend - Hybrid Search Engine<br>&emsp; + Chạy, kiểm thử hybrid search engine với data đã thu thập<br>&emsp; + Hoàn thiện kiến trúc hybrid search engine | 13/11/2025 | 13/11/2025 | [Supabase Hybrid Search](https://supabase.com/docs/guides/ai/hybrid-search) |
| 6 | - Thực hành Lab 24: Serverless - CI/CD với CodePipeline<br>&emsp; + Tạo kho Git, SAM pipeline<br>&emsp; + Xây dựng pipeline cho front-end<br>- Thực hành Lab 25: Serverless - Giám sát với CloudWatch và X-Ray<br>&emsp; + Gỡ lỗi với CloudWatch Logs<br>&emsp; + Tạo số liệu tùy chỉnh, CloudWatch Alarm<br>&emsp; + Theo dõi với X-ray | 14/11/2025 | 14/11/2025 | [Lab 24](https://000084.awsstudygroup.com/vi/)<br>[Lab 25](https://000085.awsstudygroup.com/vi/) |

### Thành tựu đạt được trong tuần 10:

#### **Team Project - Hybrid Search Engine Development**
- **Daily Meeting (4/11)**: Plan processing query feature
  - Mục tiêu: Query phải xử lý được vị trí địa lý (Quận 1, gần tôi) + semantic meaning
  - Concern: Độ chính xác của query results
- **Research Phase**:
  - Đọc paper về hybrid search: [Hybrid Search](https://arxiv.org/pdf/2408.09236)
  - Tham khảo Supabase hybrid search implementation
  - **Key insight**: Pure vector search miss keyword matches, pure text search miss semantic similar
- **Architecture Design**:
  ```
  Query → [Text Search (BM25)] → Score A
        → [Vector Search]      → Score B
        → RRF Fusion           → Final Ranking
  ```
- **Implementation với PostgreSQL**:
  - Full-text search: `to_tsvector('vietnamese', description) @@ plainto_tsquery('vietnamese', query)`
  - Vector search: `embedding <-> query_embedding` với pgvector
  - **RRF (Reciprocal Rank Fusion)**: `1/(k + rank_a) + 1/(k + rank_b)` với k=60
  - Geospatial filter: `ST_DWithin(location, user_location, radius_meters)`
- **Testing Results**:
  - Query "quán cafe yên tĩnh quận 1" → Top 5 results relevant
  - Query "chỗ ăn sáng gần đây ngon" → Results sorted by distance + rating
  - **Performance**: <100ms cho 10k records với proper indexing

#### **Lab 22: Serverless - Lambda + S3 + DynamoDB**
- **Image Processing Pipeline**:
  - S3 trigger Lambda khi upload image
  - Lambda resize image (thumbnail, medium, large)
  - Store metadata trong DynamoDB
  - **Tip**: Set Lambda memory 1024MB+ cho image processing, CPU scales với memory
- **DynamoDB Patterns**:
  - Single-table design: PK = `PLACE#123`, SK = `REVIEW#456`
  - GSI cho query by different access patterns
  - **Gotcha**: DynamoDB scan = expensive, always use query với key conditions

#### **Lab 23: API Gateway + Lambda**
- **REST API Setup**:
  - Lambda Proxy Integration: API Gateway pass entire request to Lambda
  - Method: GET /places, POST /places, GET /places/{id}
  - **CORS config**: Allow-Origin, Allow-Methods, Allow-Headers - quan trọng cho frontend!
- **Testing Workflow**:
  - Postman collection với environment variables
  - Test từng endpoint trước khi integrate frontend
  - **Tip**: Enable CloudWatch Logs cho API Gateway để debug integration issues

#### **Lab 24 & 25: CI/CD + Monitoring**
- **SAM Pipeline**:
  - `sam init` → `sam build` → `sam deploy`
  - Pipeline stages: Source → Build → Test → Deploy (Dev) → Approve → Deploy (Prod)
  - **Benefit**: Infrastructure và application code cùng repo, deploy together
- **CloudWatch Deep Dive**:
  - **Logs Insights query**:
    ```
    fields @timestamp, @message
    | filter @message like /ERROR/
    | sort @timestamp desc
    | limit 20
    ```
  - **Custom Metrics**: `put-metric-data` cho business metrics (orders/minute, search latency)
  - **Alarms**: Lambda errors > 5 trong 5 phút → SNS notification
- **X-Ray Tracing**:
  - Visualize request flow: API Gateway → Lambda → DynamoDB
  - Identify bottlenecks: Thấy DynamoDB query slow → Add GSI
  - **Service Map**: Overview toàn bộ architecture, dependencies
