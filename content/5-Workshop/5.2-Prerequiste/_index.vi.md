---
title : "Các bước chuẩn bị"
date : 2026-07-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

Trước khi bắt tay vào xây dựng hệ thống trên AWS, bạn cần đảm bảo đã chuẩn bị đầy đủ các công cụ và thông tin cấu hình sau đây.

#### 1. Yêu cầu về Tài khoản và Phần mềm
Để thực hành theo tài liệu này, máy tính của bạn cần được cài đặt sẵn các môi trường sau:
* **Tài khoản AWS:** Một tài khoản AWS đang hoạt động. Khuyến khích sử dụng tài khoản mới để tận dụng gói Free Tier.
* **Node.js:** Phiên bản 18.x trở lên. Đây là môi trường runtime để chạy mã nguồn Frontend (ReactJS) và Backend (AWS Lambda).
* **Git:** Dùng để clone mã nguồn mẫu và kết nối lưu trữ với AWS Amplify.
* **Code Editor:** Khuyên dùng Visual Studio Code.
* **Trình duyệt Web:** Google Chrome hoặc Firefox.

#### 2. Chuẩn bị Môi trường Thanh toán (VNPAY Sandbox)
Hệ thống Đặt xe yêu cầu tích hợp thanh toán trực tuyến. Chúng ta sẽ sử dụng môi trường thử nghiệm (Sandbox) của VNPAY.

**Bước 1:** Đăng ký tài khoản Sandbox
* Truy cập vào trang chủ dành cho nhà phát triển của VNPAY Sandbox: `https://sandbox.vnpayment.vn/devreg/`
* Điền đầy đủ thông tin để đăng ký một tài khoản Merchant (Nhà cung cấp).

![Đăng ký VNPAY Sandbox](/images/vnpay-sandbox2.png)

**Bước 2:** Lấy thông tin cấu hình (Credentials)
* Sau khi đăng ký thành công, VNPAY sẽ gửi một email chứa các thông tin kết nối quan trọng.
* Tại đây, bạn sẽ thấy 2 thông tin cực kỳ quan trọng cần lưu lại để sử dụng cho phần lập trình Backend AWS Lambda sau này:
    * `vnp_TmnCode` (Mã website)
    * `vnp_HashSecret` (Chuỗi bí mật dùng để tạo chữ ký số)

![Email thông tin cấu hình VNPAY](/images/vnpay-sandbox.png)
