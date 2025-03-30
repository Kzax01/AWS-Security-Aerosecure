# 📌 Securing Your Web Application with AWS WAF  

<p align="center">
  <img src="https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/aws-waf-cyberpunk.KENZA.gif" width="800">
</p>


<p align="center">
  Welcome to this new exciting project!
</p>

## 🎯 The Mission :
As seen previously, the objective is to deploy an AWS Web Access Control List (Web ACL) to detect and block threats such as SQL injections and malicious HTTP headers targeting your web application.
> Note that we only focus on setting the WAF in a Cloud Sec point of view. 

## Table of Contents
- 🛡️ [Phase 1: Creating the Web ACL](#-phase-1-creating-the-web-acl--the-defense-wall)
- 🔒 [Phase 2: Adding AWS Managed Rules](#️-phase-2-adding-aws-managed-rules--the-first-layer-of-defense)
- 🔧 [Phase 3: Creating a Custom Rule](#-phase-3-creating-a-custom-rule--advanced-protection)
- ✅ [Phase 4: Final Configuration & Validation](#-phase-4-final-configuration--validation)
- 🛡️ [Validating & Monitoring Attacks](#-validating--monitoring-attacks)
- 🚀 [Conclusion & Next Steps](#-conclusion--next-steps)

---

## 🎯 **Phase 1: Creating the Web ACL – The Defense Wall**  

![infowafdetailed](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/WAF%20AWS%20presentationdetailed.png)

### 👨‍💻 Step 1: Access AWS WAF  
![introwaf](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf%20presentation.png)
1. Log in to the **AWS console**.  
2. In the top search bar, type **"WAF & Shield"** and select the service.  


### 👨‍💻 Step 2: Create the Web ACL

![create acl](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf%201.png)
1. Under **Get started with AWS WAF**, click **Create web ACL**.  
2. **Web ACL Name** 🏷️
3. **CloudWatch metric name** 📊 
4. **Resource type**: Select **Regional resources** (Application Load Balancer, API Gateway, AWS AppSync, Cognito User Pools).  
5. Scroll down and click **Next**.  

> 📌 *Why "Regional resources"?*  
This is required if you're protecting **ALB, API Gateway, or AppSync**. If you're using **CloudFront**, select **"CloudFront distribution"** instead.  

---

## ⚔️ **Phase 2: Adding AWS Managed Rules – The First Layer of Defense**  

![managed rules](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf2.png)

AWS offers **Managed Rules** that provide immediate protection without manual configuration. Let’s activate them!  


### 👨‍💻 Step 3: Add AWS Managed Rules  
1. Under **Rules**, click **Add rules** > **Add managed rule groups**.  
2. Click **AWS managed rule groups** to expand the menu.  
3. **Enable the following protections**:  
   ✔️ **Core rule set**: Protects against common web attacks (XSS, LFI, RFI, etc.).  
   ✔️ **Known bad inputs**: Blocks known malicious input patterns.  
   ✔️ **SQL database**: Defends against SQL injection attacks.  
4. Scroll down and click **Add rules**.

![freerules](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf3.png)

> 📌 *Why these rules?*  
>- **Core rule set**: Provides **immediate** protection against various web threats.  
>- **Known bad inputs**: Blocks frequently used **malicious input patterns**.  
>- **SQL database**: Protects against **one of the most dangerous attacks** targeting databases.  

---

## 🔍 **Phase 3: Creating a Custom Rule – Advanced Protection**  

🔥 **Threat detected!**  
Our logs show attackers attempting to use **malicious HTTP headers** to inject commands. Let's stop them now!  

### 👨‍💻 Step 4: Create a Custom Rule for HTTP Header Injection

![ownrule](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf4.png)
1. Under **Rules**, click **Add rules** > **Add my own rules and rule groups**.  
2. Under **Rule type**, select **Rule builder**.  
3. Ensure **Rule visual editor** is selected.  

4. **Rule Name** 🏷️: Enter **HTTPHeaderInjection**.  
5. **Type**: Select **Regular rule**.  
![ruleset](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf5.png)

6. Under **If a request**, choose **matches the statement**.  
7. Under **Statement > Inspect**, select **Single header**.  
8. **Header field name**: Enter **User-Agent**.  
9. **Match type**: Select **Exactly matches string**.  
10. **String to match**: Enter **`%0d%0aLocation`**.  
11. **Text transformation**: Set to **None**.  
12. **Action**: Select **Block** (to block the request).  
![blockrule](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf6.png)

💡 **Why this rule?**  
Attackers often use **special characters** in the **User-Agent** header to **inject malicious code** and **redirect users**. This rule blocks such requests instantly.  

🎯 Click **Add rule** once all settings are configured.  
![add all rules](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf7.png)

---

## 🚀 **Phase 4: Final Configuration & Validation**  

### 🎯 Step 5: Prioritizing Rules  
![ruleorder](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf8.png)
1. On the **Add rules and rule groups** page, click **Next**.  
2. Under **Set rule priority**, select **HTTPHeaderInjection** and move it to **second position**.  
3. Click **Next**.  

> 📌 *Why prioritize rules?*  
AWS Managed Rules should be evaluated first, but **our custom rule must be high in the list** to block critical attacks immediately.  

---

### 🎯 Step 6: Verify CloudWatch Metrics
![cw](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf9.png)
1. Ensure that each rule has **an active CloudWatch metric**.  
2. Click **Next**.  

> 📌 *Why CloudWatch?*  
CloudWatch allows **real-time attack monitoring** and helps adjust rules when needed.  

---

### 🎯 Step 7: Final Web ACL Creation  
![review](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf10.png)

![2ndreview](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf11.png)
1. On the **Review and create web ACL** page, check that everything is correct.  
2. Scroll down to **Web ACL rule capacity units used** and ensure it does not exceed **1500 WCUs**.  
3. Click **Create web ACL**.  

🎉 **Congratulations!** You have successfully created an **AWS WAF shield** against cloud threats! 🎯🔥  
![done](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf12.png)
---

## 📊 **Validating & Monitoring Attacks**  

Now that the WAF is in place, here’s how to **verify it’s working**:  

✔️ **Monitor logs in AWS WAF**  
1. Go to your **waf** Web ACL.  
2. Click on **Logging and metrics**.  
3. Enable **AWS WAF logs** to see which requests are being blocked.

![wafdashboard](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Web%20Defense%20-%20Implementing%20AWS%20WAF/Screenshots/waf13.png)

✔️ **Analyze attacks with CloudWatch**  
1. Open **CloudWatch Metrics**.  
2. Look for **AWS WAF** metrics to visualize attack trends.  

> Note : Obviously we won't have metrics nor actions from the WAF because it is configured against header injections. So unless someone really does it, we won't see. And it is against AWS's rule to do it so. This project was more on "how to do it" side. 

---

## 🔥 **What have we achieved**  

✅ We have deployed a **WAF firewall** capable of detecting and blocking real-time attacks.  
✅ We used a **combination of AWS Managed Rules** and an **advanced custom rule**.  
✅ We enabled **CloudWatch Metrics** for continuous monitoring.  

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

