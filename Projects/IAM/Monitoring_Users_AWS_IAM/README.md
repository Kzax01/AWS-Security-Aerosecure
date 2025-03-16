# 📌 GitHub Project: Monitoring, Auditing, and Logging Users and Resource Usage in AWS IAM

Welcome to this exciting new project!

---

## 🔍 Scenario: AWS IAM Access Monitoring and Auditing for AeroSecure

In this hands-on lab, you'll take on the role of a **Cloud Security Engineer** at **AeroSecure**, a cybersecurity company specializing in securing cloud infrastructures used by the aerospace industry. The data managed by AeroSecure is highly sensitive, including satellite communications and critical navigation systems for aircraft.

---

## 🚨 Problem  
Recently, AeroSecure's **SOC** team detected suspicious login attempts on an AWS account with elevated permissions. These unauthorized accesses could compromise critical resources and threaten the integrity of operations.

## 🎯 Goal  
Your mission is to implement advanced IAM activity monitoring using **AWS IAM** and **AWS CloudTrail**. This will allow you to:
- ✅ Ensure traceability of access to AWS resources.
- ✅ Detect any abnormal or unauthorized activity.
- ✅ Strengthen compliance with aerospace cybersecurity standards (ISO 27001, NIST 800-53).

---

## 🔍 Context and Project Importance

| **Concept**           | **Definition**                                                                      |
|-----------------------|-------------------------------------------------------------------------------------|
| **AWS IAM**           | Service for managing identities and access to AWS resources.                        |
| **CloudTrail**        | AWS service that logs API calls made within an AWS account.                          |
| **Access Advisor**    | Tool for analyzing service usage by an IAM user.                                    |
| **Credential Report** | Report that lists the status of IAM users' credentials.                             |

### Why is it important?  
- **Visibility and auditability**: Track who is doing what on the AWS account.  
- **Reducing unnecessary access**: Identify unused permissions to avoid over-permissioning.  
- **Detecting suspicious activities**: Logs all actions performed, useful in case of a security incident.  
- **Compliance**: Meets regulatory requirements for traceability and identity management.

### Consequences of Poor IAM Management  
- **Data leaks**: A user with too many permissions could expose sensitive data.  
- **AWS account compromise**: Unprotected credentials could be exploited by an attacker.  
- **Non-compliance**: Poor access management could lead to sanctions or fines during audits.

---

## 🛠 What We’ll Cover  
- Generating a **Credential Report** to audit IAM credentials.  
- Using **IAM Access Advisor** to identify unused permissions.  
- Setting up **CloudTrail** to monitor activities on the AWS account.

---

## 🎯 Learning Objectives  
- Understand how to monitor IAM resource usage.  
- Learn how to detect unused or insecure IAM access.  
- Implement tracking of AWS actions for auditing and security purposes.

---

## 🏗️ Project Steps  

### **1️⃣ Generate a Credential Report**  
📌 A **Credential Report** provides an overview of IAM credentials (last usage, MFA status, etc.).  
1. Access the AWS IAM console.  
2. Go to **Credential Report** (left-hand menu).  
3. Click **Download Report**.  
4. Analyze the downloaded CSV file to identify users without MFA or with outdated credentials.  
➡️ **Why?** This helps identify inactive accounts, outdated credentials, and IAM best practices.

### **2️⃣ Use Access Advisor to Audit IAM Permissions**  
📌 **Access Advisor** shows which services are used by a given user.  
1. Go to the IAM console.  
2. Navigate to **Users** and select a user (e.g., `developer-1`).  
3. Click **Access Advisor**.  
4. Check if any AWS services are accessible but unused.  
5. Adjust permissions to remove unnecessary access.  
➡️ **Why?** This implements the **Least Privilege** principle by removing unneeded access.

### **3️⃣ Create a CloudTrail to Monitor Account Activities**  
📌 **CloudTrail** records all actions taken within your AWS account.  
1. Go to the **AWS CloudTrail** console.  
2. Navigate to **Event History** to see event logs.  
3. Click **Create Trail** and fill in the following parameters:  
   - Trail Name: `lab-trail`  
   - Create a new S3 bucket to store logs  
   - Disable SSE-KMS Encryption and Log File Validation  
4. In **Choose log events**:  
   - **Management Events**: Enable **Read & Write**  
   - **Data Events**: Enable **S3** and **Lambda** in **All Regions**  
   - **Insights Events**: Enable **API call rate**  
5. Click **Create trail**.  
➡️ **Why?** This ensures full visibility over AWS resource usage and helps investigate any suspicious behavior.

---

## 🚀 Expected Results  
- ✅ A complete audit of IAM access and permissions with an active audit trail on **CloudTrail** to ensure traceability and security of operations related to **OrbitalX satellites**.  
- 📌 Monitoring of non-compliant access.  
- 📌 Optimization of IAM permissions.  
- 📌 Compliance with aerospace standards.

---

## 📁 Resources  
- [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/)  
- [AWS CloudTrail Documentation](https://docs.aws.amazon.com/cloudtrail/latest/userguide/)
