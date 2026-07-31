---
title: "Event 2"
date: 2026-07-11
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Báo cáo tổng kết: “SLA, giám sát hệ thống, AWS Security Agent và AWS Cloud Practitioner”

### Mục tiêu sự kiện

- Giải thích vai trò của SLA và cách xây dựng hệ thống giám sát dựa trên những tín hiệu thực sự ảnh hưởng đến người dùng.

- Giới thiệu quy trình nhận diện rủi ro, theo dõi tín hiệu, phản ứng sự cố và cải tiến hệ thống sau sự cố.

- Trình bày khả năng của AWS Security Agent trong việc hỗ trợ đánh giá thiết kế, kiểm tra mã nguồn và kiểm thử bảo mật ứng dụng.

- Phân tích những lợi ích và giới hạn của việc tự động hóa kiểm thử bảo mật bằng AI Agent.

- Cung cấp cái nhìn tổng quan về chứng chỉ AWS Certified Cloud Practitioner và bốn miền kiến thức của kỳ thi.

### Nội dung nổi bật

#### Từ SLA đến giám sát những gì thực sự quan trọng

- **SLA là một cam kết chính thức**: Service Level Agreement xác định mức dịch vụ mà nhà cung cấp cam kết với khách hàng. SLA giúp làm rõ kỳ vọng, trách nhiệm giải trình, phương pháp đo lường hiệu năng và cách quản trị rủi ro.
- **Giám sát là một phần của quản trị rủi ro**: Mục tiêu của monitoring không chỉ là quan sát hệ thống mà còn phát hiện rủi ro trước khi chúng gây ảnh hưởng đến SLA và trải nghiệm khách hàng.
- **Nhận diện rủi ro**: Trước tiên cần xác định những tình huống có thể ảnh hưởng đến tính sẵn sàng, hiệu năng hoặc khả năng hoàn thành tác vụ của người dùng.
- **Theo dõi tín hiệu**: Sau khi xác định rủi ro, hệ thống cần thu thập metric, log và alarm tương ứng để phát hiện dấu hiệu bất thường càng sớm càng tốt.
- **Phản ứng sự cố**: Khi alarm được kích hoạt, hệ thống có thể gửi thông báo qua Amazon SNS, áp dụng SOP và tiến hành các bước khôi phục cần thiết.
- **Cải tiến liên tục**: Sau sự cố, đội ngũ cần đánh giá nguyên nhân, xem lại hiệu quả phản ứng và điều chỉnh hệ thống giám sát để ngăn vấn đề tái diễn.

#### Mô hình kim tự tháp giám sát

Kim tự tháp giám sát cho thấy dữ liệu kỹ thuật ở tầng dưới cần được liên kết với tác động thực tế ở tầng trên:

1. **Customer Experience** - Nằm ở đỉnh kim tự tháp, phản ánh khả năng người dùng cuối hoàn thành hành trình và có được trải nghiệm mong đợi.
2. **Business** - Theo dõi các chỉ số như tỷ lệ đăng nhập thành công, số lượng đơn hàng, tỷ lệ hoàn thành giao dịch hoặc doanh thu.
3. **Application** - Quan sát latency, error rate và request count để đánh giá hành vi của ứng dụng.
4. **Infrastructure** - Theo dõi CPU, memory, disk và network của tài nguyên hạ tầng.
5. **Cloud Provider** - Nằm ở đáy kim tự tháp, thể hiện trạng thái của các dịch vụ nền tảng như Amazon EC2, Amazon RDS, Elastic Load Balancing và Amazon S3.

- **Healthy infrastructure không đồng nghĩa với happy users**: Health check có thể thành công trong khi người dùng vẫn không đăng nhập, đặt hàng hoặc hoàn thành tác vụ được.
- **Hạ tầng không thể mô tả toàn bộ chất lượng dịch vụ**: CPU và memory ổn định không có nghĩa là luồng nghiệp vụ đang hoạt động đúng.
- **Trách nhiệm được chia sẻ**: AWS chịu trách nhiệm về hạ tầng cloud thuộc phạm vi của AWS, còn đội ngũ xây dựng ứng dụng vẫn chịu trách nhiệm với cấu hình, dữ liệu, logic ứng dụng và trải nghiệm người dùng.
- **Cần hiểu hành trình người dùng**: Monitoring hiệu quả phải bắt đầu từ việc hiểu người dùng muốn làm gì và điều gì có thể khiến hành trình đó thất bại.

#### Bảo vệ ứng dụng web với AWS Security Agent

