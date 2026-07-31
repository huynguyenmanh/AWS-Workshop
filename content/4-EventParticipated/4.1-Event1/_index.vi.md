---
title: "Event 1"
date: 2024-06-06
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch “Modern Cloud Infrastructure Engineering”

### Mục Đích Của Sự Kiện

- Chia sẻ lộ trình phát triển sự nghiệp từ IT Helpdesk lên Senior Sysadmin.

- Trang bị kiến thức nền tảng và thực hành về công nghệ đóng gói ứng dụng với Docker.

- Giới thiệu giải pháp kiến trúc Serverless cho game realtime.

- Phân tích giải pháp an ninh mạng kết hợp AWS WAF vàMachine Learning NIDS.

- Hướng dẫn xây dựng hệ thống GraphRAG với Amazon Neptune và Amazon Bedrock.

- Chia sẻ nghệ thuật làm việc nhóm hiệu quả, ứng dụng công cụ trong quản lý công việc.

### Danh Sách Diễn Giả

- **Tran Trung Vinh** - System Administrator tại Central Retail Group

- **Bao Huynh** - Junior Cloud Native Developer tại Endava Vietnam, Founder / Head Lab - ITea Lab

- **Lê Hoàng Gia Đại** -  AWS Cloud Engineer &
Cyber Security Engineer

- **Viet Phat** - AI Major tại Swinburne University of Technology
- **Nguyen Quoc Bao** - Diễn giả
- **Truong Huy Phuoc** - Diễn giả 

### Nội Dung Nổi Bật

#### Hành Trình Phát Triển Sự Nghiệp Từ IT Helpdesk Đến Senior Sysadmin

- **Lộ trình thực tế**: Không bắt buộc xuất phát từ các trường đại học danh tiếng. Quá trình đi từ IT Helpdesk → Junior Sysadmin → Senior Sysadmin đòi hỏi tích lũy liên tục kiến thức thực tế, tự học và hiểu sâu về hạ tầng.
- **Tư duy vận hành & xử lý sự cố**: Đảm bảo tính sẵn sàng của hệ thống, xử lý sự cố khẩn cấp và thiết lập giám sát với Kubernetes, Grafana/Prometheus.
- **Kinh nghiệm phỏng vấn MNCs**: Nhấn mạnh dự án thực tế, năng lực thiết kế kiến trúc, kịch bản ứng phó sự cố, kỹ năng tìm và xử lý sự cố.
- **Lời khuyên**: Đào sâu 1–2 kỹ năng cốt lõi, ưu tiên xây dựng portfolio thực tế hơn chứng chỉ và phải kiên trì.

#### Nền Tảng Containerization Và Thực Hành Với Docker

- **Virtualization vs. Containerization**: So sánh sự khác biệt giữa **Ảo hóa truyền thống** sử dụng Hypervisor, mỗi VM chạy một OS riêng với **Đóng gói ứng dụng** chia sẻ OS kernel, nhẹ hơn, khởi động cực nhanh và tối ưu tài nguyên.
- Nắm chắc các  khái niệm **thành phần cốt lõi của Docker**: 
    - **Docker Image** - bản đóng gói tĩnh
    - **Docker Container** - thể hiện thực thi của Image
    - **Dockerfile** - kịch bản đóng gói
    - **Docker Volume** - lưu trữ dữ liệu bền vững
    - **Docker Network** - kết nối giữa các container
- **Hệ thống lệnh CLI**: giới thiệu các nhóm lệnh quan trọng giúp quản lý đa container dễ dàng như `docker run`, `exec, logs`, `stop`, `build`, `pull`, `push`, `docker-compose up`, `logs`, `build`
- **Ứng dụng thực tế**: Sử dụng rộng rãi trong CI/CD pipelines, kiến trúc Microservices, ứng dụng Cloud-native và legacy modernization.

#### Kiến Trúc Game Multiplayer Serverless Với AWS WebSockets Và Godot

