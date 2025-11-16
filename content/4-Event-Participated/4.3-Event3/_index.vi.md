---
title: "Sự kiện 3"
date: 2025-11-15
weight: 3
chapter: false
pre: " <b> 4.3 </b> "
---

## AWS Cloud Mastery Series #1

### Thông tin Sự kiện

&emsp; **Tên sự kiện:** ​AI/ML/GenAI on AWS <br>
&emsp; **Ngày tháng:** 15 tháng 11 năm 2025 <br>
&emsp; **Diễn giả:** Danh Hoang Hieu Nghi (Kỹ sư AI, Renova Cloud), Dinh Le Hoang Anh (Trainee Cloud Engineer), Lam Tuan Kiet (Kỹ sư DevOps Cấp cao FPT Software) <br>
&emsp; **Vai trò:** Người tham dự <br>
&emsp; **Chủ đề chính:** Xây dựng các tác nhân thông minh và ứng dụng trí tuệ nhân tạo sinh tạo với Amazon Bedrock <br>

### Tổng quan Sự kiện

Hội thảo này cung cấp một cái nhìn sâu sắc toàn diện về Amazon Bedrock và khả năng của nó trong việc xây dựng các tác nhân thông minh và ứng dụng trí tuệ nhân tạo sinh tạo. Phiên làm việc bao gồm các mô hình nền tảng, kỹ thuật kỹ thuật prompt, retrieval-augmented generation (RAG), kiến trúc tác nhân, và các giải pháp thực tế cho các thách thức triển khai AI thực tế. Nhiều diễn giả chuyên gia đã cung cấp các bài thuyết trình kỹ thuật với các bản trình diễn trực tiếp và ví dụ thực hành.

### Các Chủ đề Chính

#### 1. Các Nguyên tắc Cơ bản của Prompt Engineering

##### Kỹ thuật Prompting: Zero-Shot Prompting

![Kỹ thuật Prompting: Trình bày Zero-Shot Prompting](/images/Event-Participated/pic8.jpg)

<figure>
    <figcaption>Bài thuyết trình kỹ thuật về Kỹ thuật Prompting tập trung vào các phương pháp Zero-Shot Prompting, bao gồm các kỹ thuật phân loại và chiến lược nhập văn bản cho prompt engineering hiệu quả<figcaption>
</figure>

Các kỹ thuật cơ bản để tương tác hiệu quả với mô hình:

**Các Phương pháp Prompting Cốt lõi:**

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

---

#### 2. Quy trình Tạo Văn bản & Kiến trúc RAG

##### Chuyên sâu Retrieval-Augmented Generation

![Quy trình Tạo Văn bản - Chuyên sâu Kiến trúc RAG](/images/Event-Participated/pic9.jpg)

<figure>
    <figcaption>Giải thích chi tiết về Quy trình Tạo Văn bản hiển thị các thành phần kiến trúc RAG (Retrieval-Augmented Generation) bao gồm mô hình embeddings, vector store, tìm kiếm ngữ nghĩa, tăng cường ngữ cảnh, và quy trình truy xuất dữ liệu<figcaption>
</figure>

**Các Thành phần Kiến trúc RAG:**

1. **Nhập Tài liệu**: Tải lên và chia nhỏ cơ sở kiến thức
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

---

#### 3. Kiến trúc Amazon Bedrock AgentCore

##### Các Thành phần Nền tảng Agentic Toàn diện

![Amazon Bedrock AgentCore - Kiến trúc Thành phần](/images/Event-Participated/pic17.jpg)

<figure>
    <figcaption>Amazon Bedrock AgentCore - Nền tảng agentic toàn diện hiển thị các thành phần cốt lõi: Runtime, Memory, Identity, Gateway, Code Interpreter, Browser Tool, và Observability cho triển khai tác nhân sản xuất sẵn sàng<figcaption>
</figure>

**Các Thành phần Kiến trúc Chính:**

