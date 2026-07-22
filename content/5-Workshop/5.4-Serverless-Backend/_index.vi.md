---
title : "Xây dựng API Backend"
date : 2026-07-01 
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

Trong phần này, chúng ta sẽ xây dựng "bộ não" của hệ thống Đặt xe bằng cách sử dụng **AWS Lambda** để xử lý logic nghiệp vụ và **Amazon API Gateway** để tạo các điểm neo (Endpoints) cho phép giao diện người dùng giao tiếp với Backend.

#### Bước 1: Tạo các hàm AWS Lambda
AWS Lambda cho phép bạn chạy mã mà không cần cung cấp hay quản lý máy chủ. Chúng ta sẽ tạo các hàm bằng môi trường Node.js.

1. Truy cập dịch vụ **AWS Lambda** trên AWS Management Console.
2. Bấm **Create function** (Tạo hàm).
3. Chọn **Author from scratch** (Tạo từ đầu).
4. **Function name:** Nhập `getTripsFunction` (Hàm lấy danh sách chuyến đi).
5. **Runtime:** Chọn **Node.js 18.x** (hoặc 20.x).

![Giao diện khởi tạo hàm AWS Lambda](/images/create-lambda.png)

6. Bấm **Create function**.
7. Tại mục **Code source**, bạn dán đoạn mã xử lý (mã kết nối để đọc dữ liệu từ DynamoDB) vào file `index.mjs` hoặc `index.js`, sau đó bấm **Deploy** để lưu lại.

*(Bạn cần lặp lại các bước trên để tạo thêm các hàm cốt lõi khác như `bookTicketFunction` để xử lý logic đặt vé).*

#### Bước 2: Tạo REST API với Amazon API Gateway
API Gateway sẽ đóng vai trò là "cửa ngõ" tiếp nhận yêu cầu (HTTP Requests) từ người dùng cuối và chuyển tiếp tới các hàm Lambda tương ứng.

1. Truy cập dịch vụ **API Gateway**.
2. Tìm mục **REST API** (đảm bảo không chọn loại Private) và bấm **Build**.
3. Chọn **New API**, nhập **API name** là `CarBookingAPI` và bấm **Create API**.
4. **Tạo Resource (Tài nguyên):**
   * Bấm nút **Create Resource**.
   * Resource Name: Nhập `trips` (Đường dẫn khi gọi API sẽ là `/trips`). Bấm Create.
5. **Tạo Method (Phương thức):**
   * Chọn resource `/trips` vừa tạo, bấm **Create Method**.
   * **Method type:** Chọn `GET`.
   * **Integration type:** Chọn **Lambda function**.
   * **Lambda function:** Gõ và chọn hàm `getTripsFunction` đã tạo ở Bước 1.
   * Bấm **Create method**.

*(Thực hiện tương tự để tạo resource `/tickets` với method `POST` và trỏ về hàm `bookTicketFunction`).*

![Cấu hình Routes và Methods trên API Gateway](/images/api-gateway.png)

#### Bước 3: Triển khai API (Deploy API)
Để ứng dụng Frontend có thể gọi được các API này qua mạng Internet, bạn bắt buộc phải triển khai chúng.

1. Chọn nút **Deploy API** ở góc trên.
2. **Stage:** Chọn `*New stage*`, đặt tên Stage name là `prod` (Production).
3. Bấm **Deploy**.
4. Hệ thống sẽ cung cấp cho bạn một đường link **Invoke URL** (ví dụ: `https://xyz.execute-api.us-east-1.amazonaws.com/prod`). 

![Lấy đường link Invoke URL cung cấp cho Frontend](/images/api-invoke-url.png)