# 📌 Configure Privileged User Access by Setting Permissions Boundaries in AWS IAM 🔒

Welcome back to this exciting project! 

## 🎯 The Mission :
As we seen earlier, the scenario involves securing the cloud infrastructure for **SkyFleet**, where the risk arises from broad `AdministratorAccess` permissions granted to cloud admins, and your mission is to implement **Permissions Boundaries** to enforce the **Least Privilege** principle and restrict access to critical resources.

## 📋 Table of Contents:

- 👥 [Create Admin Group](#create-admin-group)
- 🛡️ [Set Permissions Boundaries](#set-permissions-boundaries)
- 🧪 [Test Access Controls](#testing-the-boundaries)


--- 

## Step 1 : Create a Group Controlled via an AWS-Managed Policy and Assign Users to a Group 👥


1. **Go to IAM**  
   - Log in to the AWS Console  
   - Navigate to **IAM > Users**  
   
![create user](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/1-users_test_creation.png)

2. **Create a New User**  
   - Click **Create User**  
   - Enter a name and confirm : we'll need at least 2 users to test the permissions boundaries.

![ourtestuser](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/2_users_tests.png)


3. **Access User Groups** 🔄

   - Click **User groups** in the left navigation panel
> 💡 User groups allow you to assign permissions to multiple users at once, simplifying access management
   

4. **Create a new group** ➕
   - Click **Create group**
   - For the **User group name**, enter "SysAdmins-Kzax01project"
> 💡Choose a descriptive name that reflects the group's purpose and responsibilities

6. **Add users to the group** 👪
   - In the *Add users to the group - Optional* section, select:
     * *sysadmin-1*
     * *sysadmin-2*
     * *sysadmin-3*

 ![usergp](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/3_policy_attached.png) 
> 💡This adds these pre-existing users to your new group, so they'll inherit the group's permissions
 

7. **Attach the administrator policy** 🔑
   - In the *Attach permissions policies - Optional* section, select the **AdministratorAccess** policy

> 💡This policy grants full access to all AWS services and resources, making it extremely powerful

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "ec2:*",
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "elasticloadbalancing:*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "cloudwatch:*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "autoscaling:*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:AWSServiceName": [
                        "autoscaling.amazonaws.com",
                        "ec2scheduled.amazonaws.com",
                        "elasticloadbalancing.amazonaws.com",
                        "spot.amazonaws.com",
                        "spotfleet.amazonaws.com",
                        "transitgateway.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```


8. **Finalize group creation** ✅

![usergrp](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/4-group_user_test_created.png)
   
   - Review your selections to ensure accuracy
   - Click **Create group**
   - This creates the group with the selected users and permissions

---

## Step 2 : Limit Privileged Users by Setting Permissions Boundaries 🛡️

### Set EC2 Boundary for sysadmin-2

1. **Access the SysAdmins group** 🔍
   - Click **SysAdmins** under *Group name*
   - This takes you to the group management page
   

2. **Select the user to modify** 👤
   - Click **sysadmin-2**
   - This takes you to this specific user's configuration page
   

3. **Access permissions boundary settings** 📋
   - Click the arrow next to **Permissions boundary (not set)** to expand it
   - Permissions boundaries act as an upper limit on what a user can do, regardless of what their attached policies allow
![PBEC2](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/5-setPB-EC2-test-users.png)

4. **Begin setting the boundary** 🚧
   - Click **Set boundary**
   - This initiates the process of adding a permissions boundary
   

5. **Find the EC2 policy** 🔎
   - In the **Filter policies** field, type "ec2"
> 💡 This filters the available policies to show only those related to EC2
![ec2](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/6-PB%20ec2.png)

6. **Select EC2 policy** 💻
   - Select **AmazonEC2FullAccess**
> 💡This policy allows full access to EC2 but nothing else, which will become the maximum permissions this user can have
   

7. **Apply the boundary** ✅
   - Click **Set boundary**
   - This finalizes the permissions boundary, restricting this admin to only EC2 operations despite having AdministratorAccess through the group

![ec2done](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/7-pb-ec2-set.png)

---

### Step 3 : Set S3 Boundary for sysadmin-S3

➡️ Repeat the same steps as sysadmin-ec2


1. **Go to IAM > Users**, select **sysadmin-3**, and expand **Permissions boundary**.  
2. **Click "Set boundary"**, filter for **"s3"**, select **AmazonS3FullAccess**, and confirm. ✅  
3. **Apply the boundary**  

   > 💡 This completes the boundary setting, limiting this admin to only S3 operations
   
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:*"
            ],
            "Resource": "*"
        }
    ]
}
```

---

## Step 4 : Verifying Limits on Privileged Users 🧪

### Test sysadmin-3 (S3-Only Admin)

1. **Sign out as current user** 🚪

2. **Enter sysadmin-S3 credentials** 🔐
 
3. **Attempt to access EC2** 🚫
   - Navigate to **EC2** in the AWS Management Console
   - This is testing whether the permissions boundary is working

You should see this :
![failed1](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/10-ec2-cannot-be-created.png)
   

4. **Try viewing EC2 instances** ❌
   - Click **Instances (running)**
   - You should see an error message indicating you're not authorized
   - This confirms the boundary is preventing access to EC2 resources
   

5. **Attempt to launch an EC2 instance** ❌
   - Click **Launch Instances**
   - Click **Select** on *Amazon Linux 2 AMI (HVM), SSD Volume Type*
   - You should receive another access denied message
   - This further confirms the boundary is working correctly

![ec2failed2](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/8-ec2-cannot-create.png)
   

8. **Return to main console** 🏠
   - Navigate back to the AWS Management Console
   - This takes you back to the service selection page
   

9. **Access S3 service** ✅
   - Click **S3**
   - Since this user has S3 permissions, this should work
   

10. **Test bucket creation** 📦
    - Click **Create bucket**
    - Under **General configuration**, enter a unique name like "Buckettestkzax01"
    - Select **US East (N. Virginia) us-east-1** for the AWS Region
    - This tests whether S3 operations are permitted
    

11. **Finalize bucket creation** ✅
    - Click **Create bucket**
    - The bucket should be created successfully
    
![S3created](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/11-s3-created.png)

> 💡 This confirms that S3 operations are allowed within the boundary.

➡️ Now Log out from the sysadmin-S3 account and log back in as Sysadmin-EC2.

---

### Step 5 : Test sysadmin-EC2 (EC2-Only Admin)

1. **Attempt to access S3** 🚫
   - Navigate to **S3**
   - You should receive an "Access Denied" message
   - This confirms that the EC2-only admin cannot access S3 resources

![nos3](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/12-s3-cannot-be-created.png)

2. **Launch EC2**
- Unlike the previous one, creating an EC2 will actually work as per as the permission assigned.

[ec2created](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/IAM/01_Limiting_Privileged_Access_IAM/screenshots/13-ec2created%20test.png)

---

## What have we learned from this Project ? 

- Created a SysAdmins group with AdministratorAccess policy ✓
- Added three system administrators to the group ✓
- Set a permissions boundary on sysadmin-2 limiting them to EC2 operations only ✓
- Set a permissions boundary on sysadmin-3 limiting them to S3 operations only ✓
- Verified that the boundaries correctly limit access despite group permissions ✓

You've successfully implemented the principle of least privilege while maintaining the operational effectiveness of your administrators! 🎉

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

