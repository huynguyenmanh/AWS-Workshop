---
title: "Proposal"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả cảnh báo này.
{{% /notice %}}

# Serverless Todo/Note Web Application on AWS
## Giải pháp Quản lý Công việc & Ghi chú Thông minh dựa trên Kiến trúc Serverless AWS

---

### 1. Tóm tắt Dự án (Executive Summary)
Dự án **Serverless Todo/Note Web Application** được thiết kế nhằm cung cấp một nền tảng quản lý công việc và ghi chú cá nhân hiện đại, linh hoạt và bảo mật. Ứng dụng ứng dụng hoàn toàn kiến trúc **AWS Serverless** (không máy chủ) giúp hệ thống tự động mở rộng theo lưu lượng truy cập, tối ưu hóa chi phí vận hành ở mức gần như bằng 0 khi không sử dụng và loại bỏ hoàn toàn gánh nặng quản trị hạ tầng server truyền thống.

---

### 2. Đặt vấn đề & Giải pháp (Problem Statement)

#### Vấn đề hiện tại
* **Hạ tầng truyền thống cồng kềnh:** Các ứng dụng quản lý công việc triển khai trên máy chủ ảo (EC2/VPS) duy trì chi phí cố định hàng tháng dù lưu lượng truy cập thấp.
* **Rủi ro bảo mật & Cô lập dữ liệu:** Thiếu cơ chế phân quyền bảo mật chặt chẽ dẫn đến nguy cơ rò rỉ dữ liệu cá nhân giữa các người dùng.
* **Xử lý file đính kèm kém hiệu quả:** Việc tải ảnh/file đính kèm trực tiếp qua server backend gây nghẽn băng thông và tăng độ trễ hệ thống.

#### Giải pháp Đề xuất
Xây dựng ứng dụng Web dựa trên kiến trúc Serverless toàn diện trên AWS:
* **Frontend:** Host trên Amazon S3 và phân phối qua mạng CDN AWS CloudFront (HTTPS).
* **Xác thực:** Amazon Cognito quản lý Đăng ký/Đăng nhập và phát hành JWT Token bảo mật.
* **API & Compute:** Amazon API Gateway kết hợp JWT Authorizer điều hướng request đến các hàm AWS Lambda xử lý logic CRUD và cấp S3 Presigned URL.
* **Storage:** Amazon DynamoDB lưu trữ dữ liệu công việc (NoSQL) và Amazon S3 lưu trữ hình ảnh đính kèm.

#### Lợi ích & Tối ưu Chi phí (ROI)
* **Chi phí tối ưu:** Tận dụng tối đa gói AWS Free Tier (1 triệu request Lambda/tháng, 25 GB DynamoDB, 5 GB S3, 1 TB CloudFront), chi phí duy trì hàng tháng chỉ khoảng $0.00 – $0.50 USD.
* **Sẵn sàng cao & Tự động mở rộng:** Hệ thống tự động đáp ứng từ 1 đến hàng ngàn người dùng đồng thời mà không cần cấu hình Auto Scaling phức tạp.

---

### 3. Kiến trúc Giải pháp (Solution Architecture)

Ứng dụng chia làm 7 tầng dịch vụ chính theo đúng sơ đồ kiến trúc hệ thống:

![Serverless Todo/Note Architecture](../images/2-Proposal/architecture_diagram.png)

#### Các dịch vụ AWS sử dụng
1. **Client Layer:** Trình duyệt người dùng (User Browser).
2. **Frontend Hosting Layer:**
   * **Amazon S3 (Frontend Static Bucket):** Lưu trữ mã nguồn web tĩnh (HTML, CSS, JS).
   * **AWS CloudFront:** Mạng phân phối nội dung CDN toàn cầu tích hợp HTTPS.
3. **Authentication Layer:**
   * **Amazon Cognito:** Quản lý User Pools, xử lý Đăng ký/Đăng nhập và phát hành JWT Tokens.
4. **API Layer:**
   * **Amazon API Gateway:** Tiếp nhận REST HTTP request và xác thực thông qua JWT Authorizer.
5. **Backend Compute Layer:**
   * **AWS Lambda:** Thực thi logic xử lý CRUD dữ liệu và sinh Presigned URL cho S3 upload.
6. **Data Storage Layer:**
   * **Amazon DynamoDB:** Cơ sở dữ liệu NoSQL lưu trữ thông tin công việc (Primary Key: `userId`, Sort Key: `taskId`).
   * **Amazon S3 (Attachments Bucket):** Lưu trữ file/ảnh đính kèm do người dùng tải lên.
7. **Monitoring, Security & Cost Control Layer:**
   * **Amazon CloudWatch:** Ghi nhận Logs, theo dõi Metrics và cấu hình Alarms cảnh báo lỗi.
   * **AWS IAM:** Quản lý phân quyền truy cập theo nguyên tắc Quyền tối thiểu (Least-privilege roles).
   * **AWS Budgets:** Thiết lập hạn mức cảnh báo chi phí tài nguyên (Billing alerts).

---

### 4. Yêu cầu & Bối cảnh Kỹ thuật (Technical Implementation)

