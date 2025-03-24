# 📌 Part 2 : Identifying and Remediating Threats with AWS Security Hub

Welcome back to this exciting project! 

## 🎯 The Mission :
As seen previously, the mission at AeroSecure is to protect StratoJet’s top-secret electric propulsion technology from cyber threats by simulating and analyzing a security breach using AWS Security Hub to track and neutralize the threat.

*Are you ready to accept this challenge?* Let's go! 

---

## 📋 Table of Contents

- 🛡️ [Phase 1: Enabling Security Hub and Deploying Honeypots](#phase-1-enabling-security-hub-and-deploying-honeypots)
- 👤 [Phase 2: Monitoring Suspect #1 - Martina's Activities](#phase-2-monitoring-suspect-1---martinas-activities)
- 🕵️‍♂️ [Phase 3: Analyzing Suspect #2 - George's Activities](#-phase-3-analyzing-suspect-2---georges-activities)
- 🕵️‍♀️ [Phase 4: Investigating Suspect #3 - Edward's Activities](#phase-4-investigating-suspect-3---edwards-activities)
- 📊 [The Investigation Results](#the-investigation-results-)
- 🔧 [Incident Response and Remediation Plan](#incident-response-and-remediation-plan)
- 🧠 [Key Takeaways](#key-takeaways-)


---

## Phase 1: Enabling Security Hub and Deploying Honeypots

> *In cybersecurity, the best trap is the one your adversary never sees coming. We'll set up honeypots to lure and identify our intruder.*

### Step 1: Configure AWS Config

AWS Config will be our surveillance system:

1. Type `Config` in the search bar at the top of the page
2. Select **Config** from the dropdown menu
3. Click **1-click setup** for rapid configuration
4. On the review page, click **Confirm** at the bottom
5. In the AWS Config Dashboard, navigate to **Settings** (left menu)


### Step 2: Activate Security Hub

Security Hub will serve as our command center:

1. Search for `Security Hub` in the search bar
2. Select **Security Hub** from the results
3. Click **Go to Security Hub**
4. On the activation page, click **Enable Security Hub** at the bottom

### Step 3: Deploy Your Honeypots

Now, let's set up our traps:

1. Open a new tab and search for `CloudFormation`
2. Right-click on **CloudFormation** and select **Open Link in New Tab**
3. Click **Create Stack > With new resources (standard)** in the top right
4. Under **Prerequisite**, select **Create template in designer**
5. Click **Create template in designer**
6. Open the lab GitHub repository in a new tab
7. Copy all the template code from GitHub
8. Return to the designer page, ensure the **Templates** tab is selected at the bottom
9. Paste the template code, replacing the existing code
10. In the menu bar (top left), click the validation icon (✓)
11. Then click the create stack icon (cloud with arrow)
12. On the **Create Stack** page, scroll and click **Next**
13. In **Stack name**, enter `AeroSecureStack`
14. Click **Next**
15. Scroll to **Capabilities**, check the box next to "I acknowledge..."
16. Click **Create stack**

### [➡️ FYI : Here's the link of the CloudFormation template used](https://github.com/arcuricm/Hands-on-Security-Hub/blob/main/OriginalChallenge.yml)

> *Patience, agent... Our trap deployment takes a few minutes. Meanwhile, let's return to our command center.*

17. Return to the Security Hub tab
18. Refresh the page to see your baseline security indicators under **Findings by Region**
19. Click the number in the **Critical findings** box to view the details
20. Return to the CloudFormation tab to verify your honeypots are deployed. You should see `CREATE_COMPLETE` under SecretServers

*Note: Building may take a few minutes. Refresh if necessary.*

---

## Phase 2: Monitoring Suspect #1 - Martina's Activities

Our first suspect enters the scene. Let's observe what Martina does in our environment.

### ➡ Logging in as Martina

1. Open a new tab in private browsing
2. Log into the AWS console with Martina's credentials
3. Ensure you're still in the **N. Virginia (us-east-1)** region

### ➡ Martina's Actions

> *Let's track Martina's steps to determine if they're legitimate or suspicious.*

#### ➡ Creating a VPC

1. Search for `VPC` in the search bar
2. Select **VPC** from the menu
3. Click **Create VPC**
4. Under **Resources to create**, verify **VPC and more** is selected
5. Scroll and click **Create VPC**

#### ➡ Launching an EC2 Instance

1. While the VPC is being created, search for `EC2`
2. Select **EC2** from the results
3. In the left menu, select **Key Pairs**
4. Click **Create key pair**
5. Name the key pair `AeroSecureKP`
6. Click **Create key pair**
7. Return to the **EC2 Dashboard** from the left menu
8. Select **Instances (running)**
9. Click **Launch instances**
10. Name the instance `AerosecureInstance`
11. Under **Instance type**, choose **t3.micro**
12. Under **Key pair**, select **AeroSecureKP**
13. Next to **Network settings**, click **Edit**, then under **Auto-assign public IP**, choose **Enable**
14. Click **Launch instance**

#### ➡ Managing IAM Access Keys

1. Search for `IAM` in the search bar
2. Select **IAM** from the results
3. In the left menu, select **Users**
4. Click on **Martina**
5. Select the **Security credentials** tab and click **Create access key**
6. Close the **Create access key** popup

#### ➡ Accessing S3 Buckets

1. Search for `S3` in the search bar
2. Select **S3** from the results
3. Under **Buckets**, click on the bucket starting with `cf-templates`
4. Under **Objects**, click on the object containing `designer` in its name

#### ➡ Deactivating and Deleting Access Keys

1. Return to the IAM console by searching for `IAM`
2. Select **IAM** from the results
3. In the left menu, select **Users**
4. Click on **Martina**
5. Select the **Security credentials** tab
6. Under **Access key ID**, click **Make Inactive** on the right
7. In the **Deactivate** popup, click **Deactivate**
8. Next to the **Make active** button, click **X** to delete the access key
9. In the **Delete** popup, confirm deletion by copy-pasting the access key
10. Click **Delete** to close the popup and delete the access key

 > *You can now close this tab. Martina's actions are complete.*

---
## 🕵️‍♂️ Phase 3: Analyzing Suspect #2 - George's Activities

Our second suspect, George the IAM and Compliance Specialist has now enters the game. Let's see what he does.

### ➡ Logging in as George

1. Open a new tab in private browsing
2. Log into the AWS console with George's credentials
3. Ensure you're still in the **N. Virginia (us-east-1)** region

### ➡ George's Actions 

> *Are George's actions normal for his role? Let's analyze them in detail.*

#### ➡ Creating IAM Access Keys

* Search for `IAM` in the search bar
* Select **IAM** from the results
* In the left menu, select **Users**
* Click on **George**
* Select the **Security credentials** tab
* Click **Create access key**
* In the popup, click **Download .csv file**, then close the popup

#### ➡ Accessing S3 and DynamoDB Resources

1. Search for `S3` in the search bar
2. Select **S3** from the results
3. Under **Buckets**, click on the first bucket starting with `cf-templates`
4. Under **Objects**, click on the first object containing `designer` in its name
5. On the designer page, check the box next to the object starting with `cf.template` and click **Download**
6. Search for `DynamoDB` in the search bar
7. Select **DynamoDB** from the results
8. In the left menu, select **Tables**
9. Copy the name of the listed table to your clipboard ( Mine was AeroSecureStack-DataTable-xxxx)

#### ➡ Connecting to the EC2 Instance

* Search for `EC2` in the search bar
* Select **EC2** from the results
* Under **Resources**, click **Instances (running)**
* Check the box next to **AdminServer** and click **Connect**
* On the **Connect to Instance** page, click **Connect**

#### ➡ Running Commands on the Instance

In the newly opened EC2 Instance Connect terminal:
1. Run `ls`
2. Run `pwd`
3. Run `sudo yum update`

*You can now close the EC2 and EC2 Instance Connect tabs. George's actions are complete.*

---

## Phase 4: Investigating Suspect #3 - Edward's Activities

Our final suspect, Edward : the Security Operations Lead is now under surveillance. His actions could reveal our hacker.

### Logging in as Edward

1. Open a new tab in private browsing
2. Log into the AWS console with Edward's credentials
3. Ensure you're still in the **N. Virginia (us-east-1)** region

### Edward's Actions

> *Let's closely track Edward's movements. Do his actions appear legitimate?*

#### ➡ Creating IAM Access Keys

1. Search for `IAM` in the search bar
2. Select **IAM** from the results
3. In the left menu, select **Users**
4. Click on **Edward**
5. Select the **Security credentials** tab
6. Click **Create access key**
7. In the popup, click **Download .csv file**, then close the popup

#### ➡ Connecting to EC2 Instance and Configuring AWS CLI

1. Search for `EC2` in the search bar
2. Select **EC2** from the results
3. Under **Resources**, click **Instances (running)**
4. Check the box next to **AdminServer** and click **Connect**
5. On the **Connect to Instance** page, click **Connect**

In the EC2 Instance Connect terminal:
1. Run `aws configure`
2. Open the downloaded CSV file, copy the AWS access key ID, and paste it when prompted
3. Copy the AWS secret access key and paste it when prompted
   *Note: Make sure to copy the entire secret key, as the end may be hidden in the downloaded document*
4. For the region, enter `us-east-1`
5. Accept the default output format by pressing Enter

#### ➡ Unauthorized Access to S3 and DynamoDB Data

1. Return to the EC2 tab and search for `S3`
2. Right-click on **S3** and select **Open Link in New Tab**
3. Under **Buckets**, copy the name of the first bucket : _aerosecurestack-databucket-xxxxx_
4. Click on this bucket
5. In the EC2 Instance Connect tab, run the following command (replacing the bucket name):
   ```
   aws s3 cp s3://[INSERT_BUCKET_NAME_HERE]/data-object /tmp
   ```
6. Return to the S3 tab and search for `DynamoDB`
7. Select **DynamoDB** from the results
8. In the left menu, select **Tables**
9. Copy the name of the table starting with `AeroSecureStack-DataTable-1OAWNRxxx`
10. In the EC2 Instance Connect tab, run a table scan with the command:
    ```
    aws dynamodb scan --table-name [INSERT_TABLE_NAME_HERE]
    ```
    
_Mine was : aws dynamodb scan --table-name AeroSecureStack-DataTable-1OAWNRxxx_

> *Edward's actions are now complete.*

---

## The Investigation Results 🔍

Let's return to our Security Hub console (logged in as cloud_user) and examine the findings:

1. Refresh the Security Hub page to see new alerts and findings
2. Particularly examine the Critical alerts
3. For each alert, analyze:
   - Which user is involved?
   - What action triggered the alert?
   - Is it a legitimate or malicious action?

---

### ➡️ The Verdict: **Edward is the compromised account! 🏴‍☠️**

➡ Key evidence pointing to Edward:
1. He accessed sensitive data in our S3 bucket containing our honeypots
2. He performed an unauthorized scan on a DynamoDB table containing sensitive information
3. These actions left traces in Security Hub as critical alerts

---

## Incident Response and Remediation Plan

Now that we've identified our hacker, here are the recommended remediation steps:

1. **Immediate Access Revocation**:
   - Disable all of Edward's access keys
   - Revoke his IAM permissions
   

2. **Forensic Analysis**:
   - Examine all actions performed by Edward
   - Verify the integrity of the data he accessed
   

3. **Security Enhancement**:
   - Update IAM policies to limit access to sensitive resources
   - Enable MFA for all users
   - Configure more specific alerts in Security Hub

---
## Key Takeaways 🔑

This project that we did together has demonstrated the importance of:
- Actively monitoring user activities, even those with elevated privileges
- Setting up honeypots to detect malicious behavior
- Using AWS Security Hub as a centralized threat detection tool
- Applying the principle of least privilege to limit potential damage

---

*Congratulations! Thanks to this project, we successfully identified and contained an insider threat using AWS Security Hub. These skills are essential for any modern cybersecurity professional.*

**Secure. Detect. Respond.** 🛡️

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

