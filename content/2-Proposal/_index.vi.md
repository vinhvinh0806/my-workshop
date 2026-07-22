---
title: "Bản đề xuất"
date: 2026-05-15
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Tại phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn dự tính sẽ làm.

# Serverless Car Booking System
## Giải pháp AWS Serverless toàn diện cho quy trình đặt vé và thanh toán trực tuyến tự động

### 1. Tóm tắt điều hành 
Dự án "Hệ thống Đặt xe trực tuyến" được thiết kế nhằm mục đích số hóa và tự động hóa toàn bộ quy trình đặt vé xe khách và thanh toán trực tuyến. Ứng dụng được xây dựng theo kiến trúc Microservices/Serverless trên nền tảng điện toán đám mây AWS, giúp hệ thống có khả năng tự động mở rộng (auto-scaling) không giới hạn khi lượng truy cập tăng cao vào các dịp lễ Tết, đồng thời tối ưu hóa chi phí vận hành (pay-as-you-go). Hệ thống tích hợp trực tiếp cổng thanh toán VNPAY, cho phép người dùng đặt chỗ và thanh toán 24/7 một cách an toàn và tiện lợi, được bảo vệ bởi Amazon Cognito.

### 2. Tuyên bố vấn đề 
**Vấn đề hiện tại** 
Nhiều nhà xe hiện nay vẫn sử dụng quy trình đặt vé thủ công qua điện thoại hoặc tin nhắn, dẫn đến tình trạng quá tải, sai sót dữ liệu (trùng ghế) và tốn nhiều nguồn lực nhân sự. Khách hàng gặp khó khăn trong việc theo dõi chuyến đi, sơ đồ ghế trống theo thời gian thực và phương thức thanh toán chủ yếu là tiền mặt hoặc chuyển khoản thủ công không tự động xác nhận. Các hệ thống máy chủ truyền thống (On-premise/VPS) thường xuyên bị sập (downtime) khi lượng truy cập tăng vọt đột biến.

**Giải pháp** 
Phát triển một ứng dụng web Single Page Application (SPA) bằng ReactJS cung cấp giao diện trực quan cho phép khách hàng tự chọn chuyến đi và ghế ngồi. Nền tảng sử dụng kiến trúc AWS Serverless: Amazon API Gateway và AWS Lambda (Node.js) để xử lý logic nghiệp vụ; Amazon DynamoDB (NoSQL) để lưu trữ trạng thái ghế ngồi và vé theo thời gian thực. Việc thanh toán được xử lý thông qua API của VNPAY, sau đó phản hồi (Webhook) sẽ được gửi về AWS Lambda để tự động cập nhật trạng thái vé. Quản trị CI/CD và lưu trữ giao diện được thực hiện thông qua AWS Amplify.

**Lợi ích và hoàn vốn đầu tư (ROI)** 
Giải pháp loại bỏ hoàn toàn các thao tác thủ công của nhân viên điều hành, giúp phục vụ lượng lớn khách hàng cùng lúc mà không làm tăng chi phí nhân sự. Việc sử dụng kiến trúc Serverless đảm bảo chi phí hạ tầng trong những tháng thấp điểm gần như bằng 0 (chỉ trả tiền khi có request phát sinh). Khách hàng có trải nghiệm mượt mà, chuyên nghiệp hơn, từ đó tăng tỷ lệ chuyển đổi và doanh thu. Việc tích hợp sẵn thanh toán không tiền mặt giúp dòng tiền xoay vòng nhanh chóng và minh bạch.

### 3. Kiến trúc giải pháp 
Nền tảng áp dụng kiến trúc AWS Serverless để tách biệt hoàn toàn Frontend và Backend. Dữ liệu phi cấu trúc và tốc độ cao được quản lý bởi DynamoDB. Luồng bảo mật được kiểm soát khép kín từ người dùng cuối đến cơ sở dữ liệu thông qua Cognito, API Gateway Authorizer và IAM Role.

![Serverless Car Booking Architecture](/images/so_do_kien_truc1.png)

**Dịch vụ AWS sử dụng** 
*   **AWS Amplify**: Đóng gói, lưu trữ giao diện ReactJS/Vite và tự động hóa quy trình CI/CD từ GitHub.
*   **Amazon Cognito**: Quản lý vòng đời người dùng (đăng ký, đăng nhập) và cấp phát JWT Token bảo mật.
*   **Amazon API Gateway**: Xây dựng các RESTful API định tuyến request từ Frontend tới Backend, tích hợp Cognito Authorizer để chặn các luồng truy cập trái phép.
*   **AWS Lambda (Node.js)**: Chạy các đoạn mã xử lý nghiệp vụ cốt lõi (tạo vé, kiểm tra ghế trống) và xử lý Webhook IPN từ VNPAY mà không cần quản lý máy chủ.
*   **Amazon DynamoDB**: Lưu trữ cơ sở dữ liệu NoSQL tốc độ cao cho bảng Người dùng, Chuyến đi, và Vé xe.
*   **AWS IAM**: Phân quyền bảo mật đặc quyền tối thiểu (Least Privilege) cho các dịch vụ AWS giao tiếp với nhau.
*   **Amazon CloudWatch**: Thu thập log, giám sát độ trễ và thiết lập cảnh báo (Alarms) cho hệ thống.

