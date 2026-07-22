---
title : "Database Initialization"
date : 2026-07-01 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

In this section, we will set up the database for the online ride-booking system. Since the application uses a Serverless architecture, **Amazon DynamoDB** is the perfect choice thanks to its automatic scaling capability and millisecond response speed, without the need to manage virtual servers.

Our system will need 3 main data tables:
1. **Users:** Stores customer information.
2. **Trips:** Stores the list of rides (departure point, destination, time, fare).
3. **Tickets:** Stores information about booked tickets and payment status.

#### Steps to Create Tables on DynamoDB

**Step 1: Access the DynamoDB service**
* Log in to the [AWS Management Console](https://console.aws.amazon.com/).
* Make sure you are in the **us-east-1 (N. Virginia)** Region (check the top-right corner).
* In the search bar, type **DynamoDB** and select the service.

**Step 2: Create the Users table**
* In the left-hand menu, select **Tables** and click the **Create table** button (the orange Create table button).
* **Table name:** Enter `Users`
* **Partition key:** Enter `userId` (Data type: **String**)
* **Table settings:** Select **Default settings**.
* Click **Create table** at the bottom of the page and wait a few seconds for the table to switch to the *Active* state.
![DynamoDB table creation input interface](/images/create-dynamodb-table.png)

**Step 3: Create the Trips table**
* Repeat the table creation steps.
* **Table name:** Enter `Trips`
* **Partition key:** Enter `tripId` (Data type: **String**)
* Click **Create table**.

**Step 4: Create the Tickets table**
* Repeat the table creation steps once more.
* **Table name:** Enter `Tickets`
* **Partition key:** Enter `ticketId` (Data type: **String**)
* Click **Create table**.

Once completed, you will see the list of the 3 tables you just created with an **Active** status as shown below, ready to be connected to the AWS Lambda functions in the next section.

![List of DynamoDB tables](/images/dynamodb-tables.png)