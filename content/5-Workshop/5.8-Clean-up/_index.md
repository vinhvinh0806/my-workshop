---
title : "Cleaning Up Resources"
date : 2026-07-10 
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---

Congratulations on successfully building the **Serverless Online Ride-Booking System**!

To avoid incurring unexpected charges beyond the Free Tier after completing this hands-on lab, you need to clean up and delete all the AWS resources you created, following the exact order below.

#### Step 1: Delete the Frontend (AWS Amplify)
1. Go to the **AWS Amplify** service.
2. Select the `CarBookingClient` app (or whatever name you gave your application).
3. In the left-hand menu, expand **App settings** -> select **General**.
4. Scroll all the way down and click the red **Delete app** button.
5. Type `delete` to confirm and proceed with the deletion.

![Deleting the AWS Amplify app](/images/Clean-up-AWS%20Amplify.png)

#### Step 2: Delete the API (Amazon API Gateway)
1. Go to the **API Gateway** service.
2. Find and select the API named `CarBookingAPI` (or DatXe-API).
3. Click the **Delete** button and confirm. This will remove the Backend's public-facing endpoint.

![Deleting Amazon API Gateway](/images/Clean-up-API%20Gateway.png)

#### Step 3: Delete the Business Logic (AWS Lambda)
1. Go to the **AWS Lambda** service -> select **Functions**.
2. Check all the functions you created in this project (e.g., `getTripsFunction`, `bookTicketFunction`, `createPaymentUrlFunction`, `vnpayWebhookFunction`, etc.).
3. Click the **Actions** button -> select **Delete**.

![Deleting AWS Lambda functions](/images/Clean-up-AWS%20Lambda.png)

#### Step 4: Delete the Authentication System (Amazon Cognito)
1. Go to the **Amazon Cognito** service -> select **User pools**.
2. Select `CarBookingUserPool`.
3. Click the **Delete user pool** button. The system will ask you to type the User Pool's name to confirm the deletion.

![Deleting the Amazon Cognito User Pool](/images/Clean-up-Amazon%20Cognito.png)

#### Step 5: Delete the Database (Amazon DynamoDB)
1. Go to the **Amazon DynamoDB** service -> select **Tables**.
2. Check the boxes for all 3 tables in turn: `Users`, `Trips` (or `CarTrips`), and `Tickets`.
3. Click the **Delete** button for each table. Uncheck the backup option if the system prompts you, to avoid consuming additional storage.

![Deleting Amazon DynamoDB tables](/images/Clean-up-DynamoDB.png)

#### Step 6: Delete Logs (Amazon CloudWatch) - Optional
Even though the log storage size is negligible, to clean things up 100%:
1. Go to **CloudWatch** -> select **Log groups**.
2. Delete all log groups starting with `/aws/lambda/` that are related to your functions.

![Deleting logs on Amazon CloudWatch](/images/Clean-up-CloudWatch.png)