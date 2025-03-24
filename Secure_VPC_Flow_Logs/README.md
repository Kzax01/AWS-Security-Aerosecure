## 📌 How to Secure VPC Flow Logs in AWS 

<p align="center">
  <img src="https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/Secure_VPC_Flow_Logs/screenshots/_Secure%20VPC%20Flow%20Logs%20in%20AWS.gif" width="80%" />
</p>

<p align="center">
  <b>Welcome to this new project!</b>
</p>


---

## 🚨 Attack Context
Cybercriminals are trying to infiltrate our infrastructure by exploiting network security vulnerabilities. Their goal is to gain unauthorized access to critical data. Our mission is to identify their attempts, block their access, and alert our teams for a rapid response.

---

## 🎯 Mission Objectives
- **Monitor** network traffic using **VPC Flow Logs**.
- **Analyze** logs to identify suspicious login attempts (SSH, HTTP, etc.).
- **Detect** and **respond** to intrusion attempts using **CloudWatch** and **Athena**.
- **Automate** monitoring and alerting for real-time response.

---

## 🔎 Action Plan 
1. **Activating VPC Flow Logs**  
   Capture incoming and outgoing network traffic from our VPC, sending the logs to **S3** and **CloudWatch Logs** for real-time analysis and long-term storage.

2. **Log Analysis with CloudWatch Logs Insights**  
   Use specific queries to detect rejected SSH login attempts and identify suspicious activities on our network infrastructure.

3. **Generating Alerts via CloudWatch Alarms**  
   Set up alerts for any rejected SSH login attempts to notify the **AeroSecure SOC** via **SNS** in case of an incident.

---

## 💥 Impact of the Mission
This operation strengthens our **threat detection** capabilities within **Airbus'** AWS environment. It demonstrates the importance of monitoring network activity in real-time, reacting swiftly to incidents, and automating responses for a more efficient SOC.

- ✅ **Network monitoring** using **VPC Flow Logs**.
- ✅ **Intrusion detection** with **CloudWatch Logs**.
- ✅ **Real-time alerts** via **CloudWatch Alarms**.
- ✅ **Advanced log analysis** with **Amazon Athena**.

---

## 🛡️ Lessons Learned
- **Configuring and leveraging VPC Flow Logs** for advanced network monitoring.
- Using **CloudWatch Logs Insights** and **Amazon Athena** for **intrusion detection** and analysis.
- Automating **alerts and responses** to improve reaction time in the event of an attack.

---

## 🚀 Application to AeroSecure
This project represents a significant step towards an **autonomous AWS SOC**, with the potential to integrate this solution with **AWS Security Hub** for centralized threat analysis. It will enhance the security posture of **Airbus'** infrastructure.

---

## 🔜 Next Step: 
We'll deep dive into the fantastic world of SecurityHud & its findings! 
---

## 📖 Glossary
| **AWS Service**        | **Description**                                                                 |
|------------------------|---------------------------------------------------------------------------------|
| **VPC Flow Logs**       | Captures network traffic metadata from VPC network interfaces.                  |
| **CloudWatch Logs**     | Centralizes and analyzes logs in real-time to detect anomalies.                 |
| **CloudWatch Alarms**   | Creates alerts based on metrics extracted from logs.                            |
| **Amazon SNS**          | Sends notifications in real-time when alerts are triggered.                     |
| **Amazon S3**           | Stores logs for later analysis and long-term retention.                         |
| **Amazon Athena**       | Runs SQL queries on data stored in **S3** for detailed analysis.                |

---

🎯 Mission accomplished, agent! 🚀 You've set up an efficient monitoring and detection system to protect **Airbus'** infrastructure. With this approach, potential threats can be swiftly identified and neutralized. Ready for your next mission? 🌟

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