- **Runtime**: Môi trường thực thi cốt lõi cho các hoạt động của tác nhân
- **Memory**: Quản lý trạng thái và bối cảnh cho tác nhân
- **Identity**: Khung xác thực và ủy quyền
- **Gateway**: Cổng API cho giao tiếp tác nhân
- **Code Interpreter**: Thực thi và xác nhận mã được tạo
- **Browser Tool**: Khả năng tương tác web và truy xuất dữ liệu
- **Observability**: Theo dõi, ghi nhật ký và tracing cho tác nhân

---

#### 4. Dịch vụ AgentCore & Mở rộng Quy mô

##### Dịch vụ Cho phép Tác nhân Mở rộng Quy mô

![Dịch vụ AgentCore - Kiến trúc Mở rộng Quy mô](/images/Event-Participated/pic18.jpg)

<figure>
    <figcaption>Kiến trúc dịch vụ AgentCore hiển thị cách các tác nhân mở rộng với Framework, Agent Instruction, Agent local tools, Agent context, AgentCore Gateway, Browser, Code Interpreter, Identity, Memory, và Observability cùng hoạt động<figcaption>
</figure>

**Kiến trúc Tác nhân Có thể Mở rộng:**

**Lớp Khách hàng:**
- Nhiều kết nối khách hàng
- Định tuyến yêu cầu và cân bằng tải

**Lớp Thời gian chạy Tác nhân:**
- Framework: Công cụ thực thi tác nhân cốt lõi
- Agent Instruction: Định nghĩa nhiệm vụ và quy trình công việc
- Agent local tools: Công cụ tích hợp sẵn và tùy chỉnh
- Agent context: Quản lý trạng thái và bộ nhớ

**Dịch vụ AgentCore:**
- Gateway: Xử lý và định tuyến yêu cầu
- Browser Tool: Tương tác web và quét dữ liệu
- Code Interpreter: Thực thi mã an toàn
- Identity: Quyền và kiểm soát truy cập
- Memory: Quản lý trạng thái liên tục
- Observability: Tracing đầy đủ và giám sát

---

#### 5. Từ Nguyên mẫu đến Sản xuất

##### Thách thức Sản xuất & Giải pháp

![Nguyên mẫu đến Sản xuất - Khoảng cách](/images/Event-Participated/pic19.jpg)

<figure>
    <figcaption>Khoảng cách từ nguyên mẫu đến sản xuất minh họa các thách thức khi chuyển từ POC (Proof of Concept) sang tác nhân AI sản xuất, bao gồm Performance, Scalability, Security, Governance, và đạt được giá trị kinh doanh có ý nghĩa<figcaption>
</figure>

**Danh sách kiểm tra Sẵn sàng Sản xuất:**

**Hiệu suất:**
- Tối ưu hóa độ trễ
- Yêu cầu thông lượng
- Hiệu quả tài nguyên

**Khả năng mở rộng:**
- Xử lý tác nhân đồng thời
- Phân phối tải
- Khả năng tự động mở rộng

**Bảo mật:**
- Kiểm soát truy cập và xác thực
- Mã hóa dữ liệu và bảo vệ
- Ghi nhật ký kiểm toán và tuân thủ

**Quản trị:**
- Khung quản trị mô hình
- Truy xuất quyết định
- Tuân thủ quy định

**Giá trị Kinh doanh:**
- Đo lường ROI
- Xác nhận trường hợp sử dụng
- Tích hợp với quy trình kinh doanh

---

#### 6. API RetrieveAndGenerate

##### Cơ sở Kiến thức cho Amazon Bedrock

![API RetrieveAndGenerate - Triển khai Cơ sở Kiến thức](/images/Event-Participated/pic20.jpg)

<figure>
    <figcaption>API RetrieveAndGenerate cho Cơ sở Kiến thức hiển thị luồng hoàn chỉnh: nhập người dùng, tạo truy vấn với embeddings, truy xuất tài liệu, tăng cường bối cảnh, và tạo phản ứng từ LLM<figcaption>
</figure>

**Luồng Triển khai RAG:**

**Bước 1: Chuẩn bị Truy vấn**
- Xử lý nhập người dùng
- Tạo embedding truy vấn từ nhập người dùng

**Bước 2: Truy xuất Cơ sở Kiến thức**
- Truy xuất tài liệu tương tự từ các cơ sở kiến thức
- Khớp ngữ nghĩa bằng embeddings

