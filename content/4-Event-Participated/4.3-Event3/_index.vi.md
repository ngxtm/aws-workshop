---
title: "Sự kiện 3"
date: 2025-11-15
weight: 5
chapter: false
pre: " <b> 4.3 </b> "
---

## AWS Cloud Mastery Series #1

### Thông tin Sự kiện

&emsp; **Tên sự kiện:** ​AI/ML/GenAI on AWS <br>
&emsp; **Ngày tháng:** 15 tháng 11 năm 2025 <br>
&emsp; **Diễn giả:** Danh Hoang Hieu Nghi (AI Engineer, Renova Cloud), Dinh Le Hoang Anh (Cloud Engineer Trainee), Lam Tuan Kiet (Senior DevOps Engineer FPT Software) <br>
&emsp; **Vai trò:** Người tham dự <br>
&emsp; **Chủ đề chính:** Xây dựng các tác nhân thông minh và ứng dụng trí tuệ nhân tạo sinh tạo với Amazon Bedrock <br>

### Tổng quan Sự kiện

Hội thảo này cung cấp một cái nhìn sâu sắc về Amazon Bedrock và khả năng của nó trong việc xây dựng các tác nhân thông minh và ứng dụng trí tuệ nhân tạo sinh tạo. Phiên làm việc bao gồm các mô hình nền tảng, kỹ thuật kỹ thuật prompt (prompt engineering), retrieval-augmented generation (RAG), và các triển khai thực tế của Bedrock agents với các ví dụ thực tế.

### Các Chủ đề Chính

#### 1. Amazon Bedrock Agent Core

Amazon Bedrock Agents cho phép các quy trình công việc đa bước tự động và tích hợp công cụ, cho phép AI đưa ra quyết định và thực hiện các hành động mà không cần can thiệp của con người.

**Các khả năng chính:**

- Điều phối quy trình công việc đa bước
- Tích hợp công cụ và gọi hàm
- Quyết định tự trị dựa trên ngữ cảnh
- Tích hợp API và nguồn dữ liệu thời gian thực
- Quản lý bộ nhớ và ngữ cảnh của tác nhân

**Điểm nổi bật của Kiến trúc:**

- Quy trình công việc tác nhân dựa trên sự kiện
- Quản lý hội thoại có trạng thái
- Cơ chế xử lý lỗi và thử lại tích hợp sẵn
- Hỗ trợ cho chuỗi công cụ và logic có điều kiện

---

#### 2. Trí tuệ Nhân tạo Sinh tạo với Amazon Bedrock

##### 2.1 Các Mô hình Nền tảng: Lựa chọn & So sánh

Các mô hình nền tảng có sẵn trên AWS:

- **Claude** (của Anthropic) - Lý luận nâng cao và hiểu bối cảnh dài
- **Llama** (của Meta) - Mã nguồn mở, linh hoạt, tiết kiệm chi phí
- **Titan** (của AWS) - Được tối ưu hóa cho hệ sinh thái AWS, hỗ trợ đa ngôn ngữ

**Hướng dẫn Lựa chọn:**

- **Claude**: Tốt nhất cho lý luận phức tạp, tạo văn bản sắc thái, và các tài liệu dài
- **Llama**: Lý tưởng cho các triển khai tiết kiệm chi phí và ứng dụng ưu tiên quyền riêng tư
- **Titan**: Được khuyên dùng cho các ứng dụng gốc AWS và embeddings

##### 2.2 Kỹ thuật Prompt Engineering

Các kỹ thuật cơ bản để tương tác hiệu quả với mô hình:

**Các kỹ thuật:**

- **Prompting Cơ bản**: Thiết kế hướng dẫn rõ ràng, cụ thể
- **Chain-of-Thought (CoT) Lý luận**: Chia nhỏ các vấn đề phức tạp thành các bước hợp lý

  - Cải thiện độ chính xác cho lý luận toán học và logic
  - Tăng độ tin cậy của mô hình cho các nhiệm vụ đa bước
  - Ví dụ: "Hãy suy nghĩ từng bước..."

- **Few-Shot Learning**: Cung cấp các ví dụ trong prompt
  - 2-3 ví dụ thường đủ
  - Cải thiện hiệu suất cụ thể cho nhiệm vụ
  - Giảm các phản ứng vô căn cứ và ngoài chủ đề

