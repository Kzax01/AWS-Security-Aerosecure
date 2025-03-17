# Part 2: Identifying and Remediating Threats with AWS Security Hub 🔍🛡️

**Context**: **Security Incident at AeroSecure**  
After setting up AWS Security Hub for monitoring (Part 1), AeroSecure detects suspicious activity: EC2 instances exposed with risky firewall rules and unauthorized attempts to exfiltrate sensitive data.

---

## Attack Simulation Breakdown 🚨

### 1. **Exploitation Phase** 🔓  
- Attackers exploited misconfigured security groups on EC2 instances for aircraft surveillance.  
- SSH (port 22) was open to **0.0.0.0/0**, allowing brute-force attempts.

### 2. **Deploying Honeypots 🪤**  
- Honeypots (fake vulnerable instances) were deployed in an isolated VPC to capture attacker activity.  
- Logs include IPs, techniques, and commands used by attackers.

### 3. **Simulated Data Exfiltration 🛑**  
- Compromised IAM keys (e.g., leaked GitHub keys) allowed access to an S3 bucket containing sensitive data.  
- **Security Hub** triggered an alert via **EventBridge** and **SNS** for abnormal access.

---

## Remediation Phase 🚑

✅ **1. Investigate via AWS Security Hub**  
- Identify the attacker’s IP and activity using **CloudTrail** and **GuardDuty** logs.

✅ **2. Countermeasures**  
- Block malicious IPs with **AWS WAF** and **VPC ACLs**.  
- Revoke and rotate IAM keys.  
- Close open ports and apply **least privilege** access policies.

✅ **3. Automate Response**  
- Use **AWS Lambda** to automatically disable suspicious access.  
- Integrate **AWS Systems Manager Automation** for immediate remediation of dangerous configurations.

---

## Why This Matters 🌍

AWS Security Hub consolidates security findings, helping quickly detect, analyze, and remediate threats. It provides a unified view to enhance threat response and security management.

---

## Key Concepts 📝

| **Term**                  | **Definition**                                                                                         | **Importance**                                                                                                 |
|---------------------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| **AWS Security Hub**       | Centralized service that aggregates findings from multiple AWS services.                               | A single pane of glass to quickly detect and resolve security threats.                                         |
| **Honeypot**               | Decoy systems designed to trap attackers and gather intel on attack methods.                           | Provides valuable insights to strengthen security and trap attackers in controlled environments.             |
| **IAM Key Compromise**     | Leakage or theft of IAM keys, enabling unauthorized access to AWS resources.                           | Protects sensitive data and ensures only authorized users have access to critical resources.                  |
| **EventBridge**            | Serverless event bus service that enables event-driven architecture.                                   | Facilitates automated responses to incidents, reducing manual intervention and accelerating reaction time.     |
| **WAF (Web Application Firewall)** | Protects web applications from common attacks like SQL injection and XSS.                        | Stops malicious traffic before it reaches your applications, protecting against potential breaches.            |

---

By implementing these steps, **AeroSecure** can quickly respond to threats, continuously improve security, and stay ahead of potential attacks. 💪

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
