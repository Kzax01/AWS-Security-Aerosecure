# 📌 How to use AWS KMS : Creating and Securing Customer Keys

<p align="center">
  <img src="https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/AWS%20KMS%20readme%20visu.gif" alt="AWS KMS Readme Visu" />
</p>

<p align="center">
  Welcome to this new exciting project!
</p>

---

## 🚀 AeroSecure Scenario: Creating and Securing Customer Managed Keys with AWS KMS

At **AeroSecure**, security teams have identified that some files stored in **Amazon S3** are not adequately protected due to the lack of default encryption methods.  
The mission is to create a **customer-managed key** in **AWS KMS**, configure it as the default encryption method for a specific S3 bucket, and then implement a secure key lifecycle management process.

---

## 🚨 Problem
To ensure the confidentiality of sensitive data stored in **Amazon S3**, AeroSecure needs to enable encryption for specific files and enforce strict access control. The solution will involve using **AWS Key Management Service (KMS)** to create and manage encryption keys that secure the data and allow controlled access.

## 🎯 The Goals :
1️⃣ Create a **symmetric KMS key** for encryption.  
2️⃣ Apply the KMS key as the **default encryption method** for an S3 bucket.  
3️⃣ Implement **key deletion scheduling** to manage the lifecycle of the encryption key.

---

## 📌 AWS KMS: Definition & Importance

| **Concept**               | **Definition**                                                                |
|---------------------------|-------------------------------------------------------------------------------|
| **AWS KMS**               | A managed service to create and control encryption keys used to secure data.   |
| **Customer Managed Key (CMK)** | A KMS key created and managed by the user for more fine-grained control.      |
| **Data Encryption**       | Converts data into an unreadable format without the decryption key.            |
| **S3 Integration**        | Use KMS to encrypt files stored in Amazon S3.                                 |
| **Regionality**           | Keys can be **single-region** or **multi-region** depending on the setup.      |
| **Key Deletion**          | Schedule the deletion of keys after a grace period of 7 to 30 days.           |

---

## ⚙️ Lab Overview

### 🔹 1. Create an S3 Bucket
- Define a new S3 bucket to store sensitive files that will be encrypted using AWS KMS.

### 🔹 2. Create a Symmetric KMS Key
- Create a **symmetric key** within AWS KMS for encrypting your files.  
- Assign an alias to easily identify the key.

### 🔹 3. Attach KMS Key to S3 Bucket (SSE-KMS Encryption)
- Apply the KMS key to the S3 bucket as its default encryption method, ensuring all files uploaded to the bucket are automatically encrypted.

### 🔹 4. Verify Encryption
- Upload a test file to the S3 bucket and verify that it is encrypted using the KMS key.

### 🔹 5. Schedule Key Deletion
- Schedule the deletion of the KMS key after a set period to ensure secure key management and prevent unauthorized access.

---

## ⚠️ Constraints and Points of Caution

- **Key Deletion**:  
  - Once a KMS key is deleted, data encrypted with it becomes inaccessible.  
  - The deletion can be canceled within a waiting period of 7 to 30 days.

- **Bucket Key Usage**:  
  - The use of **Bucket Keys** can reduce costs by minimizing KMS access calls but is incompatible with **DSSE-KMS** (Double Server-Side Encryption).

- **Security & IAM**:  
  - Ensure that IAM policies are in place to restrict access to the KMS key and prevent unauthorized sharing.

---

## 🎯 Summary of Actions Performed
✅ Created a new S3 bucket for storing encrypted files.  
✅ Created and configured a KMS key to encrypt data.  
✅ Applied the KMS key for **SSE-KMS encryption** on the S3 bucket.  
✅ Verified that the encryption was successfully applied to the files.  
✅ Scheduled the KMS key deletion for secure lifecycle management.

---

## 🏆 Conclusion
Congratulations! You’ve successfully implemented **AWS KMS** and **S3 encryption** for AeroSecure, ensuring that sensitive data remains protected and managed according to best practices. This setup is crucial in sectors like aerospace, where securing communications and critical infrastructure is a top priority. 🚀

---

## 🔹 Next Step?
Integrate **AWS GuardDuty** and **AWS Security Hub** to continuously monitor and detect threats to your cloud resources. 🔍

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
