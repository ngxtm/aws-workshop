---
title: "Proposal"
date: 2025-10-03
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# MAPVIBE

**Nền tảng Khám phá Vị trí Bản đồ Powered by AI**  
_(Khám phá các địa điểm ăn uống và các địa điểm khác tại Thành phố Hồ Chí Minh sử dụng các gợi ý ngôn ngữ tự nhiên và thông tin chi tiết theo ngữ cảnh)_

---

## 1. Tóm tắt Điều hành

MapVibe là một nền tảng web driven by AI được ra mắt tại **Thành phố Hồ Chí Minh** để biến đổi việc khám phá địa điểm, cho phép người dùng tìm kiếm địa điểm thông qua các gợi ý ngôn ngữ tự nhiên (ví dụ: "tìm nhà hàng sang trọng trên tầng thượng có view thành phố mở cửa đến nửa đêm" hoặc "quán cà phê yên tĩnh gần sông có chỗ ngồi ngoài trời"). Nền tảng sử dụng **Mô hình Ngôn ngữ Lớn (LLMs) của Amazon Bedrock** để diễn giải ý định của người dùng, tích hợp các yếu tố ngữ cảnh theo thời gian thực như vị trí, thời gian và sở thích, và truy xuất dữ liệu từ cơ sở dữ liệu nội bộ **DynamoDB**. Xây dựng trên **kiến trúc AWS serverless**, MapVibe cung cấp **độ trễ thấp (<10s)**, **độ chính xác cao (≥85% sự hài lòng tương ứng)**, và **hiệu quả chi phí (<$200 cho chu kỳ phát triển và demo 8 tuần ban đầu, hoàn thành vào ngày 22 tháng 10 năm 2025)**. Người dùng đã xác thực tận hưởng các đề xuất được cá nhân hóa, khả năng đóng góp đánh giá, và truy cập các công cụ kiểm duyệt, tất cả được nâng cao bởi công nghệ AI.

---

## 2. Phát biểu Vấn đề

### Vấn đề là gì?

- Các nền tảng bản đồ truyền thống như Google Maps phụ thuộc vào tìm kiếm dựa trên từ khóa và bộ lọc tĩnh, gặp khó khăn trong việc diễn giải các truy vấn tinh tế, giàu ngữ cảnh (ví dụ: "quán cà phê yên tĩnh gần sông có chỗ ngồi ngoài trời").
- Người dùng lãng phí thời gian điều hướng nhiều ứng dụng để tìm địa điểm ăn uống hoặc hoạt động phù hợp.
- Các giải pháp hiện tại thiếu giao diện trò chuyện và không kết hợp được các tín hiệu ngữ cảnh như thời gian, tâm trạng hoặc kích thước nhóm.

### Giải pháp

MapVibe sử dụng **AWS Bedrock LLMs** để phân tích các gợi ý ngôn ngữ tự nhiên bằng tiếng Việt và tiếng Anh, chuyển đổi chúng thành các truy vấn có cấu trúc. Nó truy xuất và xếp hạng kết quả từ cơ sở dữ liệu nội bộ **DynamoDB** với dữ liệu địa điểm được lập chỉ mục địa lý, cung cấp giao diện kết hợp (tìm kiếm trò chuyện + bộ lọc danh mục). Nội dung do người dùng tạo (đánh giá, gợi ý địa điểm) được kiểm duyệt sử dụng **AWS Rekognition**, đảm bảo an toàn và chất lượng thông qua phân tích driven by AI tiên tiến.

### Lợi ích và ROI

- **Tốc độ**: Giảm thời gian khám phá địa điểm từ vài phút xuống vài giây.
- **Cá nhân hóa**: Kết quả nhận biết ngữ cảnh dựa trên sở thích và hành vi của người dùng, powered by AI.
- **Tự động hóa**: Loại bỏ bộ lọc thủ công với phân tích ý định driven by AI.
- **Khả năng mở rộng**: Cơ sở hạ tầng AWS toàn cầu đảm bảo độ trễ thấp và khả năng phục hồi.
- **Hiệu quả chi phí**: Tối ưu hóa để phù hợp với ngân sách $200 cho chu kỳ 8 tuần ban đầu, hoàn thành vào ngày 22 tháng 10 năm 2025.
- **Tiềm năng thương mại**: Cơ hội hợp tác với các doanh nghiệp địa phương hoặc tích hợp với các hệ thống đặt phòng nội bộ.

---

## 3. Kiến trúc Giải pháp

### Tổng quan

