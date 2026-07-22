---
title : "Building the Backend API"
date : 2026-07-01 
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

In this section, we will build the "brain" of the ride-booking system by using **AWS Lambda** to handle business logic and **Amazon API Gateway** to create endpoints that allow the user interface to communicate with the Backend.

#### Step 1: Create AWS Lambda Functions
AWS Lambda allows you to run code without provisioning or managing servers. We will create functions using the Node.js environment.

1. Go to the **AWS Lambda** service in the AWS Management Console.
2. Click **Create function**.
3. Select **Author from scratch**.
4. **Function name:** Enter `getTripsFunction` (the function that retrieves the list of trips).
5. **Runtime:** Select **Node.js 18.x** (or 20.x).

![AWS Lambda function creation interface](/images/create-lambda.png)

6. Click **Create function**.
7. In the **Code source** section, paste the processing code (the connection code to read data from DynamoDB) into the `index.mjs` or `index.js` file, then click **Deploy** to save it.

*(You need to repeat the steps above to create other core functions, such as `bookTicketFunction`, to handle the ticket-booking logic).*

#### Step 2: Create a REST API with Amazon API Gateway
API Gateway will act as the "gateway" that receives HTTP requests from end users and forwards them to the corresponding Lambda functions.

1. Go to the **API Gateway** service.
2. Find the **REST API** option (make sure not to select the Private type) and click **Build**.
3. Select **New API**, enter `CarBookingAPI` as the **API name**, and click **Create API**.
4. **Create a Resource:**
   * Click the **Create Resource** button.
   * Resource Name: Enter `trips` (the API call path will be `/trips`). Click Create.
5. **Create a Method:**
   * Select the `/trips` resource you just created, then click **Create Method**.
   * **Method type:** Select `GET`.
   * **Integration type:** Select **Lambda function**.
   * **Lambda function:** Type and select the `getTripsFunction` created in Step 1.
   * Click **Create method**.

*(Do the same to create the `/tickets` resource with the `POST` method, pointing it to the `bookTicketFunction`).*

![Configuring Routes and Methods on API Gateway](/images/api-gateway.png)

#### Step 3: Deploy the API
For the Frontend application to be able to call these APIs over the Internet, you must deploy them.

1. Click the **Deploy API** button in the top corner.
2. **Stage:** Select **New stage**, and set the Stage name to `prod` (Production).
3. Click **Deploy**.
4. The system will provide you with an **Invoke URL** link (e.g., `https://xyz.execute-api.us-east-1.amazonaws.com/prod`).

![Get the Invoke URL link to provide to the Frontend](/images/api-invoke-url.png)