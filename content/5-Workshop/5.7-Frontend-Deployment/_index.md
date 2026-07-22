---
title : "Frontend Deployment"
date : 2026-07-01 
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

Once the Backend is complete, the next step is to bring the user interface (Frontend - ReactJS) online. We will use **AWS Amplify** to automate the process of pulling code from GitHub, building, and hosting the application with a public URL.

#### Step 1: Configure Environment Variables (.env)
Before pushing the code to GitHub, the ReactJS application needs to know the Backend's address in order to communicate with it.

1. Open the Frontend source code folder in Visual Studio Code.
2. Find (or create) the `.env` file in the root directory.
3. Update the following values based on the resources you created in the previous steps:
   * `VITE_API_GATEWAY_URL`: The Invoke URL link of the API Gateway (created in section 5.4).
   * `VITE_COGNITO_USER_POOL_ID`: The ID of the Cognito User Pool (created in section 5.5).
   * `VITE_COGNITO_CLIENT_ID`: The Cognito App Client ID.

#### Step 2: Push the Source Code to GitHub
Make sure you have saved all your changes and pushed the source code to your personal GitHub repository.

```bash
git add .
git commit -m "Update environment variables for production"
git push origin staging
```

#### Step 3: Deploy on AWS Amplify

AWS frequently updates its interface, so the current deployment configuration process includes the following 4 straightforward steps:

1. **Choose source code provider:** Go to AWS Amplify and select **Create new app**. On the first screen, select **GitHub** as the source code provider and click Next.
![Select the GitHub provider](/images/amplify-new-step1.png)

2. **Add repository and branch:** Grant GitHub access, then select the repository containing the source code (`TangQuocHungg/DoAnDatVeBus`) and the branch to deploy (e.g., `main`).
![Select the Repository and Branch](/images/amplify-new-step2.png)

3. **App settings:** Set up the build configuration. Make sure Amplify correctly detects the build output directory as `dist` and that the build command matches ReactJS/Vite.
![Configure App Settings](/images/amplify-new-step3.png)

4. **Review:** Review all the settings you've configured. If everything is correct, click **Save and deploy** so the system automatically triggers the CI/CD pipeline.
![Review and Deploy](/images/amplify-new-step4.png)

#### Step 4: Check the Result
AWS Amplify will start the CI/CD process, which includes 3 steps: **Provision** -> **Build** -> **Deploy**. This process takes about 2-3 minutes.

Once the status changes to **Deployed** (green checkmark), Amplify will provide you with an HTTPS link (e.g., `https://staging.xyz123.amplifyapp.com`). Click that link, and your Serverless ride-booking website is now officially online!

![CI/CD deployment interface on AWS Amplify](/images/amplify-deploy.png)