# 📌 Creating and Securing Customer Managed Keys with AWS KMS

Welcome back to this exciting project! 

## 🎯 The Mission :
As we seen earlier, the scenario focuses on ensuring the confidentiality of sensitive data stored in **Amazon S3** for **AeroSecure**, where your task is to enable encryption using **AWS KMS**, apply it to an S3 bucket, and implement key deletion scheduling for effective lifecycle management.

# 📖 Table of Contents  

  - [🔹 Creating an S3 Bucket](#-creating-an-s3-bucket)  
  - [🔹 Creating a KMS Key](#-creating-a-kms-key)  
  - [🔹 Enabling SSE-KMS Encryption](#-enabling-sse-kms-encryption)  
  - [🔹 Verifying Encryption](#-verifying-encryption)  
  - [🔹 Scheduling Key Deletion](#-scheduling-key-deletion)  

- [⚠️ Considerations](#-considerations)

---
# ⚙️ Project's Steps  

## 🔹 1. Creating an S3 Bucket
![s3bucket](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/s3%20page.png)

📍 **Actions:**  
1. Navigate to **Amazon S3** > Create a bucket.  
2. Provide a **unique name** for the bucket.  
3. Keep the **default settings** and create the bucket.  

📌 **Why?**  
The bucket will be used to store **sensitive files**, which will be encrypted with **AWS KMS**.  

## 🔹 2. Creating a Symmetric KMS Key  
📍 **Actions:**  
1. Go to **AWS KMS** > Create a key.  
![config key](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%202.1.png)
2. Select **Symmetric Key** and **Encrypt/Decrypt**.  
3. Choose **Single-Region Key** (AeroSecure’s security requirement).
4. Assign an **alias** to the key  
![alias](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%202.2.png)
5. Define **key administrators**
![define key admin](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%202.3.png)
6. Apply the **access control policy**.  
![key policy](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%202.4.png)
7. Create the key.  
![key created](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/successfulkey.png)

➡️ FYI  I used this policy :
```
{
  "Id": "key-consolepolicy-3",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<account-id>:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow access for Key Administrators",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<account-id>:user/<username>"
      },
      "Action": [
        "kms:Create*",
        "kms:Describe*",
        "kms:Enable*",
        "kms:List*",
        "kms:Put*",
        "kms:Update*",
        "kms:Revoke*",
        "kms:Disable*",
        "kms:Get*",
        "kms:Delete*",
        "kms:TagResource",
        "kms:UntagResource",
        "kms:ScheduleKeyDeletion",
        "kms:CancelKeyDeletion",
        "kms:RotateKeyOnDemand"
      ],
      "Resource": "*"
    }
  ]
}
```
    
> This policy grants full access to AWS KMS actions for the root user of the specified AWS account. 
It also allows a specific IAM user to manage key-related actions, including creating, updating, deleting, and rotating keys, but restricts them to those specific KMS operations.


📌 **Why?**  
This key will encrypt **sensitive files** in S3 and restrict access using **IAM/KMS policies**.  

## 🔹 3. Attaching the KMS Key to the S3 Bucket (SSE-KMS Encryption)  
📍 **Actions:**  
1. Navigate to **S3**, select the bucket.  
![property](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%203.1.png)
2. Go to the **Properties** tab.  
3. In **Default Encryption**, click **Edit**.
4. Select **Server-side encryption with AWS Key Management Service keys (SSE-KMS)**.  
5. Choose the **created KMS key**  
6. Enable **Bucket Key** to reduce KMS access costs.  
7. Save the changes.
![edit default encryption](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%203.2.png)

📌 **Why?**  
Ensures that all **uploaded objects** in this bucket are automatically **encrypted** using the defined **KMS key**.  

## 🔹 4. Encryption Verification  
📍 **Actions:**  
![s3upload](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/4.2%20s3%20upload.png)
1. Upload a file to the **S3 bucket**.  
2. Click on the file and go to **Server-side encryption settings**.  
3. Verify that **SSE-KMS** with the **alias/<user_name>** key is applied.
![sse-kms](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%204.1.png)

📌 **Why?**  
This ensures that all **stored objects** are properly **protected** by AWS KMS.  

## 🔹 5. Scheduling the Deletion of the KMS Key  
📍 **Actions:**  
1. Go back to **AWS KMS**.  
![kms](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%20page.png)
2. Select the **key you created**   
3. Navigate to **Key Actions** > **Schedule Key Deletion**.  
![keyactions](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%205.png)
4. Set a **waiting period** (between **7 and 30 days**).  
5. Check the confirmation box and confirm deletion.  
![deletion](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/AWS%20Key%20Management%20Service%20(KMS)/screenshots/kms%205.1%20deletion%20key.png)

📌 **Why?**  
A **deleted key** makes all **encrypted data permanently inaccessible**.  
The **waiting period** allows cancellation in case of an error.  

---

# ⚠️ Constraints and Considerations  

## 🔹 KMS Key Deletion:  
- Once a key is **permanently deleted**, files encrypted with it **cannot be accessed**.  
- Deletion can be **canceled** as long as the **waiting period (7-30 days)** has not expired.  

## 🔹 Using the Bucket Key:  
- Reduces costs by **minimizing calls** to AWS KMS.  
- **Not compatible** with **DSSE-KMS** (Double Server-Side Encryption).  

## 🔹 Security & IAM:  
- Always **restrict** access to **KMS keys** with a **strict IAM policy**.  
- **Never** expose a **KMS key** to **public access**.  

---

# 🎯 Summary of Completed Actions  
✅ Created an **S3 bucket** for storing **sensitive files**.  
✅ Created and configured a **KMS key** for **data encryption**.  
✅ Enabled **SSE-KMS encryption** on the S3 bucket.  
✅ Verified that **encryption** is properly applied.  
✅ Scheduled the **KMS key deletion** for **secure lifecycle management**.  

---

# 🏆 Conclusion  
You have successfully **secured AeroSecure’s sensitive data** by implementing **strong encryption** with **AWS KMS and Amazon S3**. 🚀  
This type of implementation is **critical in aerospace**, where protecting **communications and critical systems** is a top priority.  





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

