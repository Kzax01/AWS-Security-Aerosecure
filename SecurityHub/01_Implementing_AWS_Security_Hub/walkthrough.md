## 📌 Part 1 : Implement AWS Security Hub for Threat Detection

Welcome back to this exciting project! 

## 🎯 The Mission 

As mentioned before, The goal is to implement AWS Security Hub for continuous compliance and proactive threat detection.

## 📋 Table of Contents

- ⚡ [Phase 1: Creating an SNS Topic – Real-Time Alerts](#️-phase-1-creating-an-sns-topic--real-time-alerts)
- 🔄 [Phase 2: Automating Security Alerts with Amazon EventBridge](#-phase-2-automating-security-alerts-with-amazon-eventbridge)
- 🔍 [Phase 3: Enabling AWS Config – The Compliance Watchdog](#-phase-3-enabling-aws-config--the-compliance-watchdog)
- 🧠 [Phase 4: Enabling AWS Security Hub – The Intelligence Center](#️-phase-4-enabling-aws-security-hub--the-intelligence-center)
- 💥 [Phase 5: Simulating a Security Incident – Generate Findings](#-phase-5-simulating-a-security-incident--generate-findings)

**🚀 Let’s get started!**  

---

## 🛠️ **Phase 1: Creating an SNS Topic – Real-Time Alerts**  

📌 **Why?** We need an SNS topic to send email alerts whenever **AWS Security Hub detects a critical security issue**.  

### 👨‍💻 **Step 1: Create an SNS Topic**  
1. Open the **AWS Management Console**.  
2. In the **search bar**, enter **Simple Notification Service** and select it.  
3. Click **Create topic**.  
4. **Topic name** 🏷️: `eMailMe`  
5. Scroll down and click **Next step**.  
6. Expand **Access policy - optional** and set:  
   - **Publishers**: `Everyone`  
   - **Subscribers**: `Everyone`  
7. Click **Create topic**.  

_➡️ I called mine " AeroSecure-Kenza-project-email" ; feel free to call wahetever you want._


### 👨‍💻 **Step 2: Add an Email Subscription**  
1. Click **Create subscription**.  
2. **Protocol**: Select **Email**.  
3. **Endpoint**: Enter your **email address** for alerts.  
4. Click **Create subscription**.  
5. Check your **email inbox** for a confirmation email. Click **Confirm subscription**.  
6. Return to **AWS SNS** and refresh. The status should now be **Confirmed**.  

---

## 🔄 **Phase 2: Automating Security Alerts with Amazon EventBridge**  

📌 **Why?** We need to detect **important Security Hub findings** (e.g., misconfigurations, policy violations) and trigger an **SNS notification**.  

### 👨‍💻 **Step 3: Create an EventBridge Rule**  
1. In AWS Console, search for **Amazon EventBridge**.  
2. Click **Create rule**.  
3. **Rule name** 🏷️: `AeroSecureHub`  
4. **Description**: `"Triggers an email alert when a Security Hub finding is generated"`  
5. Click **Next**.  

### 👨‍💻 **Step 4: Define the Event Pattern**  
1. **AWS service**: `Security Hub`  
2. **Event type**: `Security Hub Findings - Important`  
3. **Compliance status**: Select `FAILED`  
4. **Record state**: Select `ACTIVE`  
5. **Severity**: Select `HIGH` and `CRITICAL`  
6. Click **Next**.  

➡️ The policy should look like that :
```
{
  "source": ["aws.securityhub"],
  "detail-type": ["Security Hub Findings - Imported"],
  "detail": {
    "findings": {
      "Compliance": {
        "Status": ["PASSED"]
      },
      "RecordState": ["ACTIVE"],
      "Severity": {
        "Label": ["HIGH", "CRITICAL"]
      }
    }
  }
}
``` 

### 👨‍💻 **Step 5: Set the Target to SNS**  
1. **Target type**: `AWS service`  
2. **Select a target**: `SNS topic`  
3. **Topic**: Select `eMailMe`  
4. Click **Next**, then **Create rule**.  

---

## 📊 **Phase 3: Enabling AWS Config – The Compliance Watchdog**  

> 📌 **Why?** AWS Config **continuously monitors** your AWS resources to check for misconfigurations.  

### 👨‍💻 **Step 6: Enable AWS Config**  
1. In AWS Console, search for **AWS Config**.  
2. Click **Set up AWS Config**.  
3. Click **1-click setup**.  
4. Click **Confirm**.  

🚀 **AWS Config is now tracking compliance across AWS resources!**  

---

## 🛡️ **Phase 4: Enabling AWS Security Hub – The Intelligence Center**  

>📌 **Why?** AWS Security Hub aggregates security findings from AWS services and third-party tools, providing a **centralized** view of compliance.  

### 👨‍💻 **Step 7: Enable Security Hub**  
1. Search for **Security Hub** in the AWS Console.  
2. Click **Go to Security Hub**.  
3. On the **Enable AWS Security Hub** page, select:  
   - ✅ **PCI DSS Compliance**  
   - ✅ **AWS Foundational Security Best Practices**  
   - ✅ **CIS AWS Foundations Benchmark**  
4. Click **Enable Security Hub**.  

### 👨‍💻 **Step 8: View Compliance Findings**  
1. In **Security Hub**, go to **Findings**.  
2. You will soon see security violations reported!  

---

## 🚨 **Phase 5: Simulating a Security Incident – Generate Findings**  

>📌 **Why?** To test our setup, let’s **create a misconfiguration** that should trigger Security Hub alerts.  

### 👨‍💻 **Step 9: Create a Security Group with Open Ports**  
1. Search for **EC2** in AWS Console.  
2. Click **Security Groups** under **Network & Security**.  
3. Click **Create security group**.  
4. **Name** 🏷️: `AeroSecureSecurityGroupTEST`  
5. **Description**: `"Security group of Aerosecure"`  
6. **Inbound rules**:  
   - **All ICMP - IPv4** → `0.0.0.0/0` (❌ Bad practice)  
   - **SSH** → `0.0.0.0/0` (❌ Open to all)  
   - **SSH** → `::/0` (❌ Open to IPv6)  
   - **RDP** → `0.0.0.0/0` (❌ Open to all)  
   - **RDP** → `::/0` (❌ Open to IPv6)  
7. Click **Create security group**.  

>🚨 **Result:** This misconfiguration will soon trigger Security Hub **high-severity findings**.  

### 👨‍💻 **Step 10: Verify Findings in Security Hub**  
1. Go back to **Security Hub**.  
2. Open **Findings**.  
3. You should see new **FAILED** findings.  
4. **Check your email inbox** – you should receive an alert from SNS!  

---

## 🎯 **Mission Accomplished!**  

✅ **Security Hub is monitoring AWS resources** for compliance violations.  
✅ **EventBridge automatically notifies security teams** of critical issues.  
✅ **AWS Config tracks configuration changes** for proactive security.  
✅ **Aerosecure is now protected** against compliance failures!  

### 🚀 **Next Steps**  
💡 **Automate remediation**: Use AWS Lambda to **fix misconfigurations automatically**.  
💡 **Integrate third-party security tools** into Security Hub.  
💡 **Explore GuardDuty** for **threat detection** beyond compliance.  

---

🛡️ **Congratulations, security engineer!** You have successfully deployed **AWS Security Hub** and **enhanced Aerosecure's security posture**. 🔥  

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
