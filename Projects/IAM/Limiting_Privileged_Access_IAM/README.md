# 📌 GitHub Project: Securing Access with Permissions Boundaries in AWS IAM

Welcome to this exciting new project!

---

## 🛫 Scenario: IAM Security for an Aerospace Infrastructure

You are a Cloud Security Engineer at **AeroSecure**, a company that provides secure cloud solutions for the aerospace industry. One of your clients, **SkyFleet**, is developing a flight management system for autonomous drones. This system must be secured to prevent unauthorized access to the drones' critical data (flight paths, flight parameters, etc.).  
SkyFleet uses AWS IAM to manage access for its cloud administrator team, but it's crucial to implement the **Least Privilege** principle to avoid any excessive access risks.

---

## 🛑 Problem  
Currently, the administrators have overly broad permissions through the `AdministratorAccess` policy, which poses a significant risk in case of compromise.

## 🎯 Goal  
You need to restrict the administrators' access by setting up **Permissions Boundaries** in IAM to ensure that:
- ✅ An admin can only manage the resources they need (EC2, S3, etc.)  
- ✅ An admin cannot create new users or modify IAM  
- ✅ Any actions outside the defined permissions are blocked by AWS IAM

---

## 🔎 Explanation: Why is Least Privilege important?  

| **Concept**           | **Definition**                                                                      | **Why is it important?**                                                    | **Consequences of poor implementation**                        |
|-----------------------|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------|----------------------------------------------------------------|
| **Least Privilege**    | Grant only the necessary permissions to perform a specific task.                    | Reduces the attack surface and limits the impact of a compromise.           | An attacker could gain full access if an admin account is compromised. |
| **Permissions Boundaries** | Define the maximum actions a user can perform, even if they have other IAM permissions. | Prevents an IAM user from elevating their own privileges and accessing unauthorized services. | A user could grant themselves admin rights without restrictions. |

---

## 📖 What You’ll Learn in This Lab  
- ✅ Creating IAM groups and assigning permissions  
- ✅ Applying a Permissions Boundary to restrict access  
- ✅ Testing the restrictions and verifying the application of the Least Privilege principle

---

## ⚙️ Project Steps  

### **1️⃣ Create the Admin Group**  
📌 We’ll create a SysAdmins group and add three IAM users.  
1. Create an IAM SysAdmins group  
2. Add users `sysadmin-1`, `sysadmin-2`, `sysadmin-3`  
3. Attach the `AdministratorAccess` policy (temporarily)

### **2️⃣ Define Permissions Boundaries**  
📌 We’ll restrict the admins' access by applying specific permissions.  
1. Go to IAM > Groups > SysAdmins  
2. Select `sysadmin-2` and add a Permissions Boundary for EC2 only (`AmazonEC2FullAccess`)  
3. Select `sysadmin-3` and add a Permissions Boundary for S3 only (`AmazonS3FullAccess`)

### **3️⃣ Verify the Restrictions**  
📌 Test the admins' access to ensure the restrictions are properly applied.  
1. Log in as `sysadmin-2` and try to:  
   - Launch an EC2 instance (success ✅)  
   - Create an S3 bucket (failure ❌: access denied)  
2. Log in as `sysadmin-3` and try to:  
   - Create an S3 bucket (success ✅)  
   - Launch an EC2 instance (failure ❌)

---

## 🚀 Impact of the Project  
- ✅ Clear demonstration of implementing Least Privilege in AWS IAM  
- ✅ Real-world example of restricting access in a sensitive environment  
- ✅ A practical and applicable project for any company using AWS

---

That's how you secure your AWS environment with the **Least Privilege** principle and **Permissions Boundaries**. Ready to dive into it? Let's get started! 🚀

### Next, we will have a look on how to monitor, audit the resource usage of users in AWS IAM