### 4. Triển khai kỹ thuật 
**Các giai đoạn triển khai** 
Dự án được triển khai theo mô hình Agile, chia làm 4 giai đoạn chính:
1.  **Khởi tạo & Thiết kế**: Nghiên cứu tài liệu AWS Serverless, thiết kế UI/UX cơ bản, phác thảo sơ đồ cơ sở dữ liệu NoSQL (DynamoDB) và kiến trúc hệ thống.
2.  **Phát triển Frontend (Client-side)**: Xây dựng ứng dụng bằng ReactJS (Vite), thiết lập mock data, kiểm thử giao diện luồng đặt vé và triển khai lên AWS Amplify.
3.  **Phát triển Backend (Server-side)**: Viết các hàm Lambda bằng Node.js, cấu hình API Gateway và kết nối đọc/ghi dữ liệu với DynamoDB. Thiết lập Cognito để bảo vệ API.
4.  **Tích hợp & Vận hành**: Tích hợp SDK VNPAY, thiết lập Webhook. Thực hiện E2E Testing, theo dõi log trên CloudWatch và cấu hình bảo mật IAM hoàn chỉnh.

**Yêu cầu kỹ thuật** 
*   **Frontend**: Sử dụng ReactJS, Vite để tối ưu tốc độ build. Nắm vững cách quản lý state và gọi API (Axios/Fetch).
*   **Backend & Cloud**: Hiểu rõ cách viết mã bất đồng bộ (Async/Await) trong Node.js. Nắm vững khái niệm NoSQL (Partition Key, Sort Key) của DynamoDB. Có khả năng đọc hiểu tài liệu API (API Specification) của bên thứ ba (VNPAY) và tích hợp AWS SDK (v3).

### 5. Lộ trình & Mốc triển khai 
*   **Tuần 1 - Tuần 4**: Nghiên cứu AWS cơ bản, chốt đề tài, phác thảo giải pháp và viết Đề xuất dự án.
*   **Tuần 5 - Tuần 6**: Lập trình giao diện Frontend cục bộ (Local) và triển khai lên môi trường Cloud (ban đầu dùng S3, sau đó tối ưu bằng Amplify).
*   **Tuần 7**: Xây dựng hoàn chỉnh hạ tầng Serverless Backend (DynamoDB, Lambda, API Gateway) và tích hợp xác thực Amazon Cognito.
*   **Tuần 8 - Tuần 10**: Tích hợp VNPAY, cấu hình IAM Role, thực hiện kiểm thử toàn trình (E2E Testing) và rà soát log/debug qua CloudWatch.
*   **Tuần 11 - Tuần 12**: Hoàn thiện tài liệu kỹ thuật, chuẩn bị slide, bàn giao mã nguồn và báo cáo nghiệm thu dự án.

### 6. Ước tính ngân sách 
Nhờ tận dụng gói **AWS Free Tier** (Bậc miễn phí của AWS) cho các dịch vụ Serverless, chi phí duy trì hệ thống trong giai đoạn phát triển và vận hành quy mô nhỏ là cực kỳ thấp, gần như bằng 0.

**Chi phí hạ tầng (Ước tính hàng tháng cho lượng truy cập vừa)** 
*   **AWS Lambda**: 0,00 USD (Miễn phí 1 triệu yêu cầu và 400.000 GB-giây tính toán mỗi tháng).
*   **Amazon DynamoDB**: 0,00 USD (Miễn phí 25 GB dung lượng lưu trữ và 25 RCU/WCU mỗi tháng).
*   **Amazon API Gateway**: 0,00 USD (Miễn phí 1 triệu lệnh gọi API trong 12 tháng đầu).
*   **Amazon Cognito**: 0,00 USD (Miễn phí 50.000 Người dùng hoạt động hàng tháng - MAU).
*   **AWS Amplify**: ~0,50 USD (Tính phí băng thông truyền ra và thời gian build).
*   **Amazon CloudWatch**: 0,00 USD (Nằm trong giới hạn ghi log miễn phí hàng tháng).

*Tổng chi phí AWS ước tính: Dưới 1,00 USD/tháng trong năm đầu tiên.*
*Chi phí khác: Miễn phí (Sử dụng môi trường Sandbox của VNPAY để kiểm thử).*

### 7. Đánh giá rủi ro 
**Ma trận rủi ro** 
*   **Lỗi tích hợp Webhook VNPAY (mất kết nối mạng tạm thời)**: Ảnh hưởng cao, xác suất trung bình.
*   **Độ trễ khởi động của AWS Lambda (Cold Start)**: Ảnh hưởng trung bình, xác suất cao.
*   **Lỗi bảo mật API (bị gọi API rác/DDOS)**: Ảnh hưởng cao, xác suất thấp.

**Chiến lược giảm thiểu** 
*   **Webhook**: Xây dựng cơ chế kiểm tra chéo trạng thái đơn hàng (Polling/Cronjob) để đối soát với VNPAY nếu Webhook bị rớt.
*   **Độ trễ**: Cấu hình Provisioned Concurrency cho các hàm Lambda quan trọng hoặc tối ưu hóa kích thước thư viện Node.js tải lên.
*   **Bảo mật**: Sử dụng Cognito Authorizer bắt buộc xác thực JWT cho mọi API quan trọng, kết hợp thiết lập hạn mức tốc độ (Throttling) trên API Gateway.

### 8. Kết quả kỳ vọng 
*   **Cải tiến kỹ thuật**: Một hệ thống phần mềm điện toán đám mây hoàn chỉnh, tự động hóa từ khâu chọn ghế đến thanh toán không tiền mặt. Giải quyết triệt để bài toán mở rộng hệ thống (Scalability) và bảo mật dữ liệu khách hàng.
*   **Giá trị dài hạn**: Hệ thống có thể được đóng gói và thương mại hóa (SaaS) cho nhiều doanh nghiệp vận tải khác nhau. Cấu trúc mã nguồn mở gọn nhẹ dễ dàng nâng cấp hoặc thêm các module mới (như quản lý mã giảm giá voucher, chatbot AI hỗ trợ) trong tương lai.