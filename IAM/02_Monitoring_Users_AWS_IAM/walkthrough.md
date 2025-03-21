# 📌 Monitoring, Auditing, and Logging Users and Resource Usage in AWS IAM

Welcome back to this exciting project! 

## 🎯 The Mission :
As we seen earlier, the scenario involves monitoring access for AeroSecure, where the **SOC** team identified suspicious activity, including unauthorized login attempts on an AWS account with elevated permissions, posing a potential threat to critical resources and data integrity.

## Table of Contents
- 🚨 [Generate a Credential Report](#generate-a-credential-report-)
- 🔍 [Utilize the Access Advisor Tab](#utilize-the-access-advisor-tab-)
- 🛠️ [Create a Trail using CloudTrail](#create-a-trail-using-cloudtrail-️)
- 🧠 [Why These Steps Matter](#why-these-steps-matter-)

---

## Step 1 : Generate a Credential Report 📊

1. **Navigate to IAM dashboard**
   - Log into AWS Console
   - Find and select IAM from the services menu
   - This takes you to the central location for managing identities and access
   

2. **Access Credential Report section**
   - From the left menu, click **Credential report**
   - This report provides security-related information for all users in your account
   - Information includes password usage, MFA status, access key age, and last activity timestamps
   

3. **Generate and download the report**
   - Click **Download Report** to save the CSV file 
   - Open the file using Excel, Numbers, or any spreadsheet program
   - This report helps identify security risks like unused credentials, missing MFA, or old passwords that should be rotated
   

---

## Step 2 :  Utilize the Access Advisor Tab 🔎

1. **Return to IAM Dashboard**
   - Navigate back to the main IAM page
   - Access Advisor helps you understand which permissions are actually being used
   

2. **Access User List**
   - From the left menu, click **Users**
   - This displays all IAM users in your account
   

3. **Select specific user**
   - Click on **developer-1** user
   - This opens the detailed configuration page for this specific user
   

4. **View Access Advisor data**
   - Click the **Access Advisor** tab
   - Review the list of services this user is allowed to access
   - Check the "Last accessed" column to see when each service was last used
   - Services with permissions but no recent usage might indicate over-privileged users
   - This information is crucial for implementing least privilege principles
   

---

##  Step 3 : Create a Trail using CloudTrail 🛤️

1. **Navigate to CloudTrail**
   - Return to the AWS Management Console
   - Find and select CloudTrail from the services menu
   - CloudTrail records API calls and account activity for monitoring and compliance
   

2. **Review existing events**
   - On the left menu, click **Event history**
   - This shows recent API activity in your account
   - You can filter by event name, user, resource, or time period
   

3. **Start trail creation**
   - There are two ways to create a trail:
     * Option 1: Click **Dashboard** in the left menu, then click **Create trail**
     * Option 2: Click **Trails** in the left menu, then click **Create trail**
   - Trails provide a permanent record of account activity, unlike event history which is limited to 90 days
   

4. **Configure trail attributes**
   - Set the following values on the *Choose trail attributes* page:
     * **Trail name**: "kzaw01Trail" ( or whatever the name you want lol)
     * **Storage location**: Create new S3 bucket
     * **Trail log bucket and folder**: Keep the default bucket name
     * **Log file SSE-KMS encryption**: Deselect **Enabled**
     * **Log file validation**: Deselect **Enabled**
     * **CloudWatch Logs**: Deselect **Enabled**
   - The trail name should be descriptive of its purpose
   - S3 is where your logs will be stored long-term
   - KMS encryption adds another layer of security but isn't needed for this lab
   

5. **Proceed to next step**
   - Click **Next**
   - This takes you to the log selection page
   

6. **Select event types**
   - On the *Choose log events* page, select:
     * **Management events**: Administrative operations that modify resources
     * **Data events**: Resource operations performed on or within resources
     * **Insights events**: Unusual API call rate activity
   - Management events show who is making changes to your environment
   - Data events show access to your data (like S3 objects)
   - Insights events help identify unusual activity patterns
   

7. **Configure Management events**
   - In the *Management events* section:
     * Select both **Read** and **Write**
   - Read events show who is viewing resources
   - Write events show who is modifying resources
   

8. **Configure Data events**
   - In the *Data events* section, set:
     * **Data event: S3**: Select both **Read** and **Write**
     * **Data event: Lambda**: **All regions** and **All functions**
   - S3 data events log object-level operations like GetObject and PutObject
   - Lambda data events log function invocations
   

9. **Configure Insights events**
   - In the *Insights events* section:
     * Select **API call rate**
   - This detects unusual patterns in API usage that might indicate security issues
   

10. **Review and create**
    - Click **Next** to proceed to the review page
    - Verify all settings are correct
    - Click **Create trail** to finish
    - Your trail is now recording activities across your AWS account

> Obviously mine is showing cloud_user as part of the sandbox but, whenever an user will do anything, it'll show up as well. 
    

---

## Why These Steps Matter 💡

- **Credential Reports** help identify security risks by showing unused credentials, missing MFA, and outdated passwords

- **Access Advisor** helps implement least privilege by showing which permissions are actually being used and which can be safely removed

- **CloudTrail** provides:
  - Complete visibility into who is doing what in your AWS account
  - Compliance evidence for auditors
  - Security investigation capabilities
  - Historical record of all account activity

These monitoring tools are essential for maintaining security, meeting compliance requirements, and quickly responding to suspicious activities in your AWS environment.

---

## 💬 Let’s Connect!  
Thank you for visiting my GitHub! 🌸  

Here, I share my **Cloud Security projects** and **AWS learning journey**.  
Looking for **Cloud Computing Security** articles? Check out my **Medium**!  

<p align="center">
  <a href="https://www.linkedin.com/in/kenza-in-the-cloud/" target="_blank">
    <img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn">
  </a>
  <a href="https://discord.com/users/kzax01" target="_blank">
    <img src="https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Discord">
  </a>
  <a href="https://medium.com/@Kenza.In.The.Cloud" target="_blank">
    <img src="https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white" alt="Medium">
  </a>
</p>


### ☁️ Let’s build the future of cloud together!  
<p align="center">
  <img src="https://i.pinimg.com/originals/91/1d/91/911d914aaf6194489a3f5626bed2bd3a.gif" width="600" alt="Cool GIF">
</p>

---
> ###### _Big thanks to a Cloud Guru for providing the sandbox & their teachers for their guidance during this project_
