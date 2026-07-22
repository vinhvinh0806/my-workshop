---
title : "Introduction"
date : 2026-07-01 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Introduction to AWS Serverless Architecture

**Serverless computing** is a cloud computing execution model in which the cloud provider (AWS) dynamically manages the allocation and provisioning of servers. 

The core benefits of Serverless architecture for e-commerce systems (like ticket booking) include:
* **No server management:** Developers can focus entirely on writing code (Business Logic) without worrying about operating systems or security patches.
* **Auto-scaling:** The system can automatically handle thousands of concurrent requests during peak holiday seasons without experiencing downtime.
* **Cost optimization (Pay-as-you-go):** You only pay for the exact compute time and the number of actual requests. When there are no customers, infrastructure costs drop to almost zero.

#### Workshop Overview

In this Workshop, you will act as a Cloud Engineer to build a complete **Online Car Booking System (Serverless Architecture)** step-by-step. The project covers the entire lifecycle of a real-world application, from user authentication to third-party payment gateway integration.

The system will be divided into the following main modules:
1.  **User Interface (Frontend):** A ReactJS Single Page Application (SPA) with automated CI/CD and hosting on **AWS Amplify**.
2.  **Data Storage (Database):** A high-performance NoSQL database, **Amazon DynamoDB**, used to manage Users, Trips, and Tickets information.
3.  **Business Logic (Backend):** RESTful APIs built with **Amazon API Gateway**, routing requests to **AWS Lambda** (Node.js) compute functions.
4.  **Security & Identity:** Managing the login flow with **Amazon Cognito** and enforcing the principle of least privilege using **AWS IAM**.
5.  **Payment Integration:** Processing transactions via the **VNPAY** gateway and automatically updating status through an IPN Webhook.
6.  **Monitoring:** Storing logs and tracking system errors using **Amazon CloudWatch**.

![Serverless Car Booking Architecture](/images/so_do_kien_truc1.png)