**Các Thực hành Tốt nhất:**

- Sử dụng các hướng dẫn cụ thể, chi tiết
- Cung cấp bối cảnh và ràng buộc
- Bao gồm các ví dụ định dạng đầu ra mong muốn
- Sử dụng prompting dựa trên vai trò ("Bạn là một...")

##### 2.3 Retrieval-Augmented Generation (RAG)

Kiến trúc để nâng cao AI với kiến thức bên ngoài mà không cần tinh chỉnh mô hình.

**Kiến trúc RAG:**

1. **Nhập tài liệu**: Tải lên và chia nhỏ cơ sở kiến thức
2. **Tạo Embedding**: Chuyển đổi tài liệu thành biểu diễn vector
3. **Lưu trữ Vector**: Lưu trữ embeddings trong cơ sở dữ liệu vector
4. **Xử lý Truy vấn**: Chuyển đổi truy vấn người dùng thành embeddings
5. **Tìm kiếm Tương tự**: Tìm các tài liệu liên quan từ cơ sở kiến thức
6. **Tiêm Bối cảnh**: Bao gồm các tài liệu được truy xuất trong prompt
7. **Tạo Phản ứng**: Mô hình tạo câu trả lời dựa trên bối cảnh

**Tích hợp Cơ sở Kiến thức:**

- S3 buckets để lưu trữ tài liệu
- OpenSearch, Pinecone, hoặc cơ sở dữ liệu Vector cho embeddings
- Lambda để xử lý và lập chỉ mục
- CloudWatch để giám sát chất lượng cơ sở kiến thức

**Lợi ích:**

- Thông tin hiện tại mà không cần huấn luyện lại mô hình
- Nguồn tham khảo được trích dẫn và xác minh sự kiện
- Giảm các phản ứng vô căn cứ
- Tích hợp kiến thức cụ thể miền

##### 2.4 Bedrock Agents: Triển khai Thực tế

Xây dựng các quy trình công việc đa bước với tích hợp công cụ.

**Các thành phần chính:**

- Bộ não tác nhân (mô hình nền tảng)
- Các nhóm hành động (công cụ/API có sẵn)
- Các cơ sở kiến thức (tài liệu RAG)
- Prompts và hướng dẫn
- Quản lý phiên

**Trường hợp sử dụng:**

- Trợ lý dịch vụ khách hàng thông minh
- Quy trình công việc phân tích tài liệu và tóm tắt
- Các tác nhân tạo mã và gỡ lỗi
- Tự động hóa phân tích dữ liệu và báo cáo

##### 2.5 Guardrails: An toàn & Lọc Nội dung

Triển khai các biện pháp an toàn cho các ứng dụng AI sản xuất.

**Tính năng Guardrails:**

- Lọc nội dung (từ tục, nội dung tường minh)
- Phát hiện và loại bỏ PII (Thông tin Nhận dạng Cá nhân)
- Chặn nội dung có hại
- Nhận thức chủ đề nhạy cảm
- Định nghĩa quy tắc tùy chỉnh

**Triển khai:**

- Guardrails nội tuyến trong các lời gọi mô hình
- Lọc trước và sau xử lý
- Ghi nhật ký kiểm toán cho các sự kiện an toàn
- Theo dõi tuân thủ

##### 2.6 Demo Trực tiếp: Xây dựng Chatbot Trí tuệ Nhân tạo Sinh tạo

Hướng dẫn xây dựng một chatbot sản xuất sẵn sàng:

**Kiến trúc:**

- Frontend: Giao diện trò chuyện (web/mobile)
- API Gateway: Định tuyến yêu cầu tới backend
- Lambda: Điều phối các cuộc gọi Bedrock API
- RDS/DynamoDB: Lưu trữ lịch sử hội thoại và dữ liệu người dùng
- CloudWatch: Giám sát và ghi nhật ký các tương tác
- VectorDB: Lưu trữ cơ sở kiến thức RAG

**Các bước triển khai chính:**

