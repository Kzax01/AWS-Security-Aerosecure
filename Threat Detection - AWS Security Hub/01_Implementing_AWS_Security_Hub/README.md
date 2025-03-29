## Part 1 : How to Implement AWS Security Hub for Threat Detection

<p align="center">
  <img src="https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Threat%20Detection%20-%20AWS%20Security%20Hub/01_Implementing_AWS_Security_Hub/screenshots/Aerosecure%20sec%20hub.gif" width="80%" />
</p>

<p align="center">
  <b>Welcome to this new 1st part project!</b>
</p>


---

## Context: Compliance & Proactive Threat Detection 🛡️  
AeroSecure has migrated its critical systems to AWS, hosting applications for aircraft maintenance, telemetry, and real-time monitoring. With this transition, new security risks have emerged:  
🔸 Lack of visibility on compliance (ISO 27001, NIST).  
🔸 Risky configurations that could be exploited.  
🔸 No real-time alerts, delaying incident detection.  

The goal: Implement AWS Security Hub for continuous compliance and proactive threat detection.

---

## Objectives for Setting Up Security Hub 🎯  
✅ Enable AWS Security Hub & AWS Config for cloud monitoring.  
✅ Assess compliance with ISO 27001 & NIST standards.  
✅ Set up automated alerts for critical violations.  
✅ Automate corrective actions for faster incident response.

---

## 🚀 Deploying AWS Security Hub at AeroSecure

### 1️⃣ **Activating AWS Security Hub** 📌  
Enable AWS Security Hub and configure compliance standards (ISO 27001, NIST).  
Link with AWS Config to track resource changes.

### 2️⃣ **Setting Up Alerts** 📌  
Create an SNS topic and EventBridge rule for high-severity violations.  
Instant alerts are sent when issues are detected.

### 3️⃣ **Testing Security Hub’s Detection** 📌  
Generate intentional findings to test detection:  
- EC2 security flaws  
- Public S3 buckets  
- Misconfigured IAM permissions  

### 4️⃣ **Automating Remediation** 📌  
Deploy AWS Lambda to auto-fix detected issues (e.g., restrictive security group settings, disabling public S3 access).

---

## 🎯 Expected Outcomes  
✅ Continuous compliance monitoring for AeroSecure.  
✅ Real-time alerts sent to the security team.  
✅ Automatic remediation of critical vulnerabilities.  
✅ Enhanced security and compliance with aviation standards.

---

## 📊 Key Services and Their Role in this project :

| **Service**                  | **What it is**                                                                                      | **Why it's Important for Security**                                                                                   |
|------------------------------|------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **AWS Security Hub**          | A centralized security service that aggregates findings from multiple AWS services.                  | Provides a unified view of your security status, helping you quickly identify and address potential threats.             |
| **AWS Config**                | A service that tracks AWS resource configurations and compliance over time.                           | Ensures continuous monitoring of configurations, helping detect non-compliant changes that could lead to security risks. |
| **Amazon EventBridge**        | A serverless event bus that facilitates event-driven architecture.                                   | Automates responses to security findings by triggering actions based on specific events (e.g., a new alert).            |
| **AWS Lambda**                | A serverless compute service that lets you run code in response to events.                           | Enables automatic remediation actions (e.g., closing open ports, disabling public access to resources) without manual intervention. |
| **Amazon SNS**                | A fully managed notification service for sending messages to subscribers.                            | Sends real-time alerts to security teams about security violations, ensuring fast response times to critical issues.    |
| **AWS IAM (Identity and Access Management)** | A service that manages access to AWS resources by defining roles, permissions, and policies. | Controls who can access resources and what actions they can perform, ensuring least privilege and preventing unauthorized access. |
| **AWS WAF**                   | A web application firewall that protects web applications from common web exploits.                | Helps prevent attacks like SQL injection and cross-site scripting, protecting applications and data from malicious traffic. |

---

These services work together to provide continuous security monitoring, fast incident detection, and automated remediation in the AWS environment. The goal is to stay ahead of potential threats and ensure compliance with industry standards like ISO 27001 and NIST.  


## 🔗 Link to Part 2  
The scenario continues! AeroSecure faces a cyberattack, deploying honeypots and testing Security Hub’s response.  
🔥 **Next step**: Simulate an attack and analyze Security Hub’s real-time response! 🚀

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
