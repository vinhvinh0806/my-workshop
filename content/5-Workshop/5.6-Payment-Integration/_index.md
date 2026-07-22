---
title : "VNPAY Payment Integration"
date : 2026-07-01 
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

In real-world e-commerce applications, integrating a payment gateway is an essential step. In this project, we will connect the system to the **VNPAY** payment gateway. This process requires high-security handling (generating a digital signature) and automatically capturing the returned data stream (Webhook/IPN).

#### Step 1: Create the Payment URL
When the user clicks "Pay", the system must not call VNPAY directly from the Frontend, in order to avoid exposing the Secret Key. Instead, the Frontend will call a Lambda function.

1. Create a new Lambda function named `createPaymentUrlFunction`.
2. This function will receive the trip information and the amount from the Frontend.
3. Using the `vnp_TmnCode` and `vnp_HashSecret` prepared in section 5.2, encode the parameters and generate a digital signature (Secure Hash).
4. The Lambda function returns a complete VNPAY URL link. The Frontend will redirect the user to the VNPAY payment page using this URL.

**Sample code (Node.js) for the `createPaymentUrlFunction` function:**

```javascript
import crypto from 'crypto';

export const handler = async (event) => {
    try {
        const body = JSON.parse(event.body);
        const { amount, ticketId, bankCode } = body;
        const tmnCode = process.env.VNP_TMN_CODE;
        const secretKey = process.env.VNP_HASH_SECRET;
        
        // ... (Logic for generating the VNPAY signature and payment URL) ...
        
        return {
            statusCode: 200,
            body: JSON.stringify({ paymentUrl: vnpUrl })
        };
    } catch (error) {
        return { statusCode: 500, body: JSON.stringify({ message: "Error creating payment URL" }) };
    }
};
```

**⚠️ Security Note (Best Practices):** 
Never store sensitive information directly in the source code. We will configure these two security keys in the Lambda function's **Environment variables** to ensure maximum security for the system.

**How to Set Up Environment Variables on the AWS Console:**
1. On the `createPaymentUrlFunction` configuration screen, switch to the **Configuration** tab.
2. Select **Environment variables** from the menu on the left.
3. Click **Edit** and add the two variables `VNP_TMN_CODE` and `VNP_HASH_SECRET`, along with your corresponding values.
4. Click **Save** to save the changes. The system will encrypt and protect these keys.

![Configuring VNPAY environment variables for AWS Lambda](/images/vnpay-lambda-env.png)

#### Step 2: Set Up the IPN (Instant Payment Notification) Webhook
After a customer successfully scans a QR code or enters an ATM card, VNPAY needs to notify our system so it can update the status to "Paid".

1. Create a second Lambda function named `vnpayWebhookFunction`.
2. This function is responsible for receiving data (a GET request) automatically sent by VNPAY's server system.
3. Inside the function, verify the digital signature (Checksum) again to make sure this request truly comes from VNPAY and not from a hacker's forged request.
4. If valid, the function connects to **Amazon DynamoDB**, finds the corresponding ticket code (`ticketId`) in the `Tickets` table, and updates the status field from `PENDING` to `SUCCESS`.
5. Finally, expose this function to the Internet through an **API Gateway** endpoint (e.g., `GET /api/v1/payments/vnpay_ipn`) and register this link in VNPAY's Merchant admin portal.

![Selecting the VNPAY payment method](/images/vnpay-checkout1.png)

![VNPAY Test card entry interface](/images/vnpay-checkout2.png)

![Payment success notification](/images/payment.png)

![Ticket status successfully updated](/images/my_ticket.png)