**Bước 3: Tăng cường Bối cảnh**
- Tăng cường truy vấn người dùng với tài liệu được truy xuất
- Tạo prompt nâng cao với bối cảnh liên quan

**Bước 4: Tạo Phản ứng**
- Tạo phản ứng từ LLM sử dụng bối cảnh nâng cao
- Trả lại câu trả lời với trích dẫn thích hợp

**Lợi ích:**
- Phản ứng có căn cứ với quy khích nguồn
- Thông tin hiện tại mà không cần huấn luyện lại
- Giảm các phản ứng vô căn cứ
- Cập nhật kiến thức tiết kiệm chi phí

---

#### 7. Các Khung xây dựng Tác nhân

##### Khung Tác nhân Mã nguồn Mở

![Khung Xây dựng Tác nhân - Tổng quan Hệ sinh thái](/images/Event-Participated/pic21.jpg)

<figure>
    <figcaption>Tổng quan toàn diện về các khung xây dựng và SDK mã nguồn mở cho tác nhân bao gồm Strands, LangGraph, LangChain, Crew.ai, Google ADK, LlamaIndex, và hơn thế nữa<figcaption>
</figure>

**Các Khung Tác nhân Phổ biến:**

**Strands SDK**
- Khung điều phối tác nhân
- Hỗ trợ quy trình công việc đa bước

**LangGraph & LangChain**
- Chaining và điều phối mô hình ngôn ngữ
- Kiến trúc thành phần mô đun
- Hệ sinh thái tích hợp mở rộng

**Crew.ai**
- Khung hợp tác đa tác nhân
- Gán tác nhân dựa trên vai trò
- Quản lý phụ thuộc nhiệm vụ

**Google ADK (Agent Development Kit)**
- Khung tác nhân gốc Google Cloud
- Tích hợp Gemini

**LlamaIndex**
- Khung lập chỉ mục và truy xuất dữ liệu
- Tối ưu hóa quy trình RAG
- Quản lý cơ sở kiến thức

**OpenAI SDKs**
- Khung tác nhân OpenAI chính thức
- Gọi hàm và sử dụng công cụ

---

#### 8. Hệ sinh thái Dịch vụ AI của AWS

##### Bộ Dịch vụ AI/ML Toàn diện

![Dịch vụ AI AWS - Danh mục Dịch vụ Toàn diện](/images/Event-Participated/pic22.jpg)

<figure>
    <figcaption>Hệ sinh thái dịch vụ AI AWS bao gồm Rekognition (thị giác), Translate (ngôn ngữ), Textract (tài liệu), Transcribe (giọng nói sang văn bản), Polly (văn bản sang giọng nói), Comprehend (NLP), Kendra (tìm kiếm), Lookout (bất thường), và Personalize (đề xuất)<figcaption>
</figure>

**Các Dịch vụ AI/ML Cốt lõi:**

**Dịch vụ Thị giác:**
- **Amazon Rekognition**: Phân tích hình ảnh và video, phát hiện đối tượng, nhận dạng khuôn mặt

**Dịch vụ Ngôn ngữ:**
- **Amazon Translate**: Dịch máy thần kinh
- **Amazon Transcribe**: Nhận dạng giọng nói tự động
- **Amazon Comprehend**: Xử lý ngôn ngữ tự nhiên và phân tích tư cảm
- **Amazon Polly**: Tổng hợp văn bản thành giọng nói

**Dữ liệu & Kiến thức:**
- **Amazon Textract**: Trích xuất văn bản và dữ liệu từ tài liệu
- **Amazon Kendra**: Tìm kiếm và truy xuất tài liệu thông minh

**Dịch vụ Trí tuệ:**
- **Amazon Lookout**: Phát hiện bất thường trong chỉ số và dữ liệu chuỗi thời gian
- **Amazon Personalize**: Công cụ cá nhân hóa thời gian thực
- **Amazon Forecast**: Dự báo chuỗi thời gian

---

#### 9. Các Bài thuyết trình Diễn giả & Chuyên sâu Kỹ thuật