- **Pentest thủ công có thể kéo dài**: Các đợt kiểm thử truyền thống thường cần nhiều tuần để chuẩn bị, thực hiện, xác minh và viết báo cáo.
- **Chi phí chuyên gia cao**: Theo nội dung chia sẻ, chi phí thuê dịch vụ pentest bên ngoài có thể dao động từ 5.000 đến 20.000 USD, tùy phạm vi và độ phức tạp.
- **Chất lượng phụ thuộc vào chuyên môn**: Kết quả kiểm thử chịu ảnh hưởng lớn bởi kinh nghiệm, phương pháp và khả năng phân tích của pentester.

#### Frontier Agent và bảo mật toàn bộ vòng đời phát triển

- **Vận hành bởi Amazon Bedrock**: Security Agent có thể lập kế hoạch và thực hiện chuỗi tác vụ bảo mật phức tạp với mức độ tự động hóa cao.
- **Design Review**: Agent phân tích tài liệu kiến trúc và kiểm tra thiết kế theo những khung tham chiếu như PCI DSS, NIST Cybersecurity Framework và AWS Well-Architected Framework.
- **Code Security**: Agent quét pull request để tìm lỗ hổng và phát hiện dữ liệu riêng tư bị đưa vào mã nguồn, chẳng hạn mật khẩu hoặc API key.
- **Active Penetration Testing**: Trong môi trường được chủ sở hữu cho phép, Agent có thể mô phỏng hành vi của người dùng và kiểm chứng lỗ hổng thông qua các chuỗi kiểm thử nhiều bước, sau đó tạo bằng chứng và sơ đồ đường tấn công để hỗ trợ quá trình xác minh.
- **Tích hợp quy trình phát triển**: Security Agent có thể tích hợp với GitHub hoặc GitLab pull request, nhận xét tại dòng mã liên quan và đề xuất Auto-PR Fixes.
- **Bảo vệ toàn bộ vòng đời**: Ba lớp Design Review, Code Security và Active Penetration Testing giúp đưa bảo mật vào nhiều giai đoạn thay vì chờ đến cuối dự án.

#### Những giới hạn cần lưu ý

- **Cơ chế xác thực mạnh có thể làm gián đoạn tự động hóa**: MFA, sinh trắc học và mTLS yêu cầu những bước xác minh mà Agent có thể không tự hoàn thành được.
- **Thiếu ngữ cảnh nghiệp vụ**: Agent có thể gặp khó khăn với lỗi gian lận logic nếu không hiểu sâu quy tắc, vai trò và quy trình của doanh nghiệp.
- **Độ phức tạp làm tăng thời gian chạy**: Ứng dụng càng lớn và có nhiều luồng, thời gian kiểm thử càng tăng. Vì vậy cần giới hạn phạm vi và theo dõi thời gian thực thi.
- **Vẫn cần con người giám sát**: Kết quả tự động phải được đội ngũ bảo mật và phát triển đánh giá trước khi xem là kết luận cuối cùng hoặc áp dụng bản sửa lỗi.
- **Chỉ kiểm thử khi được ủy quyền**: Hoạt động pentest phải được thực hiện trên hệ thống thuộc quyền sở hữu hoặc có sự cho phép rõ ràng, với phạm vi và quy tắc kiểm thử được xác định trước.

#### Bên trong kỳ thi AWS Cloud Practitioner

- **Chứng chỉ nền tảng**: AWS Certified Cloud Practitioner tập trung vào tư duy cloud và bức tranh tổng quan về các dịch vụ AWS.
- **Không yêu cầu lập trình chuyên sâu**: Kỳ thi không yêu cầu ứng viên viết code hoặc cấu hình chi tiết một hệ thống production.
- **Domain 1 - Cloud Concepts (24%)**: Kiểm tra lợi ích của cloud, các mô hình triển khai, tính linh hoạt, khả năng mở rộng và nguyên tắc kinh tế cloud.
- **Domain 2 - Security and Compliance (30%)**: Tập trung vào shared responsibility model, IAM, bảo vệ dữ liệu, tuân thủ và các dịch vụ bảo mật cơ bản.
- **Domain 3 - Cloud Technology and Services (34%)**: Bao gồm các nhóm dịch vụ compute, storage, database, networking, analytics và những tình huống sử dụng phổ biến.
- **Domain 4 - Billing, Pricing, and Support (12%)**: Kiểm tra mô hình giá, công cụ quản lý chi phí, AWS Support và các tài nguyên hỗ trợ.

