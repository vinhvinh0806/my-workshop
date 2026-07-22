---
title : "Setting Up Security & Authentication"
date : 2026-07-01 
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

Security is a top priority in any system on AWS. In this section, we will set up a multi-layered security barrier: managing user identity with **Amazon Cognito**, protecting the API with an **Authorizer**, and internal permission management with **AWS IAM**.

#### Step 1: Manage Users with Amazon Cognito
Instead of building a complex password-encrypted login system ourselves, we will use a Cognito User Pool.

1. Go to the **Amazon Cognito** service and click **Create user pool**.
2. **Application type:** Select **Single-page application (SPA)** since our Frontend is built with ReactJS.
3. **Sign-in options:** Select **Email** (users will sign in with their email).
4. **Password policy:** Select Cognito's default settings (requiring uppercase, lowercase, numbers, and special characters).
5. **MFA and verification:** You may turn off MFA for easier testing in the lab environment. Select automatic email verification (Send email message).
6. Name the User pool `CarBookingUserPool` and create an **App client** named `CarBookingClient`.
7. Click **Create user pool**. Be sure to save the **User Pool ID** and **Client ID** for use in the Frontend configuration.

![Configuring Single-page application and Email Sign-in](/images/cognito-setup.png)

Once successfully initialized, the Cognito Overview interface will display general information about your User Pool.

![Overview of the Cognito User Pool with registered users](/images/cognito-security.png)

#### Step 2: Secure the API with a JWT Authorizer
We don't want just anyone to be able to call the `/dat-xe` (book-ticket) API. Only authenticated users (with a valid token) should be allowed to call it.

1. Go back to **API Gateway** and select your API.
2. Navigate to the **Authorization** (or Authorizers) section.
3. Click **Create authorizer** and select the type **JWT** (or Cognito, depending on the interface).
4. Select the `CarBookingUserPool` created in Step 1.
5. Once created, go back to the **Routes** tab and select the `POST /api/v1/dat-xe` route.
6. Click **Attach authorization** and select the authorizer you just created. This API is now securely locked!

![Attaching an Authorizer to secure the ticket-booking API](/images/jwt-authorizer.png)

#### Step 3: Assign Permissions with AWS IAM (Principle of Least Privilege)
Lambda functions cannot automatically read/write data to DynamoDB without being granted permission.

1. Go to the **AWS IAM** service -> **Roles**.
2. Find the Execution Role automatically created for the `bookTicketFunction`.
3. Click **Add permissions** -> **Attach policies**.
4. Search for and attach the `AmazonDynamoDBFullAccess` policy (or configure a Custom Policy that only allows operations on specific tables for better security).