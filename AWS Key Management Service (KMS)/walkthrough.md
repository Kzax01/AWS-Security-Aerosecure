# 📌 Creating and Securing Customer Managed Keys with AWS KMS

Welcome back to this exciting project! 

## 🎯 The Mission :
As we seen earlier, the scenario focuses on ensuring the confidentiality of sensitive data stored in **Amazon S3** for **AeroSecure**, where your task is to enable encryption using **AWS KMS**, apply it to an S3 bucket, and implement key deletion scheduling for effective lifecycle management.

## 📋 Table of Contents
- 🗝️ [Create KMS Key](#create-a-single-region-symmetrical-kms-key)
- 🔒 [Configure S3 Encryption](#set-up-amazon-s3-default-encryption-via-kms)
- ⏱️ [Schedule Key Deletion](#schedule-aws-kms-key-deletion)
- 💡 [Security Benefits](#why-these-steps-matter)

---

## 🗝️ Create a Single-Region Symmetrical KMS Key 

1. **Access Amazon S3**
   - Navigate to Amazon S3 using the **Services** menu or the unified search bar
   - This is your starting point for creating a KMS key that will be used with S3
   

2. **Verify your bucket**
   - Confirm you see a bucket that starts with the **cloud-user** prefix
   - This ensures you're working with the correct resources for this lab
   

3. **Open bucket properties**
   - Click on the bucket to open it
   - Navigate to the **Properties** tab
   - The Properties tab contains configuration settings for your bucket
   

4. **Access KMS service**
   - Using the search bar, type and select **Key Management Service**
   - KMS is AWS's service for creating and managing encryption keys
   

5. **Initiate key creation**
   - Under **Get started now**, click **Create a key**
   - This begins the workflow for creating a new Customer Managed Key (CMK)
   

6. **Select key type**
   - Under **Key type**, ensure **Symmetric** is selected
   - Symmetric keys use the same key for both encryption and decryption
   

7. **Define key usage**
   - Under **key usage**, ensure **Encrypt and decrypt** is selected
   - This determines what operations the key can perform
   

8. **Configure advanced options**
   - Under **Advanced options**, ensure:
     * **Regionality** is set to **Single-Region key**
     * **Key material origin** is set to **KMS**
   - Single-region keys exist in one AWS region only
   - KMS key material origin means AWS generates the key material
   

9. **Proceed to key configuration**
   - Click **Next**
   - This takes you to the administrative settings
   

10. **Name your key**
    - Under **Alias**, enter *any name you want* (Mine is kzax01)
    - The alias provides a user-friendly name for your key
    

11. **Continue to "edit key policy"**
    - I used this policy :
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

12. **Assign key administrators**
    - Under **Key administrators**, click on the checkbox next to **your user**
    - Key administrators can manage but not use the key for cryptographic operations
    

13. **Configure deletion settings**
    - Under **Key deletion**, ensure **Allow key administrators to delete this key** is selected
    - This permits authorized users to schedule key deletion
    

14. **Complete key creation**
    - Click **Next** until you reach the **Review** page
    - Review all settings and click **Finish**
    - This creates your KMS key with the specified configuration

---

## Set Up Amazon S3 Default Encryption via KMS 🔒

1. **Return to S3 service**
   - Navigate to Amazon S3 using the shortcut bar or the unified search bar
   - Click on your bucket ; Mine was called kzax01bucket
   - Navigate to the **Properties** tab
> This is where you'll configure encryption settings 

2. **Edit encryption settings**
   - Scroll down to **Default encryption**
   - Click **Edit**
   - Default encryption ensures all objects uploaded to the bucket are automatically encrypted
   

3. **Configure encryption settings**
   - Under **Default encryption**, set the following values:
     * **Encryption key type**: Select **AWS Key Management Service key (SSE-KMS)**
     * **AWS KMS key**: Select **Choose from your AWS KMS keys**
     * Under **Available AWS KMS keys**, click the dropdown menu and select **cloud_user**
     * **Bucket Key**: Select **Enable**
> SSE-KMS provides enhanced security over S3's default encryption.
Bucket Key reduces the cost of KMS operations by reusing encryption keys.
   

4. **Save encryption configuration**
   - Click **Save changes**
   - This applies your encryption settings to the bucket
   

5. **Test encryption with a file upload**
   - Navigate to the **Objects** tab
   - Click on **Upload**
   - This lets you verify the encryption settings are working
   

6. elect and upload a file**
   - Choose any file from your computer
   - Click **Upload** 
   - This sends the file to S3 where it will be encrypted with your KMS key.
   

7. **Complete the upload process**
   - Once you see the **Upload succeeded** banner
   - Click **Close** in the upper right corner
   - This returns you to the objects listing
   

8. **Verify encryption settings**
   - Click on the file you uploaded
   - Scroll down to **Server-side encryption settings**
   - Confirm you see your KMS key listed as the encryption method
   - This verifies successful implementation of KMS encryption

---

## Schedule AWS KMS Key Deletion ⏱️

1. **Access your KMS key**
   - Under **Encryption key ARN**, click on the key ARN
   - This takes you to the key management page in KMS
   

2. **Initiate key deletion**
   - In the upper right corner, click on the **key actions** dropdown menu
   - Select **Schedule key deletion**
   - Scheduling deletion starts a mandatory waiting period before the key is permanently removed
   

3. **Set waiting period**
   - Under **Waiting period**, enter *7* days
   - This waiting period gives you time to cancel deletion if needed
   - After this period, the key will be permanently deleted
   

4. **Confirm deletion**
   - Under **Confirmation**, select the checkbox next to:
   - **Confirm that you want to schedule these keys for deletion after a 7 day waiting period.**
   - This acknowledgment helps prevent accidental key deletion
   

5. **Complete scheduling**
   - Click **Schedule deletion**
   - This finalizes the deletion schedule
   - The key will be deleted after the 7-day waiting period

---

## Why These Steps Matter / What have we learned ? 💡

- **Creating a CMK** gives you full control over your encryption keys instead of using AWS-managed keys

- **Using KMS with S3** provides:
  - Enhanced security for sensitive data
  - Compliance with data protection regulations
  - Detailed audit logs of key usage

- **Bucket Keys** reduce the cost of KMS by decreasing the number of API calls

- **Proper key deletion** ensures:
  - Security best practices are followed
  - Data encrypted with deleted keys becomes unrecoverable
  - The waiting period prevents accidental deletion of keys still in use

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

