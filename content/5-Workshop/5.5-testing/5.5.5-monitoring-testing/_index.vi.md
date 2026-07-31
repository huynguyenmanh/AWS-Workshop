---
title: "Kiểm thử Giám sát & Ghi log trên Amazon CloudWatch"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả cảnh báo này.
{{% /notice %}}

Trong phần này, chúng ta sẽ kiểm thử khả năng vận hành và giám sát hệ thống Serverless thông qua **Amazon CloudWatch**, bao gồm việc kiểm tra **CloudWatch Logs** (luồng ghi vết lỗi và lịch sử thực thi của Lambda) và **CloudWatch Metrics / Alarms** (biểu đồ hiệu năng và cảnh báo chi phí tài nguyên).

---

#### 1. Kiểm thử Luồng Ghi vết Log trên CloudWatch Logs

* **Các bước thực hiện:**
  1. Truy cập **Amazon CloudWatch Console** -> Chọn **Log groups** từ menu bên trái.
  2. Chọn Log Group tương ứng với dịch vụ Lambda Backend (ví dụ: `/aws/lambda/TodoCRUDHandler` hoặc `/aws/lambda/GetPresignedUrlHandler`).
  3. Chọn **Log Stream** gần nhất vừa sinh ra sau khi thực hiện thao tác tạo/sửa Task trên giao diện Web.

* **Kết quả kỳ vọng:**
  * CloudWatch ghi nhận đầy đủ luồng thực thi: sự kiện bắt đầu `START RequestId: ...`, các dòng log debug (`console.log`), thời gian thực thi `REPORT RequestId: ... Duration: ... ms Billed Duration: ... ms Memory Size: ... MB`.
  * Không phát sinh luồng exception hoặc `Task timed out` trong quá trình xử lý request.

![Kiểm tra Log Stream trên CloudWatch Logs](/images/5-Workshop/5.4-testing/cloudwatch-logs-verification.png)

---

#### 2. Kiểm tra Biểu đồ Chỉ số Hiệu năng (CloudWatch Metrics)

* **Các bước thực hiện:**
  1. Vào CloudWatch Console -> Chọn **Metrics** -> **All metrics** -> Chọn tên miền dịch vụ **Lambda** hoặc **API Gateway**.
  2. Quan sát các chỉ số vận hành real-time:
     * **Invocations:** Tổng số lần hàm Lambda được gọi.
     * **Duration:** Thời gian phản hồi trung bình của hàm (Execution Time).
     * **Error count & Success rate (%):** Số lượng request gặp lỗi (mục tiêu 0%).
     * **Throttles:** Kiểm tra sự cố nghẽn giới hạn gọi đồng thời (Concurrent Executions).

* **Kết quả kỳ vọng:**
  * Các biểu đồ hiển thị trực quan lưu lượng truy cập hệ thống.
  * Tốc độ phản hồi của Lambda duy trì ở mức tối ưu (dưới $200\text{ ms}$ cho các thao tác đọc/ghi DynamoDB).

![Quan sát Metrics của Lambda và API Gateway](/images/5-Workshop/5.4-testing/cloudwatch-metrics-dashboard.png)

---

#### 3. Kiểm tra Cấu hình Cảnh báo Cước phí (AWS Budgets & CloudWatch Billing Alarm)

* **Các bước thực hiện:**
  1. Truy cập CloudWatch Console -> **Alarms** -> **In alarm** / **OK**.
  2. Kiểm tra trạng thái của Alarm giám sát cước phí tài nguyên (Billing Alarm) ngưỡng $1.00\text{ USD}$.

* **Kết quả kỳ vọng:**
  * Alarm duy trì ở trạng thái **`OK`** do toàn bộ tải kiểm thử nằm trong hạn mức **AWS Free Tier**.
  * Cấu hình gửi thông báo Email qua **Amazon SNS** đã sẵn sàng nếu chi phí vượt ngưỡng quy định.

![Xác minh trạng thái CloudWatch Billing Alarm](/images/5-Workshop/5.4-testing/cloudwatch-billing-alarm.png)