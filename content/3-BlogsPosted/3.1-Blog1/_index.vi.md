---
title: "Blog 1: Lưu Trữ Dữ Liệu Địa Phương Với Amazon S3 Trong AWS Local Zones"
date: 2026-07-20
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---



# LƯU TRỮ DỮ LIỆU NGAY TẠI ĐỊA PHƯƠNG VỚI AMAZON S3 TRONG AWS LOCAL ZONES

Trong thời đại điện toán đám mây, doanh nghiệp có thể lưu trữ dữ liệu tại các trung tâm dữ liệu ở nhiều nơi trên thế giới. Tuy nhiên, một số lĩnh vực như ngân hàng, y tế và cơ quan nhà nước thường có yêu cầu nghiêm ngặt về vị trí lưu trữ dữ liệu. Chẳng hạn, dữ liệu của người dùng tại Việt Nam có thể được yêu cầu lưu giữ trong một phạm vi địa lý nhất định thay vì chuyển đến một AWS Region ở quốc gia khác.

Trước đây, để đáp ứng yêu cầu này, doanh nghiệp thường phải tự xây dựng trung tâm dữ liệu hoặc duy trì hệ thống lưu trữ tại chỗ (On-premise). Cách làm đó không chỉ tốn kém mà còn đòi hỏi đội ngũ kỹ thuật quản lý phần cứng, sao lưu, bảo trì và mở rộng dung lượng.

**Amazon S3 trong AWS Local Zones** được giới thiệu nhằm giải quyết vấn đề trên. Giải pháp cho phép doanh nghiệp lưu trữ dữ liệu trong một khu vực địa lý cụ thể nhưng vẫn sử dụng những công cụ và cách làm quen thuộc của Amazon S3. Một số Local Zone hiện hỗ trợ S3 bao gồm Istanbul, Athens và đặc biệt là **Hà Nội, Việt Nam**.

---

## 🌟 NHỮNG ĐIỂM NỔI BẬT

1. **Lưu trữ dữ liệu trong phạm vi địa lý cụ thể:** Khi tạo *S3 Directory Bucket*, người dùng có thể lựa chọn Local Zone nơi dữ liệu sẽ được lưu trữ. Điều này giúp doanh nghiệp kiểm soát rõ ràng vị trí của dữ liệu và hỗ trợ đáp ứng các yêu cầu về **Data Residency** (Yêu cầu dữ liệu phải nằm trong một quốc gia hoặc khu vực nhất định).

2. **Amazon S3 được đưa đến gần người dùng hơn:** AWS Local Zones mở rộng các dịch vụ AWS đến những thành phố nằm xa AWS Region chính. Nhờ đó, ứng dụng có thể xử lý và truy cập dữ liệu tại một vị trí gần người dùng hơn, giúp giảm đáng kể độ trễ (latency).

3. **Sử dụng S3 Directory Bucket:** Amazon S3 trong Local Zones sử dụng loại bucket chuyên dụng được gọi là **Directory Bucket**. Khác với *S3 General Purpose Bucket* thông thường lưu dữ liệu trên nhiều Availability Zone, Directory Bucket được thiết kế để lưu dữ liệu trong một Zone đã chọn với cấu trúc tối ưu cho độ trễ cực thấp.

4. **Tiếp tục sử dụng công cụ S3 quen thuộc:** Lập trình viên không phải học lại hệ thống mới. Dữ liệu vẫn có thể được tải lên, tải xuống và quản lý thông qua AWS Management Console, AWS CLI, AWS SDK hoặc các S3 API quen thuộc như `PutObject`, `GetObject` và `CopyObject`.

5. **Tự động điều hướng đến đúng endpoint:** Các thao tác quản lý bucket (tạo bucket, thiết lập chính sách) được xử lý tại AWS Region chính. Trong khi đó, các thao tác dữ liệu trực tiếp (tải lên/tải xuống) được tự động điều hướng trực tiếp đến Local Zone.