#### Các tính năng chính (8 Epics User Stories)
1. **Authentication:** Đăng ký, Đăng nhập, Đăng xuất, Đổi mật khẩu, Cập nhật Profile, Xóa tài khoản.
2. **Task & Note Management:** Tạo/Sửa/Xóa Task, đính kèm ảnh (S3 Presigned URL), Phân loại Category/Tag, Auto-save.
3. **Time Organization:** Đặt ngày bắt đầu/kết thúc, xem công việc theo Ngày/Tuần/Tháng/Khoảng thời gian.
4. **Search & Custom Filters:** Tìm kiếm theo tên, lọc theo Trạng thái/Tag/Danh mục, lưu bộ lọc tùy chỉnh.
5. **Multiple Views:** Hiển thị dạng Danh sách (List), Bảng Kanban, Dòng thời gian (Timeline) và Lịch (Calendar).
6. **Custom Workflow:** Tùy chỉnh cột trạng thái, màu sắc, kéo-thả thẻ (Drag-and-Drop) trên Kanban Board.
7. **Import / Export:** Trích xuất và nhập dữ liệu công việc từ file JSON/CSV hoặc nội dung Copy/Paste.
8. **Statistics:** Thống kê tổng số công việc, phân bổ theo Danh mục, Trạng thái, Tag và biểu đồ thời gian.

---

### 5. Lộ trình Triển khai (Timeline & Milestones)

Dự án được thực hiện trong **12 tuần thực tập** (từ **01/06/2026** đến **21/08/2026**):

* **Tháng 1 (Tuần 1 - Tuần 4):** * Học tập nền tảng AWS (Console, CLI, EC2, IAM, VPC, S3, DynamoDB NoSQL).
  * Thiết lập AWS Budgets cảnh báo chi phí.
* **Tháng 2 (Tuần 5 - Tuần 8):**
  * Nghiên cứu AWS Lambda, API Gateway, Amazon Cognito và S3 Security.
  * Lập CloudWatch Alarms và soạn thảo Proposal dự án gửi Mentor phê duyệt.
  * Viết và đăng 3 bài Tech Blog lên cộng đồng AWS Study Group.
* **Tháng 3 (Tuần 9 - Tuần 12):**
  * **Tuần 9:** Khởi tạo DynamoDB Tables, S3 Attachments Bucket và viết code Backend Lambda CRUD.
  * **Tuần 10:** Tích hợp API Gateway JWT Authorizer, kết nối Cognito Auth và Deploy Frontend lên S3 + CloudFront CDN.
  * **Tuần 11:** Kiểm thử End-to-End toàn bộ 8 Epics User Stories, rà soát IAM Roles và lập quy trình Clean-up.
  * **Tuần 12:** Viết tài liệu hướng dẫn kỹ thuật Step-by-Step (Workshop) và hoàn thiện Báo cáo Hugo song ngữ.

---

### 6. Dự toán Ngân sách (Budget Estimation)

Toàn bộ tài nguyên sử dụng nằm trong phạm vi **AWS Free Tier**:
* **AWS Lambda:** 1.000.000 requests/tháng miễn phí $\rightarrow$ **$0.00**
* **Amazon DynamoDB:** 25 GB dữ liệu lưu trữ & 25 WCU/RCU miễn phí $\rightarrow$ **$0.00**
* **Amazon S3:** 5 GB dung lượng Standard Storage miễn phí $\rightarrow$ **$0.00**
* **AWS CloudFront:** 1 TB dữ liệu truyền đi (Data Transfer Out)/tháng miễn phí $\rightarrow$ **$0.00**
* **Amazon Cognito:** 50.000 Monthly Active Users (MAUs) miễn phí $\rightarrow$ **$0.00**
* **Amazon API Gateway:** 1.000.000 HTTP API requests/tháng trong năm đầu $\rightarrow$ **$0.00**

👉 **Tổng chi phí dự kiến:** **$0.00 USD/tháng** (Tối đa không quá **$0.50 USD/tháng** nếu vượt Free Tier nhẹ).

---

### 7. Đánh giá & Quản trị Rủi ro (Risk Assessment)

| Rủi ro kỹ thuật | Mức độ | Biện pháp khắc phục & Phòng ngừa |
| :--- | :--- | :--- |
| **Lỗi CORS trên API Gateway** | Trung bình | Khởi tạo Header `Access-Control-Allow-Origin: '*'` trên cả API Gateway Responses và Lambda Code. |
| **Lỗi xác thực Token / Bị văng khỏi phiên** | Rõ ràng | Đảm bảo đồng bộ giữa Cognito ID Token và Authorization Header ở Frontend LocalStorage. |
| **Rò rỉ dữ liệu S3 Bucket** | Cao | Cấu hình S3 Block Public Access, bật KMS Encryption và chỉ cho phép upload qua Presigned URL thời hạn ngắn. |
| **Phát sinh chi phí ngoài ý muốn** | Thấp | Cấu hình AWS Budgets gửi email cảnh báo khi chi phí chạm mốc $1.00 USD. |

---

### 8. Kết quả Mong đợi (Expected Outcomes)
* **Sản phẩm hoàn thiện:** Một ứng dụng Web Serverless Todo/Note chạy thực tế trên CloudFront HTTPS với đầy đủ 8 Epics chức năng.
* **Tài liệu Workshop chuẩn hóa:** Bài hướng dẫn triển khai Step-by-Step chi tiết giúp cộng đồng có thể tự thực hành lại end-to-end.
* **Báo cáo chuyên nghiệp:** Website Báo cáo thực tập Hugo hỗ trợ song ngữ (VI/EN) đáp ứng 100% tiêu chí đánh giá mộc thực tập FCAJ.