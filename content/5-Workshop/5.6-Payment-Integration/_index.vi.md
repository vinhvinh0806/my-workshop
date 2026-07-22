---
title : "Tích hợp Thanh toán VNPAY"
date : 2026-07-01 
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

Trong các ứng dụng thương mại điện tử thực tế, việc tích hợp cổng thanh toán là bước không thể thiếu. Ở dự án này, chúng ta sẽ kết nối hệ thống với cổng thanh toán **VNPAY**. Quá trình này đòi hỏi việc xử lý bảo mật cao (tạo chữ ký số) và hứng luồng dữ liệu trả về (Webhook/IPN) một cách tự động.

#### Bước 1: Tạo URL Thanh toán (Payment URL)
Khi người dùng bấm “Thanh toán”, hệ thống không được gọi VNPAY trực tiếp từ Frontend để tránh lộ mã Secret Key. Thay vào đó, Frontend sẽ gọi một hàm Lambda.

1. Tạo một hàm Lambda mới có tên `createPaymentUrlFunction`.
2. Hàm này sẽ nhận thông tin chuyến đi và số tiền từ Frontend.
3. Sử dụng mã `vnp_TmnCode` và `vnp_HashSecret` đã chuẩn bị ở phần 5.2, mã hóa các tham số và tạo ra chữ ký số (Secure Hash).
4. Hàm Lambda trả về một đường link URL hoàn chỉnh của VNPAY. Frontend sẽ chuyển hướng (redirect) người dùng sang trang thanh toán của VNPAY bằng URL này.

**Đoạn mã mẫu (Node.js) cho hàm `createPaymentUrlFunction`:**

```javascript
import crypto from 'crypto';

export const handler = async (event) => {
    try {
        const body = JSON.parse(event.body);
        const { amount, ticketId, bankCode } = body;
        const tmnCode = process.env.VNP_TMN_CODE;
        const secretKey = process.env.VNP_HASH_SECRET;
        
        // ... (Logic tạo chữ ký và URL thanh toán VNPAY) ...
        
        return {
            statusCode: 200,
            body: JSON.stringify({ paymentUrl: vnpUrl })
        };
    } catch (error) {
        return { statusCode: 500, body: JSON.stringify({ message: "Lỗi tạo URL thanh toán" }) };
    }
};
```

**⚠️ Lưu ý Bảo mật (Best Practices):** 
Tuyệt đối không lưu trực tiếp thông tin nhạy cảm vào mã nguồn. Chúng ta sẽ cấu hình 2 khóa bảo mật này vào phần **Environment variables** của hàm Lambda để đảm bảo an toàn tối đa cho hệ thống.

**Cách thiết lập Biến môi trường trên AWS Console:**
1. Trong màn hình cấu hình hàm `createPaymentUrlFunction`, chuyển sang tab **Configuration**.
2. Chọn mục **Environment variables** ở menu bên tay trái.
3. Bấm **Edit** và thêm hai biến `VNP_TMN_CODE`, `VNP_HASH_SECRET` cùng với giá trị tương ứng của bạn.
4. Bấm **Save** để lưu lại. Hệ thống sẽ mã hóa và bảo vệ các khóa này.

![Cấu hình biến môi trường VNPAY cho AWS Lambda](/images/vnpay-lambda-env.png)

#### Bước 2: Thiết lập Webhook IPN (Instant Payment Notification)
Sau khi khách hàng quét mã QR hoặc nhập thẻ ATM thành công, VNPAY cần báo lại cho hệ thống của chúng ta biết để cập nhật trạng thái "Đã thanh toán".

1. Tạo một hàm Lambda thứ hai mang tên `vnpayWebhookFunction`.
2. Hàm này có nhiệm vụ tiếp nhận dữ liệu (GET request) do hệ thống máy chủ của VNPAY tự động gọi sang.
3. Bên trong hàm, thực hiện xác thực lại chữ ký số (Checksum) một lần nữa để đảm bảo request này thực sự đến từ VNPAY chứ không phải hacker giả mạo.
4. Nếu hợp lệ, hàm sẽ kết nối vào **Amazon DynamoDB**, tìm mã vé (`ticketId`) tương ứng trong bảng `Tickets` và cập nhật trường trạng thái từ `PENDING` sang `SUCCESS`.
5. Cuối cùng, phơi bày hàm này ra Internet thông qua một Endpoint của **API Gateway** (ví dụ: `GET /api/v1/payments/vnpay_ipn`) và khai báo đường link này trong trang quản trị Merchant của VNPAY.

![Chọn phương thức thanh toán VNPAY](/images/vnpay-checkout1.png)

![Giao diện nhập thẻ Test VNPAY](/images/vnpay-checkout2.png)

![Thông báo thanh toán thành công](/images/payment.png)

![Trạng thái vé đã được cập nhật thành công](/images/my_ticket.png)