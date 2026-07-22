---
title : "Nâng cấp Kiến trúc Hệ thống"
date : 2026-07-01 
weight : 9
chapter : false
pre : " <b> 5.9. </b> "
---

Để hệ thống Đặt xe Serverless tiến gần hơn với tiêu chuẩn của một ứng dụng thực tế quy mô lớn (Enterprise-ready), hệ thống đã được thiết kế sẵn không gian để tích hợp thêm các dịch vụ AWS bậc cao. Dưới đây là các thành phần mở rộng nhằm tối ưu hóa bảo mật, hiệu suất và trải nghiệm người dùng.

#### 1. Lưu trữ tĩnh với Amazon S3
Thay vì lưu trữ dữ liệu dạng Base64 hoặc URL ngoài, **Amazon S3** được sử dụng để làm kho lưu trữ chuyên dụng cho toàn bộ hình ảnh phương tiện (xe khách, nội thất) và tài liệu hệ thống. Điều này giúp giảm tải cho cơ sở dữ liệu DynamoDB và tối ưu băng thông tải trang.

![Lưu trữ hình ảnh trên Amazon S3](/images/s3-storage.png)

#### 2. Quản lý khóa bảo mật với AWS Secrets Manager
Các thông tin nhạy cảm như `vnp_HashSecret` và `vnp_TmnCode` của VNPAY tuyệt đối không được gán trực tiếp (hard-code) vào mã nguồn. Chúng được lưu trữ mã hóa an toàn trên **AWS Secrets Manager**, cho phép quản lý tập trung và kiểm soát quyền truy cập thông qua các chính sách IAM một cách nghiêm ngặt, ngăn chặn triệt để nguy cơ rò rỉ dữ liệu.

![Lưu trữ khóa thanh toán trên Secrets Manager](/images/secrets-manager.png)

#### 3. Hệ thống thông báo với Amazon SES
Để nâng cao trải nghiệm khách hàng, **Amazon SES (Simple Email Service)** được đưa vào kiến trúc. Bất cứ khi nào Webhook của VNPAY xác nhận thanh toán thành công, hàm Lambda sẽ tự động kích hoạt SES để gửi ngay một email hóa đơn điện tử và mã vé (QR Code) đến hòm thư của người dùng.

![Xác thực danh tính gửi email trên Amazon SES](/images/Ses_email.png)

#### 4. Tường lửa Ứng dụng Web (AWS WAF)
Để bảo vệ hệ thống khỏi các đợt tấn công từ chối dịch vụ (DDoS) hoặc dò quét tự động (Botnet), một bộ quy tắc bảo mật **AWS WAF** (Protection Pack) đã được cấu hình thành công. Theo chuẩn kiến trúc AWS dành cho HTTP API, bộ quy tắc này được thiết kế sẵn sàng để tích hợp cùng mạng phân phối nội dung Amazon CloudFront. Khi đó, WAF sẽ đóng vai trò như một màng lọc toàn cầu, từ chối ngay lập tức các IP có hành vi bất thường trước khi lưu lượng kịp chạm đến Backend.

![Thiết lập Tường lửa WAF cho API Gateway](/images/waf_firewall.png)