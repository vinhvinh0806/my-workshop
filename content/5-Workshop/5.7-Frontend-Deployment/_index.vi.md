---
title : "Triển khai Frontend"
date : 2026-07-01 
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

Sau khi Backend đã hoàn thiện, bước tiếp theo là đưa giao diện người dùng (Frontend - ReactJS) lên Internet. Chúng ta sẽ sử dụng **AWS Amplify** để tự động hóa quá trình lấy code từ GitHub, đóng gói (Build) và lưu trữ (Hosting) ứng dụng với một đường dẫn URL công khai.

#### Bước 1: Cấu hình biến môi trường (.env)
Trước khi đưa code lên GitHub, ứng dụng ReactJS cần biết địa chỉ của Backend để giao tiếp.

1. Mở thư mục mã nguồn Frontend trên Visual Studio Code.
2. Tìm (hoặc tạo) file `.env` ở thư mục gốc.
3. Cập nhật các giá trị sau dựa vào những tài nguyên bạn đã tạo ở các bước trước:
   * `VITE_API_GATEWAY_URL`: Đường link Invoke URL của API Gateway (tạo ở phần 5.4).
   * `VITE_COGNITO_USER_POOL_ID`: ID của Cognito User Pool (tạo ở phần 5.5).
   * `VITE_COGNITO_CLIENT_ID`: App Client ID của Cognito.

#### Bước 2: Đẩy mã nguồn lên GitHub (Push Code)
Đảm bảo bạn đã lưu toàn bộ thay đổi và đẩy mã nguồn lên kho lưu trữ GitHub cá nhân.

```bash
git add .
git commit -m "Update environment variables for production"
git push origin staging
```

#### Bước 3: Triển khai trên AWS Amplify 

AWS thường xuyên cập nhật giao diện, do đó quá trình cấu hình triển khai hiện tại sẽ bao gồm 4 bước trực quan sau:

1. **Choose source code provider:** Truy cập AWS Amplify, chọn **Create new app**. Tại màn hình đầu tiên, chọn **GitHub** làm nhà cung cấp mã nguồn và bấm Next.
![Chọn nhà cung cấp GitHub](/images/amplify-new-step1.png)

2. **Add repository and branch:** Cấp quyền truy cập GitHub, sau đó chọn kho lưu trữ chứa mã nguồn (`TangQuocHungg/DoAnDatVeBus`) và nhánh cần triển khai (ví dụ: `main`).
![Chọn Repo và Nhánh](/images/amplify-new-step2.png)

3. **App settings:** Thiết lập cấu hình đóng gói. Đảm bảo Amplify nhận diện đúng thư mục đầu ra (Build output directory) là `dist` và lệnh build phù hợp với ReactJS/Vite.
![Cấu hình App Settings](/images/amplify-new-step3.png)

4. **Review:** Xem lại toàn bộ các thông tin đã thiết lập. Nếu mọi thứ đã chính xác, bấm **Save and deploy** để hệ thống tự động kích hoạt quy trình CI/CD.
![Review và Deploy](/images/amplify-new-step4.png)

#### Bước 4: Kiểm tra kết quả
AWS Amplify sẽ bắt đầu quy trình CI/CD bao gồm 3 bước: **Provision** (Cấp phát) -> **Build** (Đóng gói) -> **Deploy** (Triển khai). Quá trình này mất khoảng 2-3 phút. 

Khi trạng thái chuyển sang **Deployed** (tích xanh), Amplify sẽ cung cấp cho bạn một đường link HTTPS (ví dụ: `https://staging.xyz123.amplifyapp.com`). Bấm vào link đó, và trang web Đặt xe Serverless của bạn đã chính thức online!

![Giao diện triển khai CI/CD trên AWS Amplify](/images/amplify-deploy.png)