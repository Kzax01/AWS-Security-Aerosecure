# 🚀 AeroSecure Scenario: Creating and Securing Customer Managed Keys with AWS KMS

Welcome to the exciting world of cloud security!

---

## 🛰 Context
You're a **Cloud Security Engineer** at **AeroSecure**, an Airbus initiative dedicated to securing cloud infrastructures in the aerospace industry. AeroSecure handles highly sensitive data, including satellite communications and critical systems for aircraft.

---

## 🚨 Problem
To protect confidential data stored on **Amazon S3**, AeroSecure needs to encrypt sensitive files and ensure that only authorized individuals can access them. **AWS Key Management Service (KMS)** will be used to manage the encryption keys and enhance cloud storage security.

## 🎯 Lab Objectives
1️⃣ Create a symmetric KMS key for encryption.  
2️⃣ Set default encryption on an S3 bucket using this key.  
3️⃣ Schedule secure deletion of the KMS key.

---

## 📌 AWS KMS: Definition & Importance

| **Concept**            | **Definition**                                                               |
|------------------------|-------------------------------------------------------------------------------|
| **AWS KMS**            | AWS service to manage and use encryption keys for securing data.              |
| **Customer Managed Key (CMK)** | A KMS key created and managed by the user, providing more control over access. |
| **Data Encryption**    | Transforms data into an unreadable format without the decryption key.         |
| **S3 Integration**     | Enables using KMS to encrypt files stored on an S3 bucket.                    |
| **Regionality**        | A key can be single-region (usable in one AWS region) or multi-region.        |
| **Key Deletion**       | Keys can be scheduled for deletion with a waiting period of 7 to 30 days to prevent accidental data loss. |

---

## ⚙️ Lab Steps

### 🔹 1. Create an S3 Bucket  
📍 **Actions**:
1. Navigate to **Amazon S3** > **Create bucket**.  
2. Provide a unique name for the bucket.  
3. Keep the default settings and create the bucket.  
📌 **Why?**  
This bucket will store sensitive files that will be encrypted with KMS.

### 🔹 2. Create a Symmetric KMS Key  
📍 **Actions**:
1. Go to **AWS KMS** > **Create key**.  
2. Choose **Symmetric key** and **Encrypt/Decrypt**.  
3. Select **Single-Region Key** (AeroSecure security requirement).  
4. Give the key an alias (e.g., `cloud_user`).  
5. Define the key administrators (e.g., `Kzax01`).  
6. Apply the access management policy.  
7. Create the key.  
📌 **Why?**  
This key will be used to encrypt sensitive files in S3 and limit access using IAM/KMS policies.

### 🔹 3. Attach KMS Key to S3 Bucket (SSE-KMS Encryption)  
📍 **Actions**:
1. Go to **S3**, select the bucket.  
2. Navigate to the **Properties** tab.  
3. Under **Default encryption**, click **Edit**.  
4. Select **Server-side encryption with AWS KMS keys (SSE-KMS)**.  
5. Choose the KMS key created (e.g., `alias/cloud_user`).  
6. Enable **Bucket Key** to reduce KMS access costs.  
7. Save changes.  
📌 **Why?**  
This ensures all objects uploaded to this bucket are automatically encrypted using the defined KMS key.

### 🔹 4. Verify Encryption  
📍 **Actions**:
1. Upload a file to the S3 bucket.  
2. Click the file and go to **Server-side encryption settings**.  
3. Verify that **SSE-KMS** with the `alias/cloud_user` key is applied.  
📌 **Why?**  
This ensures that all stored objects are protected by AWS KMS.

### 🔹 5. Schedule Key Deletion  
📍 **Actions**:
1. Return to **AWS KMS**.  
2. Select the key `cloud_user`.  
3. Go to **Key Actions** > **Schedule Key Deletion**.  
4. Set a waiting period (between 7 and 30 days).  
5. Check the confirmation box and validate the deletion.  
📌 **Why?**  
Once a key is deleted, all data encrypted with it becomes inaccessible. The waiting period allows cancellation of the deletion in case of error.

---

## ⚠️ Constraints and Points of Caution

- **Key Deletion**:  
  - Once a key is permanently deleted, the files encrypted with it will no longer be accessible.  
  - The deletion can be canceled as long as the waiting period (7–30 days) has not elapsed.

- **Bucket Key Usage**:  
  - Reduces costs by minimizing KMS access calls.  
  - Does not work with **DSSE-KMS** (Double Server-Side Encryption).

- **Security & IAM**:  
  - Always restrict access to KMS keys with proper IAM policies.  
  - Never share a KMS key with public access.

---

## 🎯 Summary of Actions Performed  
✅ Created an S3 bucket to store sensitive files.  
✅ Created and configured a KMS key to encrypt data.  
✅ Enabled SSE-KMS encryption on the S3 bucket.  
✅ Verified that encryption was applied.  
✅ Scheduled key deletion for secure lifecycle management.

---

## 🏆 Conclusion  
You’ve successfully secured AeroSecure’s sensitive data by implementing strong encryption with **AWS KMS** and **Amazon S3**. 🚀  
This kind of implementation is crucial in aerospace, where the protection of communications and critical systems is paramount.

---

## 🔹 Next Step?  
Integrate **AWS GuardDuty** & **Security Hub** to detect threats on AWS access and operations! 🔍
