---
title: "Event 2"
date: 2026-06-13
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “Kiến trúc AWS mở rộng và các nguyên tắc cơ bản về DevOps”

### Mục Đích Của Sự Kiện

- Chia sẻ cách thiết kế và triển khai kiến trúc hệ thống mở rộng, điển hình như dịch vụ rút gọn liên kết trên nền tảng AWS.
- Cung cấp góc nhìn thực tế, chia sẻ kinh  về vai trò, công việc hằng ngày của các vị trí DevOps và Data Analytics Engineer tại doanh nghiệp.
- Hướng dẫn lộ trình học tập, trang bị kỹ năng nền tảng và định hướng phát triển sự nghiệp trong ngành IT từ khi còn là sinh viên.
- Giới thiệu về quy trình tuyển dụng chuẩn và văn hóa làm việc tại các tập đoàn đa quốc gia.

### Danh Sách Diễn Giả

- **Đạt Phạm** - Data Analytics Engineer
- **Cường Nguyễn** - Process Engineer
- **Trong H. Truong** - DevOps Engineer tại Endava Vietnam
- **Danh Hoàng Hiếu Nghị** - AI Engineer, AWS Community Builder, AWS Student Builder Group Leader
- **Đinh Trung Kiên** - Lead developer at starup
- **Nguyễn Minh Thọ** - Student

### Nội Dung Nổi Bật

#### Hành Trình Bắt Đầu Và Phát Triển Với Điện Toán Đám Mây

Khởi đầu từ sự tò mò của sinh viên (Student Curiosity) <br>
→ Học hỏi từ các cộng đồng công nghệ <br>
→ Thực hành qua các bài Lab thực tế <br>
→ Xây dựng dự án & Portfolio cá nhân → Trở thành đối tác (AWS Partner) <br>
→ Chia sẻ tri thức ngược lại cho cộng đồng (Share Back).<br>
Nhận được công việc không phải là đích đến mà chỉ là điểm bắt đầu cho một hành trình học tập và cống hiến lâu dài.

#### Giới Thiệu Về Thiết Kế Hệ Thống Rút Gọn Liên Kết - URL Shortener Trên AWS

- Cấu trúc cơ bản tuy có **ưu điểm** dễ triển khai và chi phí thấp nhưng lại gặp **hạn chế** về rủi ro bảo mật, single point of failure, có độ trễ và khó mở rộng quy mô.
- Để tối ưu hóa và bảo mật có thể kết hợp các dịch vụ như Amazon CloudFront, Amazon WAF và Amazon Amplify.
- Sử dụng dịch vụ Key Generation trên Amazon ECS để tạo sẵn mã ngắn và đẩy vào bộ nhớ đệm của Amazon ElastiCache giúp tối ưu hóa tốc độ hệ thống.
- Ở Backend, ứng dụng SpringBoot trên Amazon ECS sẽ lấy mã ngắn từ Redis để liên kết với URL đích và lưu trữ vào cơ sở dữ liệu DynamoDB.

#### Thực Tế Công Việc Và Lộ Trình Phát Triển Của DevOps Engineer

Đầu tiên, để hiểu đúng về DevOps thì công việc này không chỉ xoay quanh việc viết CI/CD pipeline, cấu hình đám mây hay quản lý Docker/Kubernetes, mà còn đòi hỏi hiểu rõ cách ứng dụng vận hành trong thực tế.<br>
**Về kiến thức nền tảng cần có:**
- Cần ưu tiên nắm vững Linux
- Kiến thức cơ bản về Networking
- các ngôn ngữ lập trình (như Python, Golang)
- Git
- CI/CD
- Containers

**Tư duy làm việc:** Công cụ có thể thay đổi nhưng kiến thức nền tảng thì không. Một kỹ sư DevOps giỏi cần rèn luyện tư duy hệ thống, giữ sự tò mò và luôn học hỏi, tự động hóa các công việc nhàm chán, giữ mọi thứ đơn giản và dễ hiểu cho mọi người trong team.

