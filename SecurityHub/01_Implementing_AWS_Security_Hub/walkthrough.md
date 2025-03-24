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
4. **Name the Topic as you want** 🏷️  
5. Scroll down and click **Next step**.  
6. Expand **Access policy - optional** and set:  
   - **Publishers**: `Everyone`  
   - **Subscribers**: `Everyone`  
7. Click **Create topic**.  

![creation topics](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/SNS-1.png)



### 👨‍💻 **Step 2: Add an Email Subscription**  

![subcription](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/SNS-2.png)

1. Click **Create subscription**.  
2. **Protocol**: Select **Email**.  
3. **Endpoint**: Enter your **email address** for alerts.  
4. Click **Create subscription**.  
5. Check your **email inbox** for a confirmation email. Click **Confirm subscription**.  
6. Return to **AWS SNS** and refresh. The status should now be **Confirmed**.  

![snsemail](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/sns3.png)

---

## 🔄 **Phase 2: Automating Security Alerts with Amazon EventBridge**  

![EB](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/EB-1.png)

> 📌 **Why?** We need to detect **important Security Hub findings** (e.g., misconfigurations, policy violations) and trigger an **SNS notification**.  

### 👨‍💻 **Step 3: Create an EventBridge Rule**  
1. In AWS Console, search for **Amazon EventBridge**.  
2. Click **Create rule**.  
3. **Name the Rule** 🏷️ 
4. **Description but it's optional**: `"Triggers an email alert when a Security Hub finding is generated"`  
5. Click **Next**.  

![EC2](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/EB-2.png)


### 👨‍💻 **Step 4: Define the Event Pattern**  
1. **AWS service**: `Security Hub`  
2. **Event type**: `Security Hub Findings - Important`  
3. **Compliance status**: Select `FAILED`  
4. **Record state**: Select `ACTIVE`  
5. **Severity**: Select `HIGH` and `CRITICAL`  
6. Click **Next**.  

![eb3](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/EB-3.png)

![eb4](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/EB-4.png)


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
> 📌 This policy filters findings from Security Hub that are imported, have a "PASSED" compliance status, are "ACTIVE", and have a "HIGH" or "CRITICAL" severity label.

### 👨‍💻 **Step 5: Set the Target to SNS**  

![eb5](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/EB-5.png)

1. **Target type**: `AWS service`  
2. **Select a target**: `SNS topic`  
3. **Topic**: Select `eMailMe`  
4. Click **Next**, then **Create rule**.  

---

## 📊 **Phase 3: Enabling AWS Config – The Compliance Watchdog**  

> 📌 **Why?** AWS Config **continuously monitors** your AWS resources to check for misconfigurations.  

### 👨‍💻 **Step 6: Enable AWS Config**  

![config](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/config%20main%20page.png)

1. In AWS Console, search for **AWS Config**.  
2. Click **Set up AWS Config**.  
3. Click **1-click setup**.  
4. Click **Confirm**.  

> 🚀 **AWS Config is now tracking compliance across AWS resources!**  

---

## 🛡️ **Phase 4: Enabling AWS Security Hub – The Intelligence Center**  

>📌 **Why?** AWS Security Hub aggregates security findings from AWS services and third-party tools, providing a **centralized** view of compliance.  

### 👨‍💻 **Step 7: Enable Security Hub**  

![SH](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/Sec%20Hub%20main%20page.png)

1. Search for **Security Hub** in the AWS Console.  
2. Click **Go to Security Hub**.  
3. On the **Enable AWS Security Hub** page, select:  
- ✅ **PCI DSS Compliance** – Secure AWS workloads to meet PCI DSS standards.  
- ✅ **AWS Foundational Security Best Practices** – Implement AWS-recommended security controls.  
- ✅ **CIS AWS Foundations Benchmark** – Align cloud security with CIS best practices.  
- ✅ **NIST 800-53 Revision 5** – Enforce compliance with NIST security controls.  

![compliance](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/compliance%20sec%20hub.png)

4. Click **Enable Security Hub**.  

![sc created](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/sec%20hub%20created.png)

![FYI](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/2H%20sub%20hub%20for%20findings.png)

> 💡 It can take some times before seeing some findigs! 

### 👨‍💻 **Step 8: View Compliance Findings**  
1. In **Security Hub**, go to **Findings**.  
2. You will soon see security violations reported!  

![1stfindings](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/1st%20findings.png)

---

## 🚨 **Phase 5: Simulating a Security Incident – Generate Findings**  

>📌 **Why?** To test our setup, let’s **create a misconfiguration** that should trigger Security Hub alerts.  

### 👨‍💻 **Step 9: Create a Security Group with Open Ports**  

![sg create](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/EC2-VPC.png)

1. Search for **EC2** in AWS Console.  
2. Click **Security Groups** under **Network & Security**.  
3. Click **Create security group (on top of the page)**.

![sg config](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/SG%20Rules.png)

4. **Name it as you want** 🏷️  
5. **Add a Description**: `"Security group of Aerosecure"`  
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

![final findings](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/sec%20hub%20final%20findings.png)

![findings](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/01_Implementing_AWS_Security_Hub/screenshots/sec%20hub%20final%20findings%20-2.png)

>💡 - **AWS Security Hub Findings** – Serve to identify, prioritize, and remediate security risks and compliance gaps in your AWS environment.


---

## 🎯 **Mission Accomplished!**  

✅ **Security Hub is monitoring AWS resources** for compliance violations.  
✅ **EventBridge automatically notifies security teams** of critical issues.  
✅ **AWS Config tracks configuration changes** for proactive security.  
✅ **Aerosecure is now protected** against compliance failures!  

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

