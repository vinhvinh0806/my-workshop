---
title: "Proposal"
date: 2026-05-15
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

In this section, you need to summarize the content of the workshop you intend to do.

# Serverless Car Booking System
## A Comprehensive AWS Serverless Solution for Automated Online Ticket Booking and Payment

### 1. Executive Summary 
The "Online Car Booking System" project is designed to digitize and automate the entire process of booking coach tickets and online payments. The application is built on a Microservices/Serverless architecture on the AWS cloud platform, enabling the system to automatically scale infinitely during traffic spikes on holidays, while optimizing operational costs (pay-as-you-go). The system directly integrates with the VNPAY payment gateway, allowing users to book seats and pay 24/7 securely and conveniently, protected by Amazon Cognito.

### 2. Problem Statement 
**Current Issues** 
Many bus operators still use manual booking processes via phone calls or messages, leading to overloads, data errors (double-booked seats), and high human resource costs. Customers find it difficult to track trips and real-time seat availability, and the main payment methods are cash or manual bank transfers without automatic confirmation. Traditional server systems (On-premise/VPS) frequently experience downtime when traffic surges.

**Solution** 
Develop a Single Page Application (SPA) web app using ReactJS that provides an intuitive interface for customers to select trips and seats. The platform utilizes an AWS Serverless architecture: Amazon API Gateway and AWS Lambda (Node.js) handle business logic; Amazon DynamoDB (NoSQL) stores real-time seat and ticket statuses. Payments are processed through VNPAY's API, and the response (Webhook) is sent back to AWS Lambda to automatically update the ticket status. CI/CD management and frontend hosting are handled via AWS Amplify.

**Benefits and Return on Investment (ROI)** 
The solution completely eliminates manual tasks for operators, allowing them to serve a large number of customers simultaneously without increasing personnel costs. Utilizing a Serverless architecture ensures infrastructure costs during off-peak months are nearly zero (paying only when requests occur). Customers enjoy a smoother, more professional experience, thereby increasing conversion rates and revenue. The built-in cashless payment integration enables transparent and rapid cash flow turnover.

### 3. Solution Architecture 
The platform adopts an AWS Serverless architecture to completely decouple the Frontend and Backend. Unstructured and high-speed data is managed by DynamoDB. The security flow is tightly controlled from end-users to the database via Cognito, API Gateway Authorizer, and IAM Roles.

![Serverless Car Booking Architecture](/images/so_do_kien_truc1.png)

**AWS Services Used** 
*   **AWS Amplify**: Packages, hosts the ReactJS/Vite UI, and automates the CI/CD pipeline from GitHub.
*   **Amazon Cognito**: Manages the user lifecycle (registration, login) and issues secure JWT Tokens.
*   **Amazon API Gateway**: Builds RESTful APIs to route requests from the Frontend to the Backend, integrating Cognito Authorizer to block unauthorized access.
*   **AWS Lambda (Node.js)**: Runs core business logic code (creating tickets, checking seat availability) and processes the VNPAY IPN Webhook without managing servers.
*   **Amazon DynamoDB**: High-speed NoSQL database storage for Users, Trips, and Tickets tables.
*   **AWS IAM**: Implements Least Privilege security permissions for AWS services communicating with each other.
*   **Amazon CloudWatch**: Collects logs, monitors latency, and sets up system Alarms.

### 4. Technical Implementation 
**Implementation Phases** 
The project is deployed using the Agile model, divided into 4 main phases:
1.  **Initiation & Design**: Research AWS Serverless documentation, design basic UI/UX, draft the NoSQL database schema (DynamoDB), and system architecture.
2.  **Frontend Development (Client-side)**: Build the application using ReactJS (Vite), set up mock data, test the UI for the booking flow, and deploy to AWS Amplify.
3.  **Backend Development (Server-side)**: Write Lambda functions using Node.js, configure API Gateway, and connect read/write operations to DynamoDB. Set up Cognito to secure the APIs.
4.  **Integration & Operation**: Integrate VNPAY SDK, set up Webhooks. Perform E2E Testing, monitor logs on CloudWatch, and finalize IAM security configurations.

**Technical Requirements** 
*   **Frontend**: Use ReactJS and Vite to optimize build speed. Master state management and API calls (Axios/Fetch).
*   **Backend & Cloud**: Understand asynchronous programming (Async/Await) in Node.js. Master NoSQL concepts (Partition Key, Sort Key) in DynamoDB. Ability to read third-party API Specifications (VNPAY) and integrate AWS SDK (v3).

### 5. Roadmap & Milestones 
*   **Week 1 - Week 4**: Basic AWS research, finalize the topic, draft the solution, and write the Project Proposal.
*   **Week 5 - Week 6**: Local frontend UI programming and Cloud deployment (initially S3, then optimized with Amplify).
*   **Week 7**: Fully build the Serverless Backend infrastructure (DynamoDB, Lambda, API Gateway) and integrate Amazon Cognito authentication.
*   **Week 8 - Week 10**: VNPAY integration, IAM Role configuration, End-to-End (E2E) testing, and log/debug review via CloudWatch.
*   **Week 11 - Week 12**: Finalize technical documentation, prepare slides, hand over source code, and present the project acceptance report.

### 6. Budget Estimation 
By leveraging the **AWS Free Tier** for Serverless services, the cost of maintaining the system during development and small-scale operation is extremely low, almost zero.

**Infrastructure Costs (Estimated monthly for moderate traffic)** 
*   **AWS Lambda**: $0.00 (Free 1 million requests and 400,000 GB-seconds of compute time per month).
*   **Amazon DynamoDB**: $0.00 (Free 25 GB of storage and 25 RCU/WCU per month).
*   **Amazon API Gateway**: $0.00 (Free 1 million API calls in the first 12 months).
*   **Amazon Cognito**: $0.00 (Free 50,000 Monthly Active Users - MAU).
*   **AWS Amplify**: ~$0.50 (Charges for data transfer out and build time).
*   **Amazon CloudWatch**: $0.00 (Within the free monthly logging limits).

*Total Estimated AWS Cost: Under $1.00/month in the first year.*
*Other Costs: Free (Using VNPAY Sandbox environment for testing).*

### 7. Risk Assessment 
**Risk Matrix** 
*   **VNPAY Webhook integration failure (temporary network loss)**: High impact, medium probability.
*   **AWS Lambda Cold Start latency**: Medium impact, high probability.
*   **API Security Breach (Spam API calls/DDoS)**: High impact, low probability.

**Mitigation Strategies** 
*   **Webhook**: Build a cross-check mechanism for order status (Polling/Cronjob) to reconcile with VNPAY if the Webhook drops.
*   **Latency**: Configure Provisioned Concurrency for critical Lambda functions or optimize the uploaded Node.js library size.
*   **Security**: Use Cognito Authorizer to enforce JWT authentication for all critical APIs, combined with Throttling settings on API Gateway.

### 8. Expected Outcomes 
*   **Technical Improvements**: A complete cloud computing software system, automated from seat selection to cashless payment. Completely resolves the issues of system scalability and customer data security.
*   **Long-term Value**: The system can be packaged and commercialized (SaaS) for various transport enterprises. The lightweight, open-source structure makes it easy to upgrade or add new modules (such as voucher management, AI chatbot support) in the future.