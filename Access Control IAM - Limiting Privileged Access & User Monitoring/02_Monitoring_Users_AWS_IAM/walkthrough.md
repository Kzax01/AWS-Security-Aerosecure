# 📌 Part 2 : IAM Insights: Monitoring and Auditing Access

<p align="center">
  <img src="https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Access%20Control%20IAM%20-%20Limiting%20Privileged%20Access%20%26%20User%20Monitoring/02_Monitoring_Users_AWS_IAM/screenshots/IAM%20%20The%20Audit%20Tools%20KENZA.gif" width="800"/>
</p>

<p align="center">
  <strong>Welcome to the 2nd part of this AWS IAM Monitoring project! 🚀</strong>
</p>

## 🎯 The Mission :
As we seen earlier, the scenario involves monitoring access for AeroSecure, where the **SOC** team identified suspicious activity, including unauthorized login attempts on an AWS account with elevated permissions, posing a potential threat to critical resources and data integrity.

## Table of Contents
- 🚨 [Generate a Credential Report](#generate-a-credential-report-)
- 🔍 [Utilize the Access Advisor Tab](#utilize-the-access-advisor-tab-)
- 🛠️ [Create a Trail using CloudTrail](#create-a-trail-using-cloudtrail-️)
- 🧠 [Why These Steps Matter](#why-these-steps-matter-)

---

# 🔥 IAM & CloudTrail Security Monitoring  

## 🚀 Step 1: Generate a Credential Report  
**IAM → Credential Report**  

![credreport](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/credentialreportdl.png)


1. Click **"Download Report"** 📥 (CSV contains MFA status, password rotation, last login).  
> - **Why?** → Identify inactive users, accounts without MFA, and potential security risks.  

![reportexample](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/01credreport.png)

---

## 🔍 Step 2: Analyze User Activity (Access Advisor)  
**IAM → Users → Create users → select one → Access Advisor**  

![lastaccessed](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/2-last-accessed.png)

- Check the **"Last accessed"** column to detect unused permissions.  
> - **Why?** → Remove unnecessary access to minimize the attack surface.  

---

## 🚀 Step 3: Create a CloudTrail Trail  
**Navigate to AWS CloudTrail → Dashboard → Create trail**  

### ➡ Configure Trail Attributes  

![step1-ct](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/4.1-CT.png)
- **Name the Trail name that you're creating**
- **Storage location:** Create a new S3 bucket (for long-term log storage).  
- **Trail log bucket and folder:** Default bucket name (AWS generates a secure name).
- **Log file validation:** ❌ Disabled  
  - **Why?** → Useful for compliance but not mandatory in this case.  
- **CloudWatch Logs:** ❌ Disabled  
  - **Why?** → Can be enabled later if real-time analysis is needed.  

Click **Next** ⏭️  

---

## 🔍 Step 4: Select Events to Log  

![eventlog](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/4.2-ct-event.png)
- ✅ **Management events** (Tracks AWS service activity)  
- ✅ **Data events** (Monitors resource access)  
- ✅ **Insights events** (Detects abnormal activity)  

### Configure Management Events  

![managementevents](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/4.3-CT-managementevent.png)
- **Select:** ✅ **Read + Write**  
> - **Why?** → Captures all actions (read/write) for complete monitoring.  

### Configure Data Events  

![dataevent](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/4.4-CT-S3config.png)
- **S3:** ✅ **Read + Write** (Monitors bucket access).  
- **Lambda:** ✅ **All regions + All functions** (Detects function execution).  
> - **Why?** → Identifies suspicious access to files and Lambda executions.  

### Configure Insights Events  

![api call](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/4.5-CT-.png)
- **Enable:** ✅ **API call rate** (Detects unusual API spikes).  
> - **Why?** → Helps detect brute-force or credential stuffing attacks.  

---

## ✅ Step 5: Finalize and Activate the Trail
1. Click **Next** ⏭️  

![ctdone](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/5-ct-donecreated.png)
2. Review the configuration and click **"Create trail"** 🎯  
3. 🚀 **Congratulations!** Your CloudTrail is now active and monitoring AWS actions. 

![ctdone1](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/02_Monitoring_Users_AWS_IAM/screenshots/example%20of%20what%20events.png)

---

## 📌 Bonus: Test and Analyze Logs  
Once your trail is active, test its functionality by running CLI commands or performing actions in the console.  

| Command                                    | Description                                                                 | Source Link                                                                                              |
|--------------------------------------------|-----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| `aws cloudtrail list-trails`               | Lists all CloudTrail trails associated with your AWS account.               | [AWS CLI Command Reference - list-trails](https://docs.aws.amazon.com/cli/latest/reference/cloudtrail/list-trails.html) |
| `aws cloudtrail lookup-events --max-results 5` | Retrieves the most recent CloudTrail events (limited to 5 results).         | [AWS CLI Command Reference - lookup-events](https://docs.aws.amazon.com/cli/latest/reference/cloudtrail/lookup-events.html) |
| `aws s3 ls s3://your-bucket-name/`         | Lists the objects stored in a specific S3 bucket.                           | [AWS CLI Command Reference - s3 ls](https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html)          |


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

