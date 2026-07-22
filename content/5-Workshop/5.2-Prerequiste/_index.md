---
title : "Preparation Steps"
date : 2026-07-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

Before diving into building the system on AWS, you need to make sure you have fully prepared the following tools and configuration information.

#### 1. Account and Software Requirements
To follow along with this documentation, your computer needs to have the following environments installed:
* **AWS Account:** An active AWS account. It is recommended to use a new account to take advantage of the Free Tier.
* **Node.js:** Version 18.x or higher. This is the runtime environment for running the Frontend (ReactJS) and Backend (AWS Lambda) source code.
* **Git:** Used to clone the sample source code and connect the repository with AWS Amplify.
* **Code Editor:** Visual Studio Code is recommended.
* **Web Browser:** Google Chrome or Firefox.

#### 2. Preparing the Payment Environment (VNPAY Sandbox)
The ride-booking system requires online payment integration. We will use VNPAY's Sandbox (testing) environment.

**Step 1:** Register a Sandbox account
* Go to VNPAY Sandbox's official developer website: `https://sandbox.vnpayment.vn/devreg/`
* Fill in all the required information to register a Merchant account.

![Register VNPAY Sandbox](/images/vnpay-sandbox2.png)

**Step 2:** Obtain your configuration information (Credentials)
* Once you have registered successfully, VNPAY will send you an email containing important connection details.
* Here, you will find 2 extremely important pieces of information that you need to save for use in the Backend AWS Lambda programming later on:
    * `vnp_TmnCode` (Website code)
    * `vnp_HashSecret` (Secret string used to generate the digital signature)

![VNPAY configuration information email](/images/vnpay-sandbox.png)