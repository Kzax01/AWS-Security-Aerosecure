## 📌 PART 1 - How to Secure Access with Permissions Boundaries in AWS IAM.

<p align="center">
  <img src="https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/IAM%20limiting%20privilege%20%20access.gif" width="800">
</p>

<p align="center">
  <strong>Welcome to the 1st part of this AWS IAM Permissions Boundaries project! 🚀</strong>
</p>


---

## 🛫 Scenario: Securing IAM Access for SkyFleet

As a **Cloud Security Engineer** at **AeroSecure**, you're tasked with securing the cloud infrastructure for **SkyFleet**, a company working on autonomous drone flight management. The system contains critical data (flight paths, flight parameters), and it must be protected from unauthorized access.

### 🚨 The Problem
SkyFleet's cloud administrators currently have broad `AdministratorAccess` permissions, posing significant risks if compromised.

### 🎯 The Goal
Your mission is to enforce the **Least Privilege** principle by implementing **Permissions Boundaries**. This will ensure that:
- ✅ Admins only manage essential resources like EC2 or S3.
- ✅ Admins can't create new users or alter IAM configurations.
- ✅ AWS IAM blocks any actions beyond the defined permissions.

---

## 🔎 Why is Least Privilege Crucial?

| **Concept**           | **Definition**                                                              | **Why it Matters**                                             |
|-----------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------|
| **Least Privilege**    | Grant only necessary permissions for a task.                                | Minimizes the attack surface and limits the impact of compromises. |
| **Permissions Boundaries** | Restrict what users can do, even if they have other broad IAM permissions. | Prevents privilege escalation, reducing the chance of unauthorized access. |

### Consequences of Poor IAM Management:
- **Risk of Full Access**: A compromised admin account could give attackers full access.
- **Unauthorized Privileges**: A user could escalate their privileges to unauthorized services.

---

## 📖 What You’ll Learn
- ✅ Creating IAM groups and managing permissions.
- ✅ Applying **Permissions Boundaries** to restrict access.
- ✅ Testing and verifying Least Privilege implementation.

## 🚀 Impact of the Project
- ✅ A hands-on demonstration of the **Least Privilege** principle.
- ✅ Real-world solution for securing access in sensitive environments.
- ✅ Practical knowledge applicable for any company using AWS.

---

Now that you've understood the importance of restricting permissions, are you ready to dive into securing access with **Permissions Boundaries**? Let’s get started! 🚀

### What's Next?
In the next step, we’ll explore how to monitor and audit IAM usage in AWS to ensure compliance and security.

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
