---
title: "Nhật ký tuần 3"
date: 2025-09-22
weight: 3
chapter: false
pre: " <b> 1.3 </b> "
---

### Mục tiêu tuần 3:

### Nhiệm vụ cần thực hiện trong tuần này:

| Thứ | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| 2 | - Học và thực hành Module 57:<br>&emsp; + S3 là gì (định nghĩa, tính chất)<br>&emsp; + Phân biệt giữa S3 Bucket và Objects<br>&emsp; + Các loại lưu trữ của S3 hiện có<br>&emsp; + Bảo mật và hiệu năng của S3<br>&emsp; + Thực hành: tạo S3 và config static web | 22/09/2025 | 22/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 3 | - Kết nối S3 với CloudFront để tối ưu tốc độ đường truyền<br>- Học về S3 Versioning (Định nghĩa, Lợi ích, Cách hoạt động)<br>- Move Object ở S3<br>- Replicate object multi region<br>- Học về module 4:<br>&emsp; + Amazon Access Point<br>&emsp; + Chi tiết hơn về S3<br>&emsp; + Hỗ trợ CORS cho S3<br>&emsp; + S3 Endpoint và S3 Versioning<br>&emsp; + Disaster Recovery <br>&emsp; + Snow family (snow ball, snow edge, snow mobile)<br>&emsp; + Object lock<br>&emsp; + Các tuỳ chọn truy xuất dữ liệu (expedited, standard, bulk)<br>&emsp; + Các phương thức lưu trữ chính của hybrid storage gateway (filegw, volumegw, tapegw)<br>&emsp; + Thực hành full module nhỏ trong lab14 | 23/09/2025 | 23/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 4 | - Học về AWS RDS<br>- Học và nắm vững các bước thực hành module 05<br>- Ghi nhớ kiến thức về AWS RDS | 24/09/2025 | 24/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 5 | - Học về VMWare export, cách import ổ đĩa từ môi trường local lên EC2 và export ngược lại<br>- Import máy ảo vào AWS<br>- Export máy ảo từ on-premises<br>- Upload máy ảo lên AWS<br>- Import máy ảo vào AWS<br>- Triển khai máy ảo từ AMI  | 25/09/2025 | 25/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 6 | - Làm cách nào để Storage Gateway<br>- Làm cách nào để tạo File Share<br>- Cách gắn File Sharing trên máy cục bộ | 26/09/2025 | 26/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 7 | - Ôn tập các lý thuyết đã học | 27/09/2025 | 27/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| CN | - Ôn tập các lý thuyết đã học | 28/09/2025 | 28/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |

### Kết quả đạt được tuần 3:

#### **Amazon S3 (Simple Storage Service)**

- **Kiến thức cốt lõi**:
  - Hiểu định nghĩa, đặc điểm của S3 và sự khác biệt giữa Bucket và Object.
  - Tìm hiểu các loại lưu trữ S3 (Storage Classes) và trường hợp sử dụng.
  - Học về Bảo mật và Hiệu năng của S3.
- **Tính năng nâng cao**:
  - **Static Web Hosting**: Đã tạo S3 bucket và cấu hình thành trang web tĩnh.
  - **Tích hợp CloudFront**: Kết nối S3 với CloudFront để tối ưu tốc độ truyền tải.
  - **Versioning**: Bật tính năng S3 Versioning để quản lý các phiên bản đối tượng.
  - **Replication**: Cấu hình Cross-Region Replication (CRR) để phục hồi sau thảm họa.
  - **Object Lock**: Triển khai mô hình WORM (Write Once Read Many).
  - **Access Points & CORS**: Cấu hình điểm truy cập và chia sẻ tài nguyên chéo nguồn (CORS).

#### **Lưu trữ & Truyền tải dữ liệu**

- **AWS Snow Family**:
  - Tìm hiểu về Snowball, Snow Edge và Snowmobile cho di chuyển dữ liệu quy mô lớn.
- **Storage Gateway**:
  - Hiểu các loại Hybrid Storage Gateway: File Gateway, Volume Gateway, và Tape Gateway.
  - Thực hành tạo File Share và mount lên máy cục bộ.
- **Truy xuất dữ liệu**:
  - Tìm hiểu các tùy chọn truy xuất: Expedited, Standard, và Bulk.

#### **AWS RDS (Relational Database Service)**

- **Quản lý cơ sở dữ liệu**:
  - Nắm kiến thức về AWS RDS và khả năng quản lý của nó.
  - Thực hành triển khai và quản lý RDS instance (Module 05).

#### **Di chuyển & Quản lý máy ảo (VM)**

- **Import/Export VM**:
  - Học cách export VMWare VM và import ổ đĩa từ local lên EC2.
  - Thực hành export VM từ on-premises và upload/import vào AWS.
- **Triển khai AMI**:
  - Triển khai thành công máy ảo từ Amazon Machine Image (AMI).

#### **Kỹ năng thực hành đạt được**

- Cấu hình S3 cho static web và tích hợp CloudFront.
- Quản lý vòng đời, phiên bản và bảo mật S3.
- Triển khai và quản lý RDS instance.
- Thực hiện di chuyển VM giữa môi trường local và AWS.
- Thiết lập Hybrid Storage Gateway để chia sẻ file.
