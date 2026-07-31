---
title: "Blog 3: Xử Lý Hàng Triệu Bản Ghi DynamoDB Dễ Dàng Hơn Với Bulk Executor"
date: 2026-07-24
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả cảnh báo này.
{{% /notice %}}

# XỬ LÝ HÀNG TRIỆU BẢN GHI DYNAMODB DỄ DÀNG HƠN VỚI BULK EXECUTOR

Amazon DynamoDB được thiết kế để xử lý dữ liệu với tốc độ cao và tự động mở rộng theo lưu lượng của ứng dụng. Trong hoạt động hàng ngày, ứng dụng thường truy vấn hoặc cập nhật từng item dựa trên Partition Key và Sort Key.

Tuy nhiên, doanh nghiệp đôi khi cần thực hiện các thao tác trên toàn bộ bảng với quy mô hàng triệu hay hàng tỷ bản ghi: đếm số item cũ, bổ sung thuộc tính TTL, sao chép dữ liệu sang bảng/tài khoản khác hoặc nạp dữ liệu lớn từ Amazon S3. Trước đây, việc này đòi hỏi lập trình viên phải tự viết script phức tạp xử lý Segmented Scan, phân chia luồng song song, giới hạn rate limit, retry khi nghẽn mạng và theo dõi tiến trình.

**Bulk Executor for Amazon DynamoDB** là công cụ mã nguồn mở do AWS Labs phát triển giúp đơn giản hóa toàn bộ quá trình trên. Lập trình viên chỉ cần chạy các câu lệnh CLI đơn giản trên terminal, trong khi toàn bộ công việc tính toán phân tán song song sẽ do **AWS Glue (Apache Spark)** đảm nhận ở phía sau.

---

## 🌟 NHỮNG ĐIỂM NỔI BẬT

1. **Trải nghiệm dòng lệnh (CLI) đơn giản:** Người dùng tương tác trực tiếp qua Terminal (trên máy cá nhân, EC2 hoặc Container). Công cụ hoạt động như một ứng dụng cục bộ nhưng sức mạnh xử lý dữ liệu lớn được gánh vác bởi AWS Glue trên Cloud.
2. **Không cần tự dựng hệ thống xử lý song song:** Bulk Executor tự động quản lý Segmented Scan, Spark DataFrame, phân chia task, tổng hợp kết quả và xử lý lỗi. Người dùng không cần kiến thức chuyên sâu về Apache Spark hay AWS Glue.
3. **Cung cấp sẵn nhiều lệnh tiện ích:** Hỗ trợ trực tiếp các thao tác phổ biến mà không cần viết code: `count`, `find`, `update`, `delete`, `copy`, `fill`, `load`, `diff` và `sql`.
4. **Xử lý ở quy mô cực lớn:** Sử dụng AWS Glue Spark để chia dữ liệu và phân phối cho nhiều Worker, đáp ứng tốt từ bảng nhỏ đến các bảng chứa hàng triệu hay hàng tỷ items.
5. **Theo dõi tiến trình theo thời gian thực:** Tích hợp **Amazon CloudWatch Live Tail** đưa Log từ AWS Glue về trực tiếp giao diện Terminal, giúp theo dõi Job ID, tiến độ, thời gian và chi phí mà không cần chuyển sang AWS Glue Console.
6. **Chủ động kiểm soát Rate Limit:** Cho phép thiết lập giới hạn mức đọc/ghi tối đa (Read/Write Capacity limits) để tránh làm nghẽn hoặc ảnh hưởng đến lưu lượng truy cập của người dùng thật trên bảng Production.
7. **Khả năng mở rộng bằng Python:** Lập trình viên có thể viết các hàm Python tùy chỉnh để xử lý logic nghiệp vụ riêng. Bulk Executor sẽ chịu trách nhiệm chạy hàm đó song song trên toàn bộ bảng.
8. **Mã nguồn mở:** Phát hành chính thức trong kho mã nguồn `amazon-dynamodb-tools` của AWS Labs trên GitHub.

---

## 🛠️ CÁC THAO TÁC BULK PHỔ BIẾN

