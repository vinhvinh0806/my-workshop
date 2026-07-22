---
title : "Khởi tạo Cơ sở dữ liệu"
date : 2026-07-01 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

Trong phần này, chúng ta sẽ thiết lập cơ sở dữ liệu cho hệ thống Đặt xe trực tuyến. Vì ứng dụng sử dụng kiến trúc Serverless, **Amazon DynamoDB** là sự lựa chọn hoàn hảo nhờ khả năng tự động mở rộng và tốc độ phản hồi tính bằng mili-giây mà không cần phải quản lý máy chủ ảo.

Hệ thống của chúng ta sẽ cần 3 bảng dữ liệu chính:
1. **Users:** Lưu trữ thông tin khách hàng.
2. **Trips:** Lưu trữ danh sách các chuyến xe (điểm đi, điểm đến, thời gian, giá vé).
3. **Tickets:** Lưu trữ thông tin vé xe đã đặt và trạng thái thanh toán.

#### Các bước tạo bảng trên DynamoDB

**Bước 1: Truy cập dịch vụ DynamoDB**
* Đăng nhập vào [AWS Management Console](https://console.aws.amazon.com/).
* Đảm bảo bạn đang ở Region **us-east-1 (N. Virginia)** (kiểm tra góc trên cùng bên phải).
* Trên thanh tìm kiếm, gõ **DynamoDB** và chọn dịch vụ.

**Bước 2: Tạo bảng Users**
* Trong menu bên trái, chọn **Tables** và bấm nút **Create table** (Tạo bảng màu cam).
* **Table name:** Nhập `Users`
* **Partition key:** Nhập `userId` (Kiểu dữ liệu: **String**)
* **Table settings:** Chọn **Default settings** (Cài đặt mặc định).
* Bấm **Create table** ở cuối trang và đợi khoảng vài giây để bảng chuyển sang trạng thái *Active*.
![Giao diện nhập liệu tạo bảng DynamoDB](/images/create-dynamodb-table.png)

**Bước 3: Tạo bảng Trips**
* Lặp lại thao tác tạo bảng.
* **Table name:** Nhập `Trips`
* **Partition key:** Nhập `tripId` (Kiểu dữ liệu: **String**)
* Bấm **Create table**.

**Bước 4: Tạo bảng Tickets**
* Lặp lại thao tác tạo bảng một lần nữa.
* **Table name:** Nhập `Tickets`
* **Partition key:** Nhập `ticketId` (Kiểu dữ liệu: **String**)
* Bấm **Create table**.

Sau khi hoàn tất sẽ thấy danh sách 3 bảng vừa tạo có trạng thái **Active** như hình bên dưới, sẵn sàng để kết nối với các hàm AWS Lambda ở phần tiếp theo.

![Danh sách các bảng DynamoDB](/images/dynamodb-tables.png)