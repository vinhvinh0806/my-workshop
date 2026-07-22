---
title : "Thiết lập Bảo mật & Xác thực"
date : 2026-07-01 
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

Bảo mật là ưu tiên hàng đầu trong mọi hệ thống trên AWS. Trong phần này, chúng ta sẽ thiết lập hàng rào bảo mật nhiều lớp: Quản lý danh tính người dùng bằng **Amazon Cognito**, bảo vệ API bằng **Authorizer**, và phân quyền nội bộ bằng **AWS IAM**.

#### Bước 1: Quản lý Người dùng với Amazon Cognito
Thay vì tự xây dựng hệ thống đăng nhập mã hóa mật khẩu phức tạp, chúng ta sẽ dùng Cognito User Pool.

1. Truy cập dịch vụ **Amazon Cognito** và bấm **Create user pool**.
2. **Application type:** Chọn **Single-page application (SPA)** vì Frontend của chúng ta được xây dựng bằng ReactJS.
3. **Sign-in options:** Chọn **Email** (Người dùng sẽ đăng nhập bằng email).
4. **Password policy:** Chọn chế độ mặc định của Cognito (yêu cầu chữ hoa, chữ thường, số và ký tự đặc biệt).
5. **MFA and verification:** Tùy chọn tắt MFA để dễ dàng test trong môi trường lab. Chọn xác minh email tự động (Send email message).
6. Đặt tên User pool là `CarBookingUserPool` và tạo **App client** tên là `CarBookingClient`.
7. Bấm **Create user pool**. Hãy ghi lại **User Pool ID** và **Client ID** để dùng cho cấu hình Frontend.

![Cấu hình Single-page application và Email Sign-in](/images/cognito-setup.png)

Sau khi khởi tạo thành công, giao diện Overview của Cognito sẽ hiển thị thông tin tổng quan về User Pool của bạn.

![Tổng quan Cognito User Pool đã có người dùng đăng ký](/images/cognito-security.png)

#### Bước 2: Khóa API bằng JWT Authorizer
Chúng ta không muốn bất kỳ ai cũng có thể gọi API đặt vé `/dat-xe`. Chỉ những người dùng đã đăng nhập (có token hợp lệ) mới được phép gọi.

1. Quay lại **API Gateway**, chọn API của bạn.
2. Tìm đến mục **Authorization** (hoặc Authorizers).
3. Bấm **Create authorizer**, chọn loại là **JWT** (hoặc Cognito tùy giao diện API).
4. Chọn User Pool `CarBookingUserPool` vừa tạo ở Bước 1.
5. Sau khi tạo xong, quay lại tab **Routes**, chọn route `POST /api/v1/dat-xe`.
6. Bấm **Attach authorization** và chọn Authorizer vừa tạo. Giờ đây, API này đã được khóa an toàn!

![Gắn Authorizer khóa API đặt vé an toàn](/images/jwt-authorizer.png)

#### Bước 3: Phân quyền bằng AWS IAM (Principle of Least Privilege)
Các hàm Lambda không thể tự nhiên đọc/ghi dữ liệu vào DynamoDB nếu không được cấp phép.

1. Truy cập dịch vụ **AWS IAM** -> **Roles**.
2. Tìm Role thực thi (Execution Role) được tạo tự động cho hàm `bookTicketFunction`.
3. Bấm **Add permissions** -> **Attach policies**.
4. Tìm và gắn policy `AmazonDynamoDBFullAccess` (hoặc cấu hình Custom Policy chỉ cho phép thao tác trên các bảng cụ thể để bảo mật hơn).