1. Thiết lập các vai trò IAM và truy cập Bedrock
2. Tạo API gọi mô hình
3. Triển khai quản lý bộ nhớ hội thoại
4. Tích hợp RAG với cơ sở kiến thức
5. Thêm xử lý lỗi và giới hạn tốc độ
6. Triển khai với khả năng mở rộng

---

#### 3. Hệ sinh thái Dịch vụ AI của AWS

##### 3.1 Amazon Rekognition

**Mục đích:** Dịch vụ thị giác máy tính để phân tích hình ảnh và video

**Khả năng:**

- Phát hiện đối tượng và cảnh
- Nhận dạng khuôn mặt và phân tích
- Phát hiện văn bản trong hình ảnh (OCR)
- Nhận dạng người nổi tiếng
- Phát hiện hoạt động trong video

**Trường hợp sử dụng trong Thực tế:**

- Kiểm duyệt nội dung cho các nền tảng truyền thông xã hội
- Các hệ thống bảo mật và giám sát (nhận dạng lại người)
- Các tính năng trợ năng (tạo mô tả hình ảnh)
- Phân tích bán lẻ (theo dõi kho hàng trên kệ)
- Phân tích hình ảnh y tế (với các mô hình tùy chỉnh)

---

##### 3.2 Amazon Polly

**Mục đích:** Dịch vụ chuyển đổi văn bản thành giọng nói

**Tính năng:**

- Hơn 140+ giọng nói trên 30+ ngôn ngữ
- Neural TTS để có chất lượng giống con người
- Đánh dấu SSML để kiểm soát chi tiết
- Từ điển phát âm tùy chỉnh

**Trường hợp sử dụng trong Thực tế:**

- Tạo sách nói và podcast
- Các tính năng trợ năng cho người khiếm thị
- Các hệ thống phản hồi giọng nói tương tác (IVR)
- Tiếng nói cho nền tảng e-learning
- Tạo đối thoại nhân vật trò chơi

---

##### 3.3 Amazon Transcribe

**Mục đích:** Dịch vụ nhận dạng giọng nói tự động (ASR)

**Tính năng:**

- Phiên âm thời gian thực và theo lô
- Dấu câu tự động
- Nhận dạng người nói
- Hỗ trợ thuật ngữ y tế
- Hỗ trợ đa ngôn ngữ

**Trường hợp sử dụng trong Thực tế:**

- Phiên âm cuộc họp và cuộc gọi (tích hợp Teams, Zoom)
- Ghi âm tuân thủ cho các tổ chức tài chính
- Đảm bảo chất lượng dịch vụ khách hàng
- Tài liệu pháp lý và y tế
- Khả năng tiếp cận nội dung (phụ đề video)

---

##### 3.4 Amazon Comprehend

**Mục đích:** Xử lý ngôn ngữ tự nhiên và phân tích văn bản

**Khả năng:**

- Phân tích tư cảm
- Nhận dạng thực thể (người, địa điểm, tổ chức)
- Trích xuất cụm từ chính
- Phát hiện ngôn ngữ
- Mô hình hóa chủ đề

**Trường hợp sử dụng trong Thực tế:**

- Giám sát tư cảm truyền thông xã hội
- Phân tích phản hồi khách hàng
- Sàng lọc sơ yếu lý lịch và CV (trích xuất thực thể)
- Phân loại và gắn thẻ nội dung
- Phát hiện gian lận (các mẫu ngôn ngữ)

---

##### 3.5 Các Dịch vụ AI Khác

**Amazon Lookout for Anomalies:** Phát hiện các mẫu bất thường trong các chỉ số và dữ liệu chuỗi thời gian
**Amazon Forecast:** Dự báo chuỗi thời gian để lập kế hoạch nhu cầu
**Amazon Personalize:** Công cụ cá nhân hóa thời gian thực cho các khuyến nghị
**Amazon Textract:** Trích xuất văn bản và dữ liệu từ tài liệu
**Amazon Lookout for Vision:** Phát hiện các khiếm khuyết trong sản xuất/kiểm soát chất lượng

---

### Phiên Hỏi & Đáp: Các Thách thức & Giải pháp Kỹ thuật

#### Q1: Quản lý Tin nhắn Lớn Mà Không Sử dụng SQS

**Phát biểu vấn đề:**
Xử lý tin nhắn khối lượng lớn và tải trọng lớn mà không sử dụng SQS.

