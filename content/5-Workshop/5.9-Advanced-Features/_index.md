---
title : "Upgrading the System Architecture"
date : 2026-07-01 
weight : 9
chapter : false
pre : " <b> 5.9. </b> "
---

To bring the Serverless ride-booking system closer to the standards of a real-world, large-scale (enterprise-ready) application, the system has been designed with room to integrate additional high-level AWS services. Below are the extended components aimed at optimizing security, performance, and user experience.

#### 1. Static Storage with Amazon S3
Instead of storing data as Base64 or external URLs, **Amazon S3** is used as a dedicated storage repository for all vehicle images (buses, interiors) and system documents. This helps reduce the load on the DynamoDB database and optimizes page-loading bandwidth.

![Storing images on Amazon S3](/images/s3-storage.png)

#### 2. Security Key Management with AWS Secrets Manager
Sensitive information such as VNPAY's `vnp_HashSecret` and `vnp_TmnCode` must never be hard-coded directly into the source code. They are stored securely encrypted in **AWS Secrets Manager**, which allows for centralized management and strict access control through IAM policies, effectively preventing the risk of data leaks.

![Storing payment keys in Secrets Manager](/images/secrets-manager.png)

#### 3. Notification System with Amazon SES
To enhance the customer experience, **Amazon SES (Simple Email Service)** is incorporated into the architecture. Whenever VNPAY's Webhook confirms a successful payment, the Lambda function automatically triggers SES to immediately send an electronic invoice email along with a ticket code (QR Code) to the user's inbox.

![Verifying email identity on Amazon SES](/images/Ses_email.png)

#### 4. Web Application Firewall (AWS WAF)
To protect the system from denial-of-service (DDoS) attacks or automated scanning (Botnet), a security rule set using **AWS WAF** (Protection Pack) has been successfully configured. Following the standard AWS architecture for HTTP APIs, this rule set is designed to be ready for integration with the Amazon CloudFront content delivery network. In that setup, WAF acts as a global filter, immediately rejecting IPs exhibiting abnormal behavior before their traffic can even reach the Backend.

![Setting up the WAF Firewall for API Gateway](/images/waf_firewall.png)