#### Định Hướng Nghề Nghiệp Và Kỹ Năng Của Data Analytics Engineer

- **Thực tế công việc:**  Thay đổi theo từng ngành nghề, tập trung vào xây dựng báo cáo, thiết kế Dashboard theo dõi xu hướng, phân tích nguyên nhân gốc rễ và phối hợp cùng đa phòng ban để giải quyết các bài toán vận hành.
- **Kỹ năng cần có:** Khả năng tư duy phản biện để nhìn nhận thông tin khách quan, kỹ năng giao tiếp hiệu quả, năng lực kể chuyện với dữ liệu, tìm ra giải pháp tối ưu dựa trên dữ liệu.
- **Lộ trình thăng tiến:** Người thực thi (Follower) → Người học chủ động (Learner) → Người giải quyết vấn đề (Problem Solver) → Người tư duy hệ thống (System Thinker) →  dẫn dắt (Super Star).

#### Văn Hóa Doanh Nghiệp Tại Các Tập Đoàn Đa Quốc Gia

- **Quy trình tuyển dụng bài bản:** Ứng viên trải qua nhiều vòng sàng lọc từ hệ thống ATS đến phỏng vấn đánh giá năng lực chuyên môn và độ hòa hợp văn hóa.
- **Môi trường làm việc**: Tạo dựng văn hóa làm việc tôn trọng, quan tâm đến từng cá nhân, đề cao sự đa dạng và khuyến khích sự phát triển toàn diện.
- **Văn hóa "No-Blame Post-Mortem"**: Khi sự cố vận hành xảy ra, doanh nghiệp tập trung phân tích nguyên nhân gốc rễ để cải tiến hệ thống và quy trình thay vì quy trách nhiệm hay đổ lỗi cho

### Những Gì Học Được

#### Tư Duy Kiến Trúc & Kỹ Thuật System Design

- **Tư duy mở rộng hệ thống**: Luôn hướng đến một hệ thống có khả năng mở rộng và linh hoạt. Hiểu rõ cách chuyển dịch từ kiến trúc monolithic đơn giản sang kiến trúc phân tán có khả năng chịu tải cao, sử dụng caching và NoSQL.
- **Tách biệt  nhiệm vụ - Concerns**: Thiết kế dịch vụ KGS riêng biệt có thể giúp giảm tải trực tiếp cho cơ sở dữ liệu chính khi tạo mã URL
- **Tối ưu hóa độ trễ & Bảo mật**: Kết hợp CloudFront, WAF và API Gateway để bảo vệ hệ thống và mang lại trải nghiệm tối ưu cho người dùng cuối.

#### Tư Duy DevOps & Vận Hành Hệ Thống

- Nắm chắc các **kiến thức cốt lõi** như Linux, Networking, Containers thay vì ỷ lại, chạy theo các công cụ tiện ích luôn thay đổi liên tục theo thời gian.
- Học cách **Tư duy hệ thống**, nhìn nhận một ứng dụng trong toàn bộ vòng đời (Build, Test, Deploy, Monitor, Fix) thay vì chỉ hoàn thành nhiệm vụ đơn lẻ.
- Học cách phân tích sự cố, luôn tìm hướng cải tiến hệ thống để ngăn lỗi tái diễn cũng như phòng ngừa những lỗ hổng có thể xảy ra trong tương .

#### Lộ Trình Nghề Nghiệp & Văn Hóa Ứng ử

- Lộ trình phát triển sự nghiệp cần xây dựng qua sản phẩm thực tế, chứng chỉ, đóng góp cộng đồng và không ngừng học hỏi.
- Bên cạnh kiến thức chuyên môn, kỹ năng mềm đặc biệt là giao tiếp, lắng nghe đóng vai trò quan trọng trong môi trường quốc tế.

### Ứng Dụng Vào Công Việc