Gợi ý Người dùng + Ngữ cảnh → **Phân tích Ý định Bedrock LLM** → **Truy vấn có Cấu trúc** → **Tìm kiếm DynamoDB** → **Xếp hạng & Bộ nhớ đệm** → **Hiển thị Web UI** → **Vòng lặp Phản hồi Người dùng**.  
![Solution Architecture](https://4n1d.github.io/aws-workshop/images/architecture.png)

### Dịch vụ AWS được sử dụng

| Service                 | Function                                                        |
| ----------------------- | --------------------------------------------------------------- |
| Amazon Route 53         | Định tuyến tên miền                                             |
| AWS Certificate Manager | Chứng chỉ SSL/TLS                                               |
| AWS WAF                 | Tường lửa ứng dụng web                                          |
| Amazon CloudFront       | CDN toàn cầu cho tài sản tĩnh                                   |
| Amazon API Gateway      | Điểm cuối API RESTful an toàn                                   |
| AWS Lambda              | Logic phân tích ý định, tìm kiếm và xếp hạng                    |
| Amazon DynamoDB         | Dữ liệu địa điểm được lập chỉ mục địa lý và bộ nhớ đệm truy vấn |
| Amazon S3               | Lưu trữ ảnh, nhật ký và tài sản                                 |
| Amazon Cognito          | Xác thực và ủy quyền người dùng                                 |
| Amazon Bedrock          | LLM để phân tích ý định và tóm tắt                              |
| Amazon Rekognition      | Kiểm duyệt nội dung driven by AI cho các tải lên của người dùng |
| Amazon EventBridge      | Phân tích theo lịch và cập nhật huy hiệu                        |
| Amazon CloudWatch       | Giám sát và ghi nhật ký                                         |

### Thiết kế Thành phần

- **Frontend**: Ứng dụng web responsive (Next.js, song ngữ VI/EN, UI tìm kiếm kết hợp).
- **Tiếp nhận Dữ liệu**: Gợi ý và ngữ cảnh được xử lý qua API Gateway; các tải lên của người dùng (đánh giá, ảnh) được kiểm duyệt bởi AI của Rekognition.
- **Lưu trữ Dữ liệu**: DynamoDB cho dữ liệu địa điểm và truy vấn được bộ nhớ đệm (TTL 24 giờ); S3 cho ảnh và nhật ký.
- **Xử lý Dữ liệu**: Microservices Lambda xử lý các cuộc gọi Bedrock LLM, thực thi truy vấn và xếp hạng kết quả.
- **Quản lý Người dùng**: Cognito cho xác thực dựa trên JWT (đăng nhập email/mạng xã hội); người dùng khách truy cập các tính năng hạn chế.
- **Đầu ra**: Hiển thị thẻ địa điểm với tóm tắt được tạo bởi AI, đánh giá, ảnh và CTA (ví dụ: Chỉ đường, Gọi).

---

## 4. Triển khai Kỹ thuật

### Các giai đoạn Triển khai

| Phase | Description                                                 | Duration                                           |
| ----- | ----------------------------------------------------------- | -------------------------------------------------- |
| 1     | Xác định kiến trúc, schema gợi ý Bedrock và schema DynamoDB | Đã hoàn thành (2 tuần)                             |
| 2     | Ước tính chi phí và tối ưu hóa chiến lược bộ nhớ đệm        | Đã hoàn thành (1 tuần)                             |
| 3     | Xây dựng backend (Lambda, DynamoDB, Bedrock, Rekognition)   | Đã hoàn thành (3 tuần)                             |
| 4     | Phát triển frontend (Next.js, song ngữ, UI responsive)      | Đã hoàn thành (3 tuần)                             |
| 5     | Kiểm tra và tối ưu hóa cho độ trễ <10s và khả năng mở rộng  | Đã hoàn thành (2 tuần)                             |
| 6     | Ra mắt MVP, triển khai qua CI/CD, thu thập phản hồi         | Đang tiến hành (bắt đầu ngày 22 tháng 10 năm 2025) |

### Yêu cầu Kỹ thuật

- **Thiết bị Edge**: Trình duyệt hiện đại (Chrome, Safari, Firefox) với UI responsive sẵn sàng PWA.
- **Cloud**: AWS Route 53, ACM, WAF, CloudFront, API Gateway, Lambda, DynamoDB, S3, Cognito, Bedrock, Rekognition, EventBridge, CloudWatch.
- **Công cụ & Framework**: Next.js (App Router), TypeScript, AWS CDK cho infrastructure-as-code, GitHub Actions cho CI/CD.

---

## 5. Lịch trình & Cột mốc

| Period                                    | Activities                                                                    |
| ----------------------------------------- | ----------------------------------------------------------------------------- |
| Trước khi Phát triển (Tháng 0)            | Nghiên cứu các bộ dữ liệu địa điểm tại Thành phố Hồ Chí Minh cho DynamoDB     |
| Tháng 1 (Tháng 9 năm 2025)                | Xây dựng backend MVP với Bedrock LLM và DynamoDB                              |
| Tháng 2 (Tháng 10 năm 2025)               | Triển khai bộ nhớ đệm, phát triển tích hợp frontend                           |
| Tháng 3 (Tháng 10 năm 2025)               | Ra mắt beta công khai, tối ưu hóa hiệu suất, thu thập phản hồi                |
| Sau khi Ra mắt (Tháng 10 năm 2025 trở đi) | Thêm các tính năng nâng cao (ví dụ: xếp hạng dựa trên ML, chế độ ngoại tuyến) |

---

## 6. Ước tính Ngân sách

### Chi phí Cơ sở hạ tầng Đám mây

| AWS Service          | Cost/Month (USD) | Description            |
| -------------------- | ---------------- | ---------------------- |
| Lambda               | 15               | API + LLM logic        |
| DynamoDB             | 10               | Cached query store     |
| S3                   | 5                | Logs, static files     |
| API Gateway          | 10               | Request routing        |
| Cognito              | 5                | Auth MAU               |
| CloudFront           | 10               | Hosting/CDN            |
| Bedrock (LLM tokens) | 15               | Prompt parsing         |
| Rekognition          | 5                | Batch image moderation |
| CloudWatch           | 5                | Error-only logging     |
| **Total**            | **≈ 80/tháng**   | **≈ 160/8 tuần**       |

### Biện pháp Tối ưu hóa Chi phí

- **Sử dụng Free-Tier**: Tận dụng các miễn phí của AWS cho Lambda, DynamoDB, S3, CloudFront, Rekognition và Cognito để giảm thiểu chi phí.
- **Bộ nhớ đệm Aggressive cho Bedrock**: Đạt tỷ lệ truy cập bộ nhớ đệm 95% để giảm chi phí token AI từ $120 xuống <$15/tháng.
- **Xử lý Batch Rekognition**: Các kiểm tra hình ảnh không theo thời gian thực tiết kiệm ~$80 trong 8 tuần.
- **Kiểm tra Tải được Đơn giản hóa**: Kịch bản 100 người dùng × 10 phút thay vì 300 × 30 phút giảm chi phí tính toán.
- **Giảm ghi nhật ký CloudWatch**: Chỉ ghi nhật ký lỗi tiết kiệm $50+ trong 8 tuần.
- **Không có Đồng thời được Cung cấp**: Tránh chi phí Lambda nhàn rỗi.
- **Biến môi trường**: Sử dụng thay vì Secrets Manager để loại bỏ phí lưu trữ bí mật.
- **Chế độ On-Demand DynamoDB**: Tất cả đọc/ghi miễn phí theo cấp.
- **Vô hiệu hóa Origin Shield**: Tiết kiệm chi phí CloudFront.
- **Bộ nhớ đệm Tài sản Tĩnh**: Tối thiểu hóa chi phí truyền dữ liệu đi ra.

### Kịch bản Ngân sách Đề xuất

Để đảm bảo nền tảng MapVibe hoạt động hiệu quả trong ngân sách AWS $200 trong chu kỳ phát triển và demo 8 tuần ban đầu (hoàn thành vào ngày 22 tháng 10 năm 2025), chúng tôi đề xuất các kịch bản sau dựa trên các mức độ tối ưu hóa và sử dụng tài nguyên khác nhau:

- **Kịch bản Tối thiểu**: Tập trung vào các tính năng thiết yếu với sự phụ thuộc tối đa vào các miễn phí. Điều này bao gồm vô hiệu hóa các dịch vụ không quan trọng như WAF nếu không cần thiết, giới hạn các cuộc gọi Bedrock chỉ cho các truy vấn được bộ nhớ đệm (nhắm đến tỷ lệ truy cập bộ nhớ đệm 98%+), và không thực hiện kiểm tra tải. Chi phí ước tính: <$50 trong 8 tuần. Phù hợp cho prototyping ban đầu nhưng có thể ảnh hưởng đến độ tin cậy của demo do các vấn đề về khả năng mở rộng chưa được kiểm tra.

- **Kịch bản Đề xuất**: Cân bằng chi phí và độ tin cậy bằng cách kết hợp tất cả các biện pháp tối ưu hóa chính được liệt kê ở trên. Kịch bản này sử dụng bộ nhớ đệm aggressive (tỷ lệ truy cập 95% cho Bedrock), xử lý batch cho Rekognition, kiểm tra tải đơn giản hóa (100 người dùng × 10 phút), và chỉ ghi nhật ký lỗi trong CloudWatch. Nó đảm bảo độ trễ thấp và khả năng phục hồi trong khi vẫn nằm dưới ngân sách. Chi phí ước tính: ~$100-150 trong 8 tuần. Lý tưởng cho demo MVP ra mắt vào ngày 22 tháng 10 năm 2025, cung cấp trải nghiệm mạnh mẽ mà không có chi phí không cần thiết.

- **Kịch bản Nâng cao**: Bao gồm các điều khoản bổ sung cho việc sử dụng cao hơn sau khi ra mắt, chẳng hạn như đồng thời được cung cấp cho Lambda trong giờ cao điểm và ghi nhật ký đầy đủ trong CloudWatch để gỡ lỗi chi tiết. Điều này làm tăng chi phí một chút nhưng nâng cao giám sát hiệu suất và kiểm tra khả năng mở rộng (ví dụ: tải 300 người dùng × 30 phút). Chi phí ước tính: ~$180-200 trong 8 tuần. Được đề xuất cho các hoạt động liên tục sau ngày 22 tháng 10 năm 2025, nếu dự kiến có các demo mở rộng hoặc lưu lượng truy cập cao hơn, vẫn nằm trong giới hạn ngân sách tổng thể.

Đề xuất: Kịch bản Đề xuất đã được triển khai thành công cho việc ra mắt MVP vào ngày 22 tháng 10 năm 2025, đảm bảo độ tin cậy demo tối ưu, khả năng mở rộng và kiểm soát chi phí trong ngân sách $200. Đối với các hoạt động liên tục, hãy xem xét chuyển sang Kịch bản Nâng cao khi cần thiết.

### Kiểm soát & Giám sát Chi phí

- Tạo cảnh báo thanh toán và bật AWS Cost Explorer.
- Gắn thẻ tất cả tài nguyên (Project=MapVibe, Environment=Dev).
- Xem xét hàng tuần:
  - Dữ liệu CloudFront > 50 GB/tuần
  - Tỷ lệ truy cập bộ nhớ đệm Bedrock < 90%
  - Các cuộc gọi Lambda > 100K/tuần
  - Tổng chi phí > $15/tuần

---

## 7. Đánh giá Rủi ro

| Risk                                            | Impact | Probability | Mitigation                                             |
| ----------------------------------------------- | ------ | ----------- | ------------------------------------------------------ |
| Dữ liệu DynamoDB không nhất quán                | High   | Medium      | Xác thực và sao lưu dữ liệu thường xuyên               |
| Phân tích LLM không chính xác (VN/EN)           | Medium | Low         | Mẫu gợi ý được xác định trước, xác thực                |
| Khả năng mở rộng dưới tải cao                   | Medium | Medium      | Tự động mở rộng serverless, bộ nhớ đệm                 |
| Mối quan tâm về quyền riêng tư (dữ liệu vị trí) | High   | Low         | Sự đồng ý rõ ràng của người dùng, các truy vấn ẩn danh |

**Kế hoạch dự phòng**: Sử dụng kết quả DynamoDB được bộ nhớ đệm hoặc dự phòng JSON cục bộ cho các demo. Triển khai giới hạn tốc độ dựa trên IP cho người dùng chưa xác thực.

---

## 8. Kết quả Dự kiến

### Cải tiến Kỹ thuật

- **Tìm kiếm Trò chuyện**: Hỗ trợ ngôn ngữ tự nhiên cho tiếng Việt và tiếng Anh với độ trễ <10s, powered by Bedrock LLMs.
- **Tóm tắt AI**: Tổng quan địa điểm được tạo bởi Bedrock, làm mới mỗi 7 ngày hoặc sau 10 đánh giá mới.
- **Khả năng mở rộng**: Kiến trúc serverless với phân phối CDN toàn cầu qua CloudFront.
- **Kiểm duyệt**: AI của Rekognition đảm bảo nội dung do người dùng tạo an toàn (đánh giá, ảnh).

### Giá trị Dài hạn

- **Cá nhân hóa**: Xếp hạng lại dựa trên ML và phân tích hành vi người dùng.
- **Hỗ trợ Ngoại tuyến**: PWA để liệt kê các địa điểm ngoại tuyến.
- **Khả năng mở rộng**: Tiềm năng tích hợp với các hệ thống đặt phòng nội bộ.
- **Mở rộng Ngữ cảnh**: Đề xuất dựa trên thời tiết, sự kiện hoặc xu hướng xã hội.

---

### Tài liệu đính kèm / Tham khảo

- [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=12f225883db76aa012f87014f9994e8bcb304430)
- [GitHub Repository](https://github.com/4N1D/aws-workshop)

## 9. QUAN TRỌNG: Đọc SRS để biết thêm về dự án của chúng tôi

- [Software Requirement Specification](https://docs.google.com/document/d/16fEXJeQfbxF26TqigBDaBX5p0O-zkRW-sUpK6K31Ugs/edit?usp=sharing)
- [Related Documents](https://drive.google.com/drive/folders/1LtBMw0791vFCO5fvBHdWg24JyUWVCEXF?usp=sharing)