- **Mô hình kết nối UDP/ENet**: độ trễ siêu thấp, thích hợp cho game FPS/Racing.
- **Mô hình kết nối HTTP Polling**: đơn giản, độ trễ cao, tốn tài nguyên.
- **Mô hình kết nối WebSocket**: kết nối hai chiều full-duplex, tin cậy, lý tưởng cho turn-based game, lobby, chat.
- **Serverless trên AWS**: Game Client → API Gateway WebSocket → AWS Lambda → Amazon DynamoDB.
- Đưa ra các so sánh về mô hình WebSocket kết hợp Lambda với AWS GameLift khi phát triển các dòng game có nhu cầu tính toán vật lý theo thời gian thực cao.

#### Phát Hiện Tấn Công Mạng Kết Hợp AWS WAF Và Machine Learning NIDS

- **Giới hạn của WAF**: bảo vệ tầng L7 (HTTP/HTTPS) chống SQL Injection, XSS, Bot traffic hiệu quả nhưng không đủ để phát hiện các cuộc tấn công Zero-day hoặc hành vi bất thường phức tạp.
- **ML-based NIDS:**: Hệ thống phát hiện xâm nhập mạng (NIDS) dựa trên Machine Learning trained giúp nhận dạng các dạng tấn công phức tạp như DoS/DDoS, Brute Force, FTP/SSH attacks.
- Nên phân tích luồng dữ liệu thực tế bằng ML, đối chiếu sự kiện NIDS và AWS WAF trên Dashboard thời gian thực để nâng cao khả năng giám sát và ứng phó tự động.
- Độ chính xác của ML phụ thuộc vào chất lượng dữ liệu và xử lý Class Imbalance; kết hợp WAF cùng ML sẽ giúp tối ưu hóa mô hình bảo mật nhiều lớp.

#### Xây Dựng Ứng Dụng GraphRAG Hiện Đại Với Amazon Bedrock Và Amazon Neptune

- **Hạn chế**: RAG truyền thống dựa trên Vector Search và Text Chunks → khó khăn khi xử lý các câu hỏi phức tạp hay yêu cầu truy vấn đa liên kết và dễ gây ra hiện tượng Hallucination.
- **GraphRAG**: Tích hợp Đồ thị tri thức - Knowledge Graph giúp LLM hiểu sâu ngữ cảnh liên kết và tăng độ chính xác câu trả lời.
- **Kiến trúc trên AWS**: Kết hợp Amazon Bedrock với Amazon Neptune/Neptune Analytics.
- **Mô hình triển khai**: Fully Managed với Bedrock + Neptune Analytics hoặc Open-source toolkit với LlamaIndex + Custom routes.

#### Nghệ Thuật Làm Việc Nhóm Hiệu Quả Và Quy Trình Phối Hợp
"Many hands make light work" - hiệu quả làm việc nhóm vượt trội so với nỗ lực cá nhân đơn lẻ nhờ sự hiệp lực và chia sẻ khối lượng công việc.
- 4 Quy tắc vàng trong Teamwork:
    - **Clear & Shared Goals**: Mục tiêu chung rõ ràng và thống nhất.
    - **Right Person, Right Place**: Phân công đúng người, đúng việc dựa trên năng lực.
    - **Open Communication & Active Listening**: Giao tiếp cởi mở và lắng nghe tích cực.
    - **Personal Accountability**: Tinh thần trách nhiệm cá nhân đối với công việc được giao.
- **Ứng dụng công cụ vào quy trình**: 
    - Quản lý mã nguồn và luồng công việc qua GitLab như Merge Requests, Code Review
    - Công cụ quản lý dự án ClickUp 
    - Các kênh giao tiếp tự động như Discord/ClickUp Notificationsifications

### Những Gì Học Được

#### Tư Duy Kiến Trúc & Kỹ Thuật System Design

- **Kiến trúc Serverless Realtime**: Phối hợp API Gateway WebSocket, Lambda và DynamoDB để xây dựng hệ thống giao tiếp hai chiều tối ưu chi phí.
- **GraphRAG & Knowledge Graph**: Cách ết hợp Vector DB và Graph DB (Amazon Neptune) nâng cao khả năng suy luận đa bước cho LLM.
- **Đóng gói ứng dụng**:  Hiểu rõ bản chất ảo hóa cấp HĐH của Docker giúp xây dựng các môi trường phát triển nhất quán và dễ mở rộng.

