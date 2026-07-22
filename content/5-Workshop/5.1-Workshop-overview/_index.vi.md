---
title : "Giới thiệu"
date : 2026-07-01 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu về Kiến trúc Serverless trên AWS

**Serverless (Máy chủ vô hình)** là một mô hình thực thi điện toán đám mây, trong đó nhà cung cấp đám mây (AWS) sẽ tự động quản lý việc phân bổ và cấp phép máy chủ một cách linh hoạt. 

Lợi ích cốt lõi của kiến trúc Serverless đối với các hệ thống thương mại điện tử (như đặt vé xe):
* **Không cần quản lý máy chủ:** Lập trình viên chỉ cần tập trung viết code (Business Logic) mà không cần quan tâm đến hệ điều hành hay bản vá bảo mật.
* **Tự động mở rộng (Auto-scaling):** Hệ thống có khả năng tự động xử lý hàng ngàn yêu cầu truy cập cùng lúc vào các dịp lễ Tết mà không bị sập (downtime).
* **Tối ưu chi phí (Pay-as-you-go):** Bạn chỉ phải trả tiền cho thời gian tính toán và số lượng yêu cầu thực tế phát sinh. Khi không có khách hàng, chi phí hạ tầng gần như bằng 0.

#### Tổng quan về Workshop

Trong Workshop này, bạn sẽ đóng vai trò là một Cloud Engineer để từng bước xây dựng hoàn chỉnh **Hệ thống Đặt xe trực tuyến (Serverless Car Booking)**. Dự án bao trùm toàn bộ vòng đời của một ứng dụng thực tế, từ khâu xác thực người dùng đến tích hợp cổng thanh toán bên thứ ba.

Hệ thống sẽ được chia thành các phân hệ chính:
1.  **Giao diện người dùng (Frontend):** Ứng dụng ReactJS (SPA) được tự động hóa CI/CD và lưu trữ (Hosting) trên **AWS Amplify**.
2.  **Lưu trữ dữ liệu (Database):** Cơ sở dữ liệu NoSQL tốc độ cao **Amazon DynamoDB** dùng để quản lý thông tin Người dùng, Chuyến đi và Vé xe.
3.  **Xử lý nghiệp vụ (Backend):** Các RESTful API được xây dựng bởi **Amazon API Gateway**, định tuyến luồng yêu cầu đến các hàm tính toán **AWS Lambda** (Node.js).
4.  **Bảo mật & Định danh (Security):** Quản lý luồng đăng nhập bằng **Amazon Cognito** và thiết lập nguyên tắc đặc quyền tối thiểu bằng **AWS IAM**.
5.  **Tích hợp thanh toán (Payment):** Xử lý giao dịch qua cổng **VNPAY** và cập nhật trạng thái tự động thông qua Webhook IPN.
6.  **Giám sát (Monitoring):** Lưu trữ log và theo dõi lỗi hệ thống bằng **Amazon CloudWatch**.

![Sơ đồ kiến trúc hệ thống Đặt xe Serverless](/images/so_do_kien_truc1.png)