**Các giải pháp được thảo luận:**

1. **Gọi trực tiếp với Giới hạn Đồng thời**

   - Sử dụng Lambda dự trữ đồng thời
   - Triển khai giới hạn tốc độ cấp ứng dụng
   - Sử dụng DynamoDB để theo dõi yêu cầu đang xử lý
   - Ưu: Đơn giản, tiết kiệm chi phí cho tải nhẹ đến vừa
   - Nhược: Thông lượng giới hạn, không có cơ chế thử lại tích hợp sẵn

2. **DynamoDB Streams**

   - Ghi tin nhắn vào DynamoDB
   - Lambda được kích hoạt bởi DynamoDB Streams
   - Cung cấp đảm bảo sắp xếp và khả năng phát lại
   - Ưu: Độ bền, xử lý theo lô
   - Nhược: Chi phí lưu trữ, độ trễ cao hơn

3. **Kinesis Data Streams**

   - Luồng khối lượng lớn tin nhắn
   - Mở rộng dựa trên mảnh dữ liệu
   - Các nhóm người tiêu dùng để xử lý song song
   - Thay thế tốt hơn SQS cho các kịch bản khối lượng cao
   - Ưu: Thông lượng cao, xử lý thời gian thực, phát lại
   - Nhược: Chi phí cao hơn, thiết lập phức tạp hơn

4. **S3 + Thông báo Sự kiện**
   - Tải trọng lớn đến S3
   - Sự kiện S3 kích hoạt xử lý
   - Hoạt động tốt cho các công việc hàng loạt
   - Ưu: Kích thước tải trọng không giới hạn, lưu trữ bền
   - Nhược: Độ trễ cao hơn, tính nhất quán cuối cùng

**Khuyến cáo:**
Trong hầu hết các trường hợp, **Kinesis** cung cấp sự cân bằng tốt nhất giữa thông lượng, tính năng và chi phí. Đối với xử lý hàng loạt đơn giản, **S3 + Lambda** là đủ.

---

#### Q2: Giới hạn Tốc độ Chatbot AI (Giới hạn 1-2 yêu cầu/phút)

**Phát biểu vấn đề:**
Ràng buộc API hoặc mô hình giới hạn chatbot ở 1-2 yêu cầu trên phút, cần các giải pháp để xử lý nhu cầu người dùng cao hơn.

**Các giải pháp được thảo luận:**

1. **Xếp hàng Yêu cầu & Xử lý Theo lô**

   - Xếp hàng các yêu cầu đến trong SQS/Kinesis
   - Xử lý theo lô với tốc độ cho phép
   - Gửi thông báo hoàn thành qua SNS/EventBridge
   - Cung cấp điểm cuối để người dùng kiểm tra trạng thái
   - Ưu: Xếp hàng công bằng, xử lý áp lực ngược
   - Nhược: Độ trễ cao hơn cho người dùng cuối

2. **Bộ nhớ đệm Phản ứng**

   - Bộ nhớ đệm các câu hỏi và phản ứng phổ biến
   - Kiểm tra bộ nhớ đệm trước khi thực hiện cuộc gọi API
   - Sử dụng DynamoDB hoặc ElastiCache để lưu trữ
   - Làm mất hiệu lực bộ nhớ đệm dựa trên TTL
   - Ưu: Giảm đáng kể các cuộc gọi API
   - Nhược: Rủi ro dữ liệu cũ, chi phí quản lý bộ nhớ đệm

3. **Kiểm soát Đồng thời Dựa trên Phiên**

   - Triển khai giới hạn tốc độ theo người dùng hoặc phiên
   - Theo dõi các phiên đồng thời trong DynamoDB
   - Xếp hàng yêu cầu từ cùng phiên theo thứ tự
   - Ưu: Ngăn chặn một người dùng duy nhất chi phối sức mạnh
   - Nhược: Quản lý trạng thái phức tạp

4. **Nhiều Phiên bản Mô hình / Cân bằng Tải**

   - Nếu sử dụng các mô hình tại chỗ hoặc trong container
   - Chạy nhiều phiên bản phía sau bộ cân bằng tải
   - Phân phối yêu cầu trên các phiên bản
   - Ưu: Thông lượng tăng
   - Nhược: Chi phí cơ sở hạ tầng