### Bài học chính

#### Monitoring phải gắn với người dùng

- Không nên chỉ xây dashboard từ CPU, memory hoặc trạng thái dịch vụ.
- Mỗi tín hiệu kỹ thuật cần được liên kết với một rủi ro, tác động nghiệp vụ hoặc hành trình người dùng cụ thể.
- Alarm chỉ có giá trị khi đi kèm người chịu trách nhiệm, kênh thông báo và SOP phản ứng rõ ràng.
- Sau mỗi sự cố, hệ thống giám sát cần được cập nhật dựa trên những tín hiệu đã bị bỏ sót.

#### Tự động hóa bảo mật cần đi cùng kiểm soát

- AI Agent có thể rút ngắn thời gian đánh giá thiết kế, kiểm tra code và xác minh một số lỗ hổng.
- Agent không thể thay thế hoàn toàn chuyên gia bảo mật, đặc biệt với lỗi logic nghiệp vụ và các hệ thống xác thực phức tạp.
- Bảo mật nên được đưa vào toàn bộ vòng đời phát triển thay vì chỉ thực hiện pentest trước ngày phát hành.
- Các hoạt động kiểm thử chủ động phải có phạm vi, quyền hạn và môi trường an toàn rõ ràng.

#### Cloud Practitioner xây dựng nền tảng AWS

- Chứng chỉ giúp người học hiểu ngôn ngữ chung của cloud trước khi đi sâu vào kiến trúc hoặc vận hành.
- Security and Compliance cùng Cloud Technology and Services chiếm phần lớn nội dung, vì vậy cần được ưu tiên trong kế hoạch học.
- Việc học nên tập trung vào lý do chọn dịch vụ và tình huống sử dụng thay vì ghi nhớ tên dịch vụ một cách rời rạc.

### Ứng dụng vào học tập và công việc

- **Xác định SLI, SLO và SLA**: Chọn các chỉ số phản ánh đúng chất lượng dịch vụ, đặt mục tiêu nội bộ và xác định cam kết phù hợp với khách hàng.

- **Thiết kế dashboard từ trên xuống**: Bắt đầu từ customer experience và business metrics, sau đó liên kết xuống application, infrastructure và cloud provider metrics.

- **Xây dựng quy trình phản ứng**: Kết nối alarm với SNS hoặc kênh thông báo phù hợp, chỉ định người phụ trách và chuẩn bị SOP khôi phục.

- **Thực hiện post-incident review**: Phân tích nguyên nhân, tín hiệu bị bỏ sót và những thay đổi cần thực hiện sau mỗi sự cố.

- **Đưa bảo mật vào quy trình phát triển**: Kiểm tra kiến trúc trước khi code, scan pull request và chỉ thực hiện active testing trong môi trường được ủy quyền.

- **Theo dõi giới hạn của Agent**: Kiểm soát phạm vi, thời gian chạy, cơ chế xác thực và ngữ cảnh nghiệp vụ trước khi tự động hóa kiểm thử.

- **Chuẩn bị Cloud Practitioner theo domain**: Học theo bốn miền kiến thức, luyện câu hỏi tình huống và tập trung vào shared responsibility, bảo mật, dịch vụ cốt lõi cùng quản lý chi phí.

### Trải nghiệm sự kiện

#### Kết nối SLA với trải nghiệm thực tế

- Nội dung giúp làm rõ rằng monitoring không chỉ dành cho hạ tầng mà phải đo được khả năng người dùng hoàn thành tác vụ.
- Mô hình kim tự tháp cung cấp một cách trực quan để liên kết trạng thái cloud với application, business và customer experience.

#### Góc nhìn mới về tự động hóa bảo mật

- Security Agent cho thấy AI Agent có thể tham gia từ giai đoạn thiết kế đến kiểm tra mã nguồn và pentest được ủy quyền.
- Phần giới hạn nhấn mạnh rằng tự động hóa vẫn cần phạm vi rõ ràng, giám sát của con người và sự hiểu biết về nghiệp vụ.

#### Định hướng học AWS rõ ràng hơn

- Cấu trúc bốn domain giúp người mới xác định phạm vi kiến thức cần chuẩn bị cho AWS Cloud Practitioner.
- Chứng chỉ được nhìn nhận như bước xây dựng nền tảng, không phải mục tiêu thay thế kinh nghiệm thực hành.

#### Một số hình ảnh sự kiện

![Event 2](<../../../images/4-Event/Event2.png>)