#### Tư Duy An Ninh Mạng & Vận Hành Hệ Thống

- **Bảo mật đa lớp** Kết hợp WAF (L7) và Machine Learning NIDS mang lại cơ chế phòng thủ chủ động trước hành vi bất thường.
- **Tư duy SysAdmin & Troubleshooting**: Hiểu sâu bản chất hạ tầng, quản lý sự cố chủ động và thiết lập giám sát liên tục.
- **Tự động hóa & Tối ưu nguồn lực**: Ưu tiên tự động hóa CI/CD, đóng gói ứng dụng bằng Docker và tối ưu chi phí database.

#### Quy Trình Phối Hợp

- **4 Quy tắc vàng trong Teamwork**: Mục tiêu rõ ràng, đúng người đúng việc, giao tiếp cởi mở và trách nhiệm cá nhân.
- **Định hướng sự nghiệp**: Tăng trưởng bền vững bằng kỹ năng cốt lõi và sản phẩm thực tế, tránh chạy theo số lượng chứng chỉ.

### Ứng Dụng Vào Công Việc

- Adopt Docker Containerization: Đóng gói microservices bằng Dockerfile và Docker Compose để chuẩn hóa môi trường Dev/Test/Prod.

- Implement WebSocket Serverless: Dùng API Gateway WebSocket và AWS Lambda xây dựng tính năng realtime (chat, phòng chơi).

- Integrate AWS WAF & ML NIDS: Cấu hình WAF Web ACLs kết hợp mô hình ML phát hiện xâm nhập bất thường bảo vệ hệ thống.

- Deploy GraphRAG with Amazon Bedrock: Kết hợp Amazon Neptune Analytics và Bedrock giải quyết bài toán tra cứu tài liệu doanh nghiệp phức tạp.

- Apply 4 Golden Rules of Teamwork: Quản lý Merge Request trên GitLab, theo dõi tiến độ qua ClickUp và giữ trách nhiệm cá nhân.

- Focus on Core SysAdmin/DevOps Skills: Thực hành lab hạ tầng, rèn kỹ năng troubleshooting và hoàn thiện portfolio thực tế.

### Trải nghiệm trong event


#### Học hỏi từ các diễn giả có chuyên môn cao
- Diễn giả từ Central Retail Group, Endava, Swinburne University mang đến các góc nhìn thực chiến đa dạng.
- Nội dung chia sẻ thực tế, bao phủ từ kỹ thuật chuyên sâu (WAF, ML NIDS, WebSockets, GraphRAG) đến trải nghiệm nghề nghiệp.

#### Trải nghiệm kỹ thuật thực tế
- Nắm rõ kịch bản triển khai Serverless WebSocket với Godot Engine và cách xử lý đứt kết nối client.
- Hiểu cách kết hợp ML với AWS WAF bảo vệ hệ thống và ứng dụng GraphRAG cho AI hiện đại.


#### Kết nối và trao đổi
- Không khí cởi mở giúp kết nối sinh viên, lập trình viên và các chuyên gia trong ngành.
- Củng cố tinh thần gắn kết nhờ 4 quy tắc vàng trong teamwork và các công cụ phối hợp số.

#### Bài học rút ra
- VKiến thức nền tảng (Linux, Networking, Docker, Cloud) và tư duy giải quyết vấn đề là tài sản cốt lõi.
- Công nghệ mới (AI/ML, GraphRAG, Serverless) cần đi đôi với tư duy an ninh mạng và kỹ năng teamwork.

#### Một số hình ảnh khi tham gia sự kiện
* Thêm các hình ảnh của các bạn tại đây
> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn giúp tôi thay đổi cách tư duy về thiết kế ứng dụng, hiện đại hóa hệ thống và phối hợp hiệu quả hơn giữa các team.