5. **Yêu cầu Ưu tiên & Hàng đợi Ưu tiên**

   - Triển khai các mức ưu tiên (người dùng VIP, yêu cầu quan trọng)
   - SQS với các thuộc tính ưu tiên tin nhắn
   - Xử lý các yêu cầu ưu tiên cao trước
   - Ưu: Đảm bảo SLA cho người dùng quan trọng
   - Nhược: Độ phức tạp trong logic công bằng

6. **Xử lý Không đồng bộ với Callbacks**
   - Chấp nhận yêu cầu và trả về ID công việc ngay lập tức
   - Xử lý không đồng bộ (chờ 0-5 phút)
   - Thông báo cho người dùng qua webhook/email khi sẵn sàng
   - WebSocket để cập nhật thời gian thực
   - Ưu: UX tốt hơn, tôn trọng giới hạn API
   - Nhược: Yêu cầu thay đổi kiến trúc không đồng bộ

**Kiến trúc Được khuyến cáo cho Chatbot AI:**

```
Yêu cầu người dùng → API Gateway
       ↓
   [Kiểm tra Bộ nhớ đệm] → Trả về nếu tìm thấy
       ↓
   [Hàng đợi SQS] → Thực thi giới hạn tốc độ (1-2 yêu cầu/phút)
       ↓
   [Lambda] → Gọi Bedrock API
       ↓
   [DynamoDB] → Bộ nhớ đệm kết quả
       ↓
   [SNS] → Thông báo cho người dùng (email/webhook)
```

**Các Chỉ số Chính cần Giám sát:**

- Độ sâu của hàng đợi
- Độ trễ API
- Tỷ lệ bộ nhớ đệm trúng
- Độ trễ từ đầu đến cuối của người dùng
- Các sự kiện giới hạn API

---

### Những Điều Học Được và Kết quả

**Kiến thức Kỹ thuật Đạt được:**

- Hiểu sâu về kiến trúc Amazon Bedrock và khả năng
- Lựa chọn mô hình nền tảng và so sánh
- Các kỹ thuật prompt engineering nâng cao (CoT, Few-shot)
- Các mẫu triển khai RAG để tích hợp kiến thức
- Bedrock Agents để tự động hóa quy trình công việc đa bước
- Hệ sinh thái dịch vụ AI của AWS và các trường hợp sử dụng

**Giải pháp Thực tế:**

- Các mẫu giới hạn tốc độ và xử lý tin nhắn hiệu quả cho ứng dụng AI
- Chiến lược bộ nhớ đệm để tối ưu hóa việc sử dụng API
- Kiến trúc dựa trên hàng đợi để xử lý các nút thắt
- Các ví dụ thực tế về tích hợp dịch vụ AI

**Các Mẫu Kiến trúc Học được:**

- Kiến trúc chatbot serverless
- Thiết kế quy trình RAG
- Quy trình công việc tự động hóa dựa trên tác nhân
- Triển khai AI an toàn và tuân thủ với guardrails

### Suy ngẫm Cá nhân

Hội thảo này nâng cao đáng kể sự hiểu biết của tôi về xây dựng các ứng dụng trí tuệ nhân tạo sinh tạo sản xuất sẵn sàng trên AWS. Sự kết hợp giữa kiến thức về mô hình nền tảng, các kỹ thuật prompt nâng cao, và các mẫu kiến trúc thực tế cung cấp một nền tảng vững chắc để phát triển các tác nhân thông minh và các ứng dụng được cấp năng lượng bởi AI.

Các lĩnh vực chính để khám phá thêm:

- RAG nâng cao với tìm kiếm kết hợp (ngữ nghĩa + từ khóa)
- Tinh chỉnh các mô hình nền tảng cho các miền chuyên biệt
- Các hệ thống đa tác nhân và hợp tác tác nhân
- MLOps và giám sát cho các ứng dụng GenAI
- Tối ưu hóa chi phí cho khối lượng công việc AI cao

### Ảnh Sự kiện

#### Kỹ thuật Prompt Engineering & Zero-Shot Prompting

