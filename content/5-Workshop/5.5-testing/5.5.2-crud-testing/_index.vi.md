---
title: "Kiểm thử Chức năng Quản lý Task & Note (CRUD Operations)"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả cảnh báo này.
{{% /notice %}}

Trong mục này, chúng ta tiến hành kiểm thử các thao tác CRUD (Create, Read, Update, Delete) cho công việc và ghi chú, bao gồm cả tính năng đính kèm hình ảnh thông qua S3 Presigned URL và đồng bộ trạng thái trên giao diện Kanban.

---

#### 1. Kiểm thử Tạo mới Task & Đính kèm Ảnh (Create & Attach Image)

* **Các bước thực hiện:**
  1. Nhấn nút **Create New Task** trên giao diện Web.
  2. Điền Tiêu đề (Title), Mô tả (Description), chọn Danh mục (Category), Gán thẻ (Tags) và ngày hết hạn (Due Date).
  3. Tải lên một tệp hình ảnh đính kèm (`.png` / `.jpg`). Frontend sẽ gọi API lấy S3 Presigned URL và upload ảnh trực tiếp lên **S3 Attachments Bucket**.
  4. Nhấn **Save Task**.

* **Kết quả kỳ vọng:**
  * Request `POST /tasks` gửi thành công, trả về HTTP **`201 Created`**.
  * Bảng **Amazon DynamoDB (`TodoNotesTable`)** xuất hiện bản ghi mới chứa thông tin `userId` (Primary Key) và `taskId` (Sort Key).
  * Ảnh đính kèm hiển thị chính xác trên thẻ công việc.

![Tạo mới Task và Upload ảnh](/images/5-Workshop/5.5-testing/create-task.png)

---

#### 2. Kiểm thử Xem danh sách, Lọc & Tìm kiếm (Read & Query)

* **Các bước thực hiện:**
  1. Truy cập Màn hình chính ứng dụng.
  2. Chuyển đổi giữa các giao diện hiển thị: **List View**, **Kanban Board** và **Calendar View**.
  3. Nhập từ khóa vào thanh **Search Bar** và lọc theo Tag/Status.

* **Kết quả kỳ vọng:**
  * API `GET /tasks` thực thi và trả về đúng danh sách dữ liệu thuộc về người dùng đang đăng nhập.
  * Bộ lọc và công cụ tìm kiếm hoạt động chính xác theo thời gian thực.

![Giao diện danh sách Task và Bộ lọc](/images/5-Workshop/5.5-testing/read-tasks-kanban.png)

---

#### 3. Kiểm thử Cập nhật Task & Kéo thả Kanban (Update)

* **Các bước thực hiện:**
  1. Mở một Task hiện có, thay đổi tiêu đề hoặc nội dung mô tả, sau đó bấm **Update**.
  2. Tại giao diện **Kanban Board**, thực hiện thao tác kéo thả (Drag-and-Drop) thẻ công việc từ cột *In Progress* sang *Completed*.

* **Kết quả kỳ vọng:**
  * API `PUT /tasks/{id}` trả về HTTP **`200 OK`**.
  * Trạng thái (`status`) của công việc trong DynamoDB được cập nhật tương ứng (`COMPLETED`).

![Cập nhật trạng thái Task trên Kanban](/images/5-Workshop/5.5-testing/update-task-kanban.png)

---

#### 4. Kiểm thử Xóa Task (Delete)

* **Các bước thực hiện:**
  1. Nhấn nút **Delete (Xóa)** tại thẻ công việc cần loại bỏ.
  2. Xác nhận xóa trên hộp thoại cảnh báo (Confirmation Modal).

* **Kết quả kỳ vọng:**
  * API `DELETE /tasks/{id}` trả về HTTP **`200 OK`**.
  * Bản ghi tương ứng bị xóa hoàn toàn khỏi bảng DynamoDB.

![Xóa Task thành công](/images/5-Workshop/5.5-testing/delete-task.png)