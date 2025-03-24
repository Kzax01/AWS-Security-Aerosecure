# 📌 Part 2 : Identifying and Remediating Threats with AWS Security Hub

Welcome back to this exciting project! 

## 🎯 The Mission :
As seen previously, the mission at AeroSecure is to protect StratoJet’s top-secret electric propulsion technology from cyber threats by simulating and analyzing a security breach using AWS Security Hub to track and neutralize the threat.

*Are you ready to accept this challenge?* Let's go! 

---

## 📋 Table of Contents


- 🛡️ [Phase 1: Enable Security Hub & Deploy Honeypots](#phase-1-enable-security-hub--deploy-honeypots)
- 👤 [Phase 2: Monitor Martina’s Activities](#phase-2-monitor-martinas-activities)
- 🕵️‍♂️ [Phase 3: Analyze George’s Activities](#phase-3-analyze-georges-activities)
- 🕵️‍♀️ [Phase 4: Investigate Edward’s Activities](#phase-4-investigate-edwards-activities)
- 📊 [Investigation Results](#investigation-results)
- 🔧 [Incident Response & Remediation](#incident-response--remediation)
- 🧠 [Key Takeaways](#key-takeaways)


---
# 🛡️ AeroSecure: The Hunt for the Intruder

## Phase 1: Enabling Security Hub and Deploying Honeypots

> *"In cybersecurity, the best trap is the one your adversary never sees coming."*
>
> Let's lay our bait and wait for the attacker to fall into our web.

### 🎯 Step 1: Configure AWS Config

AWS Config will be our ever-watchful eye, logging every move inside our environment.

1. Type `Config` in the AWS search bar.
2. Select **Config** from the dropdown menu.
3. Click **1-click setup** for rapid configuration.
4. On the review page, click **Confirm** at the bottom.
5. In the AWS Config Dashboard, navigate to **Settings** (left menu).

> 💡AWS config will automatically create a bucket s3 to store the records


### 🕵️ Step 2: Activate Security Hub

Our **command center** is Security Hub. Let’s turn it on.

1. Search for `Security Hub` in the AWS search bar.
2. Select **Security Hub** from the results.
3. Click **Go to Security Hub**.
4. On the activation page, click **Enable Security Hub** at the bottom.

### 🪤 Step 3: Deploy Your Honeypots

Now, we deploy our digital traps—honeypots designed to lure attackers into revealing themselves.

1. Open a new tab and search for `CloudFormation`.
3. Click **Create Stack > "Build from Infrastructure composer"**.
4. Then click on **"create in infrastructure composter"**
5. A new page will open, click **Create template**.
6. Copy all the **template code** from GitHub.
7. Return to the designer page, ensure the **Templates** tab is selected at the bottom.
8. Paste the template code, replacing the existing code.
9. **Validate** it & **create template** then confirm.
10. Name the stack as you want; for this project mine is `AeroSecureStack`.
11. Check the box under **Capabilities**, then click **Create stack**.

[🔗 CloudFormation Template Link](https://github.com/arcuricm/Hands-on-Security-Hub/blob/main/OriginalChallenge.yml)

> *"Patience, agent... Our trap deployment takes a few minutes. Meanwhile, let’s return to our command center."*

### 📌 What have we deployed in that Cloudformation template ?

| Resource              | Type                  | Purpose in Honeypot/Security Monitoring                                                                 |
|-----------------------|-----------------------|--------------------------------------------------------------------------------------------------------|
| **DataBucket**         | S3 Bucket             | A decoy bucket with strict access control. Alerts when unauthorized access is attempted, triggering a Security Hub finding. |
| **DataSecret**         | Secrets Manager Secret| A fake secret that triggers a Security Hub finding when decrypted or accessed without permission, detecting unauthorized credential access. |
| **DataTable**          | DynamoDB Table        | A decoy table, monitored for unauthorized reads or writes, generating Security Hub findings for suspicious activities. |
| **WriteDataFunction**  | Lambda Function       | A custom function that creates dummy data in honeypot resources to set up the decoy environment.         |
| **DataEventsBucket**   | S3 Bucket for CloudTrail | Logs API events related to honeypot resources for detailed forensic analysis.                             |
| **DataEventsTrail**    | CloudTrail            | Logs events specific to honeypot resources, reducing noise and focusing on security incidents.          |
| **DataFunction**       | Lambda Function       | Processes CloudTrail events and generates actionable Security Hub findings from raw data.               |
| **EventBridge Rules**  | AWS EventBridge Rules | Captures API calls to various resources (DynamoDB, S3, STS, KMS) and routes them to **DataFunction** for analysis and finding creation. |

### 🕵️‍♂️ The entire architecture is designed as a sophisticated honeypot system that:
1. Creates deliberately exposed but fake resources
2. Monitors all interactions with these resources
3. Generates high-fidelity security findings in AWS Security Hub
4. Provides detailed forensic information about potential security threats or unauthorized access attempts

> ➡️ It is perfect for our project.

### Go back to the Security Hub
After we deployed our services ( it can take some times), we can see that we start to have few findings by region.

![sechub1stfidings]

---

## Phase 2: Monitoring Suspect #1 - Martina's Activities

🚨 **Suspect Identified: Martina**  

> A seemingly innocent user... or is she? Let’s observe.

### 🔑 Logging in as Martina

1. Open a **new private browsing tab**.
2. Log into the AWS console with **Martina’s credentials**.
3. Ensure you are in **us-east-1 (N. Virginia)**.

### 🔍 Martina’s Suspicious Actions

> *Her first move: Creating new AWS resources.*

#### 🏗️ Creating a VPC

1. Search for `VPC` in AWS.
2. Click **Create VPC**.
3. Verify **VPC and more** is selected.
4. Click **Create VPC**.

#### 🚀 Launching an EC2 Instance

1. While the VPC is being created, search for `EC2`.
2. Create a **key pair** named `AeroSecureKP`.
3. Navigate to **EC2 Dashboard** > **Instances**.
4. Click **Launch instance**.
5. Name it `AerosecureInstance`, select **t3.micro**.
6. Choose **AeroSecureKP** as the key pair.
7. Enable **Auto-assign public IP**.
8. Click **Launch instance**.

#### 🔓 Managing IAM Access Keys

1. Search for `IAM`.
2. Navigate to **Users** > **Martina**.
3. Create a new **Access Key**.

#### 📂 Accessing S3 Buckets

1. Search for `S3`.
2. Click on the bucket with `cf-templates`.
3. Access an object containing `designer` in its name.

> ➡️ _Let's not forget the CF template we used on github deployed an S3 bucket. Hence why I didn't show any screenshots of me creating it._

#### ❌ Deactivating and Deleting Access Keys

1. Looks like she changed her mind.. so go return to **IAM**.
2. Select **Martina’s user profile**.
3. Under **Security Credentials**, deactivate and delete her access key.

> **Conclusion:** Martina’s actions seem... odd. But is she the real culprit?

---

## 🕵️‍♂️ Phase 3: Tracking Suspect #2 - George

🚨 **New Suspect Identified: George, IAM & Compliance Specialist**

> *George has elevated IAM permissions. Let’s see how he uses them.*

### 🔑 Logging in as George

1. Open a **private browsing tab**.
2. Log into AWS with **George’s credentials**.

### 🔍 George’s Actions

#### 🔑 Creating IAM Access Keys

1. Navigate to **IAM > Users > George**.
2. Create a **new access key**.

#### 🗄️ Accessing S3 and DynamoDB Resources

1. Navigate to **S3** and download an object from a **cf-templates bucket**.
2. Search for **DynamoDB**.
3. Copy the **table name** (e.g., AeroSecureStack-DataTable-xxxx).

> ➡️ _Remember, the S3 + DynamoDB were created thanks to the CF template we used earlier._

#### 💻 Connecting to an EC2 Instance

1. Search for `EC2`.
2. Select **AdminServer**.
3. Click **Connect** and access the instance.

#### 🛠️ Running Commands on the Instance

Run `ls`, `pwd`, and `sudo yum update`.

> **George is interacting with sensitive data. Is he our intruder?**

---

## 🚨 Phase 4: Confronting Suspect #3 - Edward

🚨 **Final Suspect: Edward, Security Operations Lead**

> *If anyone knows how to cover tracks, it’s Edward. Let’s catch him in the act.*

### 🔑 Logging in as Edward

1. Open a **private browsing tab**.
2. Log into AWS with **Edward’s credentials**.

### 🔍 Edward’s Moves

#### 🔑 Creating IAM Access Keys

1. Navigate to **IAM > Users > Edward**.
2. Create a new **Access Key**.

#### 💻 Connecting to EC2 & Configuring AWS CLI

1. Access **EC2 > AdminServer**.
2. In the terminal, run:
   ```sh
   aws configure
   ```
3. Enter Edward’s **Access Key & Secret**.
4. Set the region as `us-east-1`.

#### ⚠️ Unauthorized Access to S3 and DynamoDB

1. Copy the **S3 bucket name** (e.g., `aerosecurestack-databucket-xxxxx`).
2. In **EC2 Instance Connect**, attempt an unauthorized S3 download:
   ```sh
   aws s3 cp s3://aerosecurestack-databucket-xxxxx/secret-data.txt .
   ```

> **🚨 Caught red-handed! Edward is our intruder.**


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

## 🎯 Incident Response and Remediation Plan

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

> *"In cybersecurity, the hunt never ends. Stay vigilant."* 🛡️

---
## 🔑 Key Takeaways 

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