![Kỹ thuật Prompting: Trình bày Zero-Shot Prompting](/images/Event-Participated/pic8.jpg)

<figure>
    <figcaption>Bài thuyết trình kỹ thuật về Kỹ thuật Prompting tập trung vào các phương pháp Zero-Shot Prompting, bao gồm các kỹ thuật phân loại và chiến lược nhập văn bản cho prompt engineering hiệu quả<figcaption>
</figure>

#### Quy trình Tạo Văn bản & Kiến trúc RAG

![Quy trình Tạo Văn bản - Chuyên sâu Kiến trúc RAG](/images/Event-Participated/pic9.jpg)

<figure>
    <figcaption>Giải thích chi tiết về Quy trình Tạo Văn bản hiển thị các thành phần kiến trúc RAG (Retrieval-Augmented Generation) bao gồm mô hình embeddings, vector store, tìm kiếm ngữ nghĩa, tăng cường ngữ cảnh, và quy trình truy xuất dữ liệu<figcaption>
</figure>

#### Các Phiên Hội thảo Kỹ thuật Bổ sung

![Phiên Hội thảo - Nội dung Kỹ thuật Bổ sung](/images/Event-Participated/pic10.jpg)

<figure>
    <figcaption>Bài thuyết trình hội thảo về các kỹ thuật AI/ML nâng cao và tích hợp dịch vụ AWS<figcaption>
</figure>

![Môi trường Hội thảo - Trình diễn Kỹ thuật Trực tiếp](/images/Event-Participated/pic11.jpg)

<figure>
    <figcaption>Những người tham dự tập trung vào các bài thuyết trình kỹ thuật và học tập về các dịch vụ AI của AWS<figcaption>
</figure>

![Cộng đồng Hội thảo & Chia sẻ Kiến thức](/images/Event-Participated/pic12.jpg)

<figure>
    <figcaption>Những người tham dự hội thảo tham gia thảo luận và trao đổi kiến thức trong sự kiện AWS Cloud Mastery<figcaption>
</figure>

#### Các Bài thuyết trình Diễn giả & Phiên Chuyên gia

![Bài thuyết trình Khai mạc Amazon Bedrock AgentCore của Danh Hoang Hieu Nghi](/images/Event-Participated/pic13.jpg)

<figure>
    <figcaption>Bài thuyết trình chính về Amazon Bedrock AgentCore được trình bày bởi Danh Hoang Hieu Nghi (Kỹ sư AI, Renova Cloud), giới thiệu các khái niệm cốt lõi về kiến trúc tác nhân thông minh và điều phối quy trình công việc đa bước<figcaption>
</figure>

![Tổng quan Dịch vụ AI của AWS - Các Dịch vụ AI Được Đào tạo Trước Khác](/images/Event-Participated/pic14.jpg)

<figure>
    <figcaption>Bài thuyết trình kỹ thuật về Các Dịch vụ AI Được Đào tạo Trước bao gồm Rekognition, Polly, Transcribe, và Comprehend, trình bày hệ sinh thái dịch vụ AI/ML toàn diện của AWS cho các ứng dụng thực tế<figcaption>
</figure>

![Trí tuệ Nhân tạo Sinh tạo với Amazon Bedrock - Thảo luận Nhóm Diễn giả Đa phương](/images/Event-Participated/pic15.jpg)

<figure>
    <figcaption>Thảo luận nhóm diễn giả đa phương về Trí tuệ Nhân tạo Sinh tạo với Amazon Bedrock có đại diện Lam Tuan Kiet (Kỹ sư DevOps Cấp cao FPT Software), Dinh Le Hoang Anh (Trainee Cloud Engineer), và các diễn giả khác thảo luận về các triển khai thực tế và các thực hành tốt nhất<figcaption>
</figure>

![Ảnh Nhóm Hội thảo - Kết thúc Sự kiện AWS Cloud Mastery](/images/Event-Participated/pic16.jpg)

<figure>
    <figcaption>Ảnh nhóm những người tham dự hội thảo và diễn giả trước màn hình hiển thị tương tác Kahoot, đại diện cho môi trường học tập hợp tác và hoàn thành thành công sự kiện AWS Cloud Mastery Series #1<figcaption>
</figure>