- **Caching**: Tối ưu hóa truy vấn dữ liệu khi đưa Redis làm bộ nhớ đệm
- **DynamoDB**: Cơ sở dữ liệu NoSQL → dựng các ứng dụng yêu cầu tính bảo mật cao, tốc độ phản hồi cao, linh hoạt, tự động mở rộng.
- **Automate CI/CD pipelines**: Dùng Python/Golang viết các automation scripts, tối ưu Dockerfile, loại bỏ các manual tasks lặp đi lặp lại.
- **Enhance Data Storytelling**: Thiết kế Dashboard tập trung vào các business metrics quan trọng và thực hiện RCA.
- **Build hands-on portfolio**: Tham gia các AWS labs thực tế và chủ động share back trong community để nâng cao practical skills và mở rộng network.

### Trải nghiệm trong event

Tham gia sự kiện là một trải nghiệm vô cùng giá trị và bổ ích, giúp tôi mở rộng tư duy về thiết kế kiến trúc AWS, hiểu rõ bản chất công việc DevOps, Data Analytics và định hình lộ trình phát triển sự nghiệp công nghệ.

#### Học hỏi từ các diễn giả có chuyên môn cao
- Các diễn giả giàu kinh nghiệm đến từ AWS Community, Endava, Colgate-Palmolive và Kamereo đã đem lại những câu chuyện thực chiến vô cùng phong phú và chân thực.
- Những chia sẻ không dừng lại ở lý thuyết mà đi sâu vào các bài toán kỹ thuật thực tế cũng như góc nhìn quản trị tại các tập đoàn lớn.

#### Trải nghiệm kỹ thuật thực tế
- Lắng nghe phân tích từng thành phần trong mô hình URL Shortener giúp tôi hình dung rõ ràng cách kết hợp các dịch vụ cloud để giải quyết bài toán độ trễ và khả năng mở rộng.
- Hiểu được góc nhìn thực sự từ một DevOps Engineer có kinh nghiệm giúp tôi giải tỏa những hiểu lầm phổ biến về nghề, biết được những góc khuất cũng như khó khăn từ đó xác định phương hướng, lựa chọn đúng trọng tâm học tập.

#### Tác Động Về Tư Duy & Định Hướng Nghề Nghiệp
- Tiếp thu tư duy "No-Blame Post-Mortem" giúp thay đổi góc nhìn về sai sót trong công việc: sự cố là cơ hội để củng cố hệ thống chứ không phải để quy trách nhiệm.
- Thấy rõ lộ trình phát triển từ một sinh viên trở thành AWS Community Builder và AWS Partner, tạo động lực mạnh mẽ cho bản thân trong việc kiên trì tích lũy kiến thức và hướng tới những cột mốc mới.

#### Kết nối và trao đổi
- Sự kiện mang không khí cởi mở, tạo cơ hội trao đổi trực tiếp, liên lạc với các chuyên gia trong ngành.
- Củng cố tầm quan trọng của việc **tham gia cộng đồng** (như AWS Student Builder Group, First Cloud AI Journey) để mở rộng mạng lưới quan hệ và cùng nhau phát triển.

#### Bài học rút ra
- Kiến thức nền tảng và tư duy giải quyết vấn đề mới là tài sản lâu dài, công cụ công nghệ sẽ luôn thay đổi.
- Thành công trong sự nghiệp đòi hỏi sự kết hợp hài hòa giữa năng lực kỹ thuật chuyên sâu và kỹ năng giao tiếp, hòa nhập, thấu hiểu văn hóa.
- Luôn chủ động học hỏi, xây dựng sản phẩm thực tế và sẵn sàng chia sẻ tri thức ngược lại cho cộng đồng.

#### Một số hình ảnh khi tham gia sự kiện
* Thêm các hình ảnh của các bạn tại đây
> Tổng kết lại, sự kiện đem lại góc nhìn mới, những kinh nghiệm, chia sẻ quý báu và không kém phần truyền cảm hứng từ các anh chị diễn giả giúp tôi có cái nhìn rõ ràng hơn về định hướng phát triển trong tương lai cảu bản thân.