| Lệnh | Mô tả chức năng |
| :--- | :--- |
| **`COUNT`** | Đếm tổng số item trong bảng hoặc đếm theo điều kiện lọc Spark SQL (vd: đếm đơn hàng tạo trước mốc thời gian cụ thể). |
| **`FIND`** | Quét toàn bộ bảng để tìm các item thỏa điều kiện khi chưa có GSI phù hợp. Kết quả lớn sẽ được tự động xuất ra Amazon S3. |
| **`UPDATE`** | Quét và truyền từng item qua hàm Python tùy chỉnh để cập nhật thuộc tính (vd: thêm thuộc tính TTL, chuẩn hóa định dạng dữ liệu). |
| **`DELETE`** | Xóa hàng loạt item thỏa điều kiện. *Yêu cầu bắt buộc phải bật Point-in-Time Recovery (PITR) trước khi thực hiện để đảm bảo an toàn.* |
| **`COPY`** | Sao chép dữ liệu giữa các bảng DynamoDB (hỗ trợ khác Tài khoản AWS hoặc khác AWS Region). |
| **`FILL`** | Tạo dữ liệu giả (Mock Data) số lượng lớn kết hợp thư viện Faker để kiểm thử hiệu năng và thiết kế Partition Key. |
| **`LOAD`** | Nạp dữ liệu từ các file CSV, JSON hoặc Parquet trên Amazon S3 vào bảng DynamoDB. |
| **`DIFF`** | So sánh sự khác biệt chi tiết giữa 2 bảng (báo cáo các item mới được thêm, bị xóa hoặc bị thay đổi nội dung). |
| **`SQL`** | Chạy các câu lệnh Spark SQL (`SELECT`) trực tiếp trên dữ liệu DynamoDB để tính toán thống kê, tổng hợp. |

---

## 📐 KIẾN TRÚC HOẠT ĐỘNG & TÌNH HUỐNG THỰC TẾ

### Mô hình kiến trúc
```text
[Người quản trị (Terminal / CLI)]
             │
             ▼
    [Bulk Executor CLI]
             │
             ▼
     [AWS Glue Job (Spark)] ◄──► [Amazon S3 (Scripts & Output Logs)]
             │
             ▼
    [Amazon DynamoDB Table]
```

### Tình huống thực tế:
Một hệ thống Thương mại Điện tử muốn xử lý dữ liệu hàng triệu đơn hàng cũ:
1. **Bước 1:** Dùng lệnh **`count`** để ước tính số lượng đơn hàng cũ.
2. **Bước 2:** Chạy lệnh **`update`** kèm hàm Python để bổ sung thuộc tính TTL (Time To Live) cho dữ liệu tự động hết hạn.
3. **Bước 3:** Chạy lệnh **`copy`** để sao chép dữ liệu sang bảng kiểm thử mới và dùng lệnh **`diff`** để xác minh tính chính xác của dữ liệu sau khi chép.

---

## 🔐 QUẢN LÝ BẢO MẬT, CHI PHÍ VÀ LƯU Ý

* **Bảo vệ dữ liệu với PITR:** Trước khi chạy các thao tác ghi/xóa hàng loạt (`update`, `delete`), Bulk Executor yêu cầu bảng phải được bật **Point-in-Time Recovery (PITR)** để có thể khôi phục dữ liệu nếu xảy ra sơ xuất.
* **Phân quyền IAM Least Privilege:** Sử dụng cơ chế Bootstrap tạo sẵn các tài nguyên cần thiết (Glue Job, S3 Bucket, CloudWatch Log Group). Phân chia rõ vai trò giữa *Bulk Admin* (thiết lập môi trường) và *Bulk User* (chỉ có quyền chạy job).
* **Quản lý chi phí:** Quá trình chạy vẫn tiêu thụ tài nguyên DynamoDB RCU/WCU, AWS Glue DPU và lưu trữ S3/CloudWatch. Nên chạy các Bulk Job lớn vào thời điểm lưu lượng thấp (Off-peak hours) và thiết lập giới hạn tốc độ đọc/ghi.

---

## 📝 KẾT LUẬN

**Bulk Executor for Amazon DynamoDB** là công cụ mạnh mẽ giúp biến các bài toán xử lý dữ liệu quy mô lớn phức tạp thành những câu lệnh CLI đơn giản. Việc kết hợp giữa giao diện dòng lệnh nhẹ nhàng và sức mạnh tính toán phân tán của AWS Glue giúp doanh nghiệp tiết kiệm đáng kể thời gian phát triển và vận hành hệ thống.

🔗 **Tài liệu tham khảo gốc:**
* [AWS Database Blog: Introducing open source Bulk Executor for Amazon DynamoDB](https://aws.amazon.com/blogs/database/introducing-open-source-bulk-executor-for-amazon-dynamodb/)
* [GitHub Repository: awslabs/amazon-dynamodb-tools](https://github.com/awslabs/amazon-dynamodb-tools/tree/main/tools/bulk_executor)