6. **Bảo vệ dữ liệu khỏi truy cập công khai:** Với S3 Directory Bucket, tính năng **Block Public Access** luôn được bật mặc định. Quyền truy cập được kiểm soát chặt chẽ thông qua AWS IAM và Bucket Policies.

7. **Hỗ trợ nhiều phương pháp di chuyển dữ liệu:** Doanh nghiệp có thể sao chép dữ liệu từ S3 bucket trong Region chính sang Directory Bucket trong Local Zone bằng S3 Batch Operations hoặc lệnh AWS CLI:
   ```bash
   aws s3 cp --recursive s3://my-region-bucket s3://my-localzone-bucket--han1-az1--x-s3
   ```

8. **Mở rộng khả năng cho các dịch vụ AWS khác:** Cho phép Amazon EBS lưu Snapshot máy chủ EC2 ngay trong Local Zone và Amazon EMR xử lý dữ liệu lớn tại địa phương mà không cần chuyển về Region chính.

---

## 🏥 TÌNH HUỐNG THỰC TẾ: HỆ THỐNG Y TẾ TẠI VIỆT NAM

Một bệnh viện tại Việt Nam đang xây dựng hệ thống lưu trữ và phân tích hình ảnh y tế (PACS/DICOM). Vì đây là dữ liệu nhạy cảm, bệnh viện cần bảo đảm dữ liệu được lưu trữ trong phạm vi địa lý Việt Nam, đồng thời hệ thống phải có khả năng tự động mở rộng.

Với **Amazon S3 trong Hanoi Local Zone**, bệnh viện xây dựng kiến trúc sau:

```text
[Người dùng / Thiết bị Y tế] 
       │
       ▼
[Ứng dụng EC2 tại Hanoi Local Zone]
       │
       ▼
[Amazon S3 Directory Bucket (Hanoi Local Zone)]
```

* **Ứng dụng tại Local Zone:** Triển khai trên Amazon EC2 trong Hanoi Local Zone để xử lý ảnh y tế ngay gần nơi dữ liệu được tạo ra.
* **Amazon S3 Directory Bucket:** Lưu trữ hình ảnh và hồ sơ dưới dạng object trong Directory Bucket đặt tại Hà Nội.
* **AWS IAM & Bucket Policy:** Phân quyền chặt chẽ (Bác sĩ chỉ có quyền đọc/xem, thiết bị chụp chiếu chỉ có quyền ghi/upload).
* **Amazon EBS Local Snapshot:** Lưu bản sao lưu máy chủ EC2 ngay tại Local Zone Hà Nội.

---

## 📊 DI CHUYỂN DỮ LIỆU VÀO LOCAL ZONE

* **Dữ liệu lớn từ Region cũ:** Sử dụng **S3 Batch Operations** để tự động hóa sao chép hàng triệu tệp tin và nhận báo cáo hoàn thành.
* **Dữ liệu từ On-Premise / IoT:** Sử dụng AWS CLI hoặc tích hợp `PutObject` API trực tiếp từ ứng dụng nội bộ.
* *Lưu ý kỹ thuật:* Lệnh `aws s3 sync` hiện chưa hỗ trợ trực tiếp với Directory Bucket, người dùng nên sử dụng lệnh `aws s3 cp --recursive`.

---

## 📝 KẾT LUẬN

Amazon S3 trong AWS Local Zones (đặc biệt là **Hanoi Local Zone**) là bước tiến quan trọng giúp giải quyết bài toán cân bằng giữa **Data Residency** (Tuân thủ vị trí dữ liệu) và **Cloud Flexibility** (Tính linh hoạt của điện toán đám mây) cho các doanh nghiệp Tài chính, Y tế và Cơ quan nhà nước tại Việt Nam.

🔗 **Link tài liệu tham khảo gốc:** [AWS Blog: Unlocking Data Residency with Amazon S3 in AWS Local Zones](https://aws.amazon.com/blogs/aws/unlocking-data-residency-with-amazon-s3-in-aws-local-zones/)

![Blog](<../../../images/3-Blogs/Blog1.png>)