##### Bài thuyết trình Khai mạc Amazon Bedrock AgentCore

![Amazon Bedrock AgentCore Khai mạc - Danh Hoang Hieu Nghi](/images/Event-Participated/pic13.jpg)

<figure>
    <figcaption>Bài thuyết trình chính về Amazon Bedrock AgentCore được trình bày bởi Danh Hoang Hieu Nghi (Kỹ sư AI, Renova Cloud), giới thiệu các khái niệm cốt lõi về kiến trúc tác nhân thông minh và điều phối quy trình công việc đa bước<figcaption>
</figure>

**Các Chủ đề Bài thuyết trình:**
- Khả năng và tầm nhìn của Amazon Bedrock
- Các nguyên tắc cơ bản kiến trúc AgentCore
- Các trường hợp sử dụng thực tế cho tác nhân thông minh
- Bắt đầu với Bedrock Agents

---

##### Tổng quan Dịch vụ AI AWS

![Trình bày Dịch vụ AI AWS - Các Dịch vụ AI Được Đào tạo Trước Khác](/images/Event-Participated/pic14.jpg)

<figure>
    <figcaption>Bài thuyết trình kỹ thuật về Các Dịch vụ AI Được Đào tạo Trước bao gồm Rekognition, Polly, Transcribe, và Comprehend, trình bày hệ sinh thái dịch vụ AI/ML toàn diện của AWS cho các ứng dụng thực tế<figcaption>
</figure>

**Các Dịch vụ Bao phủ:**
- Amazon Rekognition cho thị giác máy tính
- Amazon Polly cho tổng hợp giọng nói
- Amazon Transcribe cho chuyển đổi giọng nói sang văn bản
- Amazon Comprehend cho NLP và phân tích tư cảm
- Các bản trình diễn trường hợp sử dụng thực tế

---

##### Generative AI với Amazon Bedrock - Nhóm Diễn giả Đa phương

![Nhóm Diễn giả Đa phương - Generative AI với Amazon Bedrock](/images/Event-Participated/pic15.jpg)

<figure>
    <figcaption>Thảo luận nhóm diễn giả đa phương về Trí tuệ Nhân tạo Sinh tạo với Amazon Bedrock có đại diện Lam Tuan Kiet (Kỹ sư DevOps Cấp cao FPT Software), Dinh Le Hoang Anh (Trainee Cloud Engineer), và các diễn giả khác thảo luận về các triển khai thực tế và các thực hành tốt nhất<figcaption>
</figure>

**Các Chủ đề Thảo luận Nhóm:**
- Lựa chọn mô hình nền tảng và so sánh
- Các thực hành tốt nhất prompt engineering
- Các chiến lược triển khai RAG
- Các cân nhắc triển khai sản xuất
- Tối ưu hóa chi phí cho khối lượng công việc AI

---

##### Ảnh Nhóm Hội thảo

![Ảnh Nhóm Hội thảo - Sự kiện AWS Cloud Mastery](/images/Event-Participated/pic16.jpg)

<figure>
    <figcaption>Ảnh nhóm những người tham dự hội thảo và diễn giả trước màn hình hiển thị tương tác Kahoot, đại diện cho môi trường học tập hợp tác và hoàn thành thành công sự kiện AWS Cloud Mastery Series #1<figcaption>
</figure>

---

#### 10. Sự Tham gia Hội thảo & Người tham dự

##### Các Phiên Hội thảo & Học tập Tương tác

![Phiên Hội thảo Kỹ thuật](/images/Event-Participated/pic10.jpg)

<figure>
    <figcaption>Bài thuyết trình hội thảo về các kỹ thuật AI/ML nâng cao và tích hợp dịch vụ AWS<figcaption>
</figure>

![Trình diễn Kỹ thuật Trực tiếp](/images/Event-Participated/pic11.jpg)

<figure>
    <figcaption>Những người tham dự tập trung vào các bài thuyết trình kỹ thuật và học tập về các dịch vụ AI của AWS<figcaption>
</figure>

![Cộng đồng Hội thảo & Chia sẻ Kiến thức](/images/Event-Participated/pic12.jpg)

<figure>
    <figcaption>Những người tham dự hội thảo tham gia thảo luận và trao đổi kiến thức trong sự kiện AWS Cloud Mastery<figcaption>
</figure>

---

### Phiên Hỏi & Đáp: Các Thách thức Kỹ thuật & Giải pháp

#### Q1: Quản lý Tin nhắn Lớn Mà Không Sử dụng SQS

**Phát biểu Vấn đề:**
Xử lý tin nhắn khối lượng lớn và tải trọng lớn mà không sử dụng SQS.

**Các Giải pháp Được thảo luận:**

1. **Gọi Trực tiếp với Giới hạn Đồng thời**
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

**Phát biểu Vấn đề:**
Ràng buộc API hoặc mô hình giới hạn chatbot ở 1-2 yêu cầu trên phút, cần các giải pháp để xử lý nhu cầu người dùng cao hơn.

**Các Giải pháp Được thảo luận:**

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
- Các kỹ thuật prompt engineering nâng cao (CoT, Few-shot, Zero-shot)
- Các mẫu triển khai RAG để tích hợp kiến thức
- Kiến trúc Bedrock Agents và tự động hóa quy trình công việc đa bước
- Các thành phần AgentCore và chiến lược khả năng mở rộng
- Hệ sinh thái khung xây dựng tác nhân
- Hệ sinh thái dịch vụ AI AWS và các trường hợp sử dụng thực tế

**Giải pháp Thực tế:**

- Các mẫu giới hạn tốc độ và xử lý tin nhắn hiệu quả cho ứng dụng AI
- Chiến lược bộ nhớ đệm để tối ưu hóa việc sử dụng API
- Kiến trúc dựa trên hàng đợi để xử lý các nút thắt
- Danh sách kiểm tra sẵn sàng sản xuất cho tác nhân AI
- Các ví dụ thực tế về tích hợp dịch vụ AI

**Các Mẫu Kiến trúc Học được:**

- Kiến trúc chatbot serverless
- Thiết kế quy trình RAG và triển khai
- Quy trình công việc tự động hóa dựa trên tác nhân
- Triển khai AI an toàn và tuân thủ với guardrails
- Các hệ thống đa tác nhân có thể mở rộng

### Suy ngẫm Cá nhân

Hội thảo này nâng cao đáng kể sự hiểu biết của tôi về xây dựng các ứng dụng trí tuệ nhân tạo sinh tạo sản xuất sẵn sàng trên AWS. Sự kết hợp giữa kiến thức về mô hình nền tảng, các kỹ thuật prompt nâng cao, các sơ đồ kiến trúc toàn diện, và các giải pháp thực tế cung cấp một nền tảng vững chắc để phát triển các tác nhân thông minh và các ứng dụng được cấp năng lượng bởi AI quy mô lớn.

Hội thảo bao gồm cả khái niệm lý thuyết và triển khai thực tế, với các ví dụ thực tế cho thấy cách khắc phục những thách thức phổ biến trong triển khai AI. Định dạng đa diễn giả cho phép có những quan điểm đa dạng về các thực hành tốt nhất và các mẫu mới nổi trong phát triển AI.

Các lĩnh vực chính để khám phá thêm:

- RAG nâng cao với tìm kiếm kết hợp (ngữ nghĩa + từ khóa)
- Tinh chỉnh các mô hình nền tảng cho các miền chuyên biệt
- Các mẫu hợp tác hệ thống đa tác nhân và tác nhân
- MLOps và giám sát cho các ứng dụng GenAI
- Tối ưu hóa chi phí cho khối lượng công việc AI cao
- Tích hợp nhiều dịch vụ AI AWS trong các quy trình công việc phức tạp

---

_P/S: Hội thảo này cung cấp những hiểu biết có thể thực hiện được vào việc xây dựng các ứng dụng AI có khả năng mở rộng, sản xuất sẵn sàng. Các cuộc thảo luận kỹ thuật về giới hạn tốc độ, xử lý tin nhắn, và kiến trúc tác nhân đặc biệt có giá trị khi giải quyết các ràng buộc thực tế trong triển khai các chatbot AI và tác nhân thông minh._
