---
title : "Dọn dẹp tài nguyên"
date : 2026-07-10 
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---

Chúc mừng bạn đã xây dựng thành công **Hệ thống Đặt xe trực tuyến Serverless**! 

Để tránh phát sinh chi phí không mong muốn ngoài gói Free Tier sau khi kết thúc bài thực hành, bạn cần dọn dẹp và xóa toàn bộ các tài nguyên AWS đã tạo theo đúng thứ tự dưới đây.

#### Bước 1: Xóa Frontend (AWS Amplify)
1. Truy cập dịch vụ **AWS Amplify**.
2. Chọn ứng dụng `CarBookingClient` (hoặc tên ứng dụng bạn đã đặt).
3. Ở menu bên trái, mở rộng mục **App settings** -> Chọn **General**.
4. Kéo xuống dưới cùng, bấm vào nút **Delete app** màu đỏ.
5. Gõ chữ `delete` để xác nhận và tiến hành xóa.

![Xóa ứng dụng AWS Amplify](/images/Clean-up-AWS%20Amplify.png)

#### Bước 2: Xóa API (Amazon API Gateway)
1. Truy cập dịch vụ **API Gateway**.
2. Tìm và chọn API mang tên `CarBookingAPI` (hoặc DatXe-API).
3. Bấm nút **Delete** và xác nhận. Việc này sẽ gỡ bỏ điểm cuối giao tiếp công khai của Backend.

![Xóa Amazon API Gateway](/images/Clean-up-API%20Gateway.png)

#### Bước 3: Xóa Logic nghiệp vụ (AWS Lambda)
1. Truy cập dịch vụ **AWS Lambda** -> chọn **Functions**.
2. Tích chọn tất cả các hàm bạn đã tạo trong dự án này (ví dụ: `getTripsFunction`, `bookTicketFunction`, `createPaymentUrlFunction`, `vnpayWebhookFunction`...).
3. Bấm nút **Actions** -> Chọn **Delete**.

![Xóa hàm AWS Lambda](/images/Clean-up-AWS%20Lambda.png)

#### Bước 4: Xóa hệ thống xác thực (Amazon Cognito)
1. Truy cập dịch vụ **Amazon Cognito** -> chọn **User pools**.
2. Chọn `CarBookingUserPool`.
3. Bấm nút **Delete user pool**. Hệ thống sẽ yêu cầu bạn nhập tên của User Pool để xác nhận hành động xóa.

![Xóa Amazon Cognito User Pool](/images/Clean-up-Amazon%20Cognito.png)

#### Bước 5: Xóa Cơ sở dữ liệu (Amazon DynamoDB)
1. Truy cập dịch vụ **Amazon DynamoDB** -> chọn **Tables**.
2. Tích chọn lần lượt 3 bảng: `Users`, `Trips` (hoặc `CarTrips`), và `Tickets`.
3. Bấm nút **Delete** cho từng bảng. Bỏ chọn mục sao lưu (backup) nếu hệ thống hỏi để tránh tốn thêm dung lượng lưu trữ.

![Xóa bảng Amazon DynamoDB](/images/Clean-up-DynamoDB.png)

#### Bước 6: Xóa Logs (Amazon CloudWatch) - Tùy chọn
Dù dung lượng log không đáng kể, nhưng để dọn sạch sẽ 100%:
1. Truy cập **CloudWatch** -> chọn **Log groups**.
2. Xóa tất cả các nhóm log bắt đầu bằng `/aws/lambda/` liên quan đến các hàm của bạn.

![Xóa logs trên Amazon CloudWatch](/images/Clean-up-CloudWatch.png)