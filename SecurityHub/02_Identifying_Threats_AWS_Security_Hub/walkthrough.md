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

![awsconfig](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/aws%20config%20main%20page.png)


1. Type `Config` in the AWS search bar.
2. Select **Config** from the dropdown menu.
3. Click **1-click setup** for rapid configuration.
4. On the review page, click **Confirm** at the bottom.
5. In the AWS Config Dashboard, navigate to **Settings** (left menu).

![config](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/config%202.png)

> 💡AWS config will automatically create a bucket s3 to store the records

![s3created](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/config%203%20s3%20bucket%20created.png)


### 🕵️ Step 2: Activate Security Hub

![sechub](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/sec%20hub%20main%20page.png)


Our **command center** is Security Hub. Let’s turn it on.

1. Search for `Security Hub` in the AWS search bar.
2. Select **Security Hub** from the results.
3. Click **Go to Security Hub**.
4. On the activation page, click **Enable Security Hub** at the bottom.

### 🪤 Step 3: Deploy Your Honeypots

![CF](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/cloud%20formation%20main%20page.png)

Now, we deploy our digital **traps—honeypots** designed to lure attackers into revealing themselves.

![cf1](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/CF-1%20creating%20stack.png)

1. Open a new tab and search for `CloudFormation`.
3. Click **Create Stack > "Build from Infrastructure composer"**.
4. Then click on **"create in infrastructure composter"**

![cg2](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/CF-2%20deploying%20template.png)

5. A new page will open, click **Create template**.
6. Copy all the **template code** from GitHub.
7. Return to the designer page, ensure the **Templates** tab is selected at the bottom.
8. Paste the template code, replacing the existing code.
9. **Validate** it & **create template** then confirm.
10. Name the stack as you want; for this project mine is `AeroSecureStack`.
11. Check the box under **Capabilities**, then click **Create stack**.

![cfdone](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/CF-3%20deployed%20done.png)


[🔗 CloudFormation Template Link](https://github.com/arcuricm/Hands-on-Security-Hub/blob/main/OriginalChallenge.yml)

> *"Patience, agent... Our trap deployment takes a few minutes. Meanwhile, let’s return to our command center."*

### 📌 What have we deployed in that Cloudformation template ?

![service](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/cf5-what%20have%20we%20deployed%202.1.png)


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

![services2](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/CF-5%20what%20have%20we%20deployed.png)


### Go back to the Security Hub
After we deployed our services ( it can take some times), we can see that we start to have few findings by region.

![sechub1stfidings](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/sec%20hub%201st%20findings.png)


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

![vpc](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/martinavpc1.png)

1. Search for `VPC` in AWS.
2. Click **Create VPC**.
3. Verify **VPC and more** is selected.
4. Click **Create VPC**.

#### 🚀 Launching an EC2 Instance

1. While the VPC is being created, search for `EC2`.

![kp](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/martina%202%20KP.png)
2. Create a **key pair** Mine was named `AeroSecureKP` for this project.
3. Navigate to **EC2 Dashboard** > **Instances**.


4. Click **Launch instance**.
5. Name it `AerosecureInstance`, select **t3.micro**.
6. Choose **AeroSecureKP** as the key pair.
7. Enable **Auto-assign public IP**.

![enable autoip](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/martina%20enable%20auto%20ip.png)

8. Click **Launch instance**.

![launchec2](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/martina%20ec2%20creating.png)


#### 🔓 Managing IAM Access Keys

1. Search for `IAM`.
2. Navigate to **Users** > **Martina**.
3. Create a new **Access Key**.

![Access keys ](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/martina%20AK.png)


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

![db](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/george%20dynamodb.png)


> ➡️ _Remember, the S3 + DynamoDB were created thanks to the CF template we used earlier._

#### 💻 Connecting to an EC2 Instance

1. Search for `EC2`.
2. Select **AdminServer**.
3. Click **Connect** and access the instance.

> 💡 _George & Edward are using the EC2 that Martina created earlier_

#### 🛠️ Running Commands on the Instance

Run `ls`, `pwd`, and `sudo yum update`.

| **Command**              | **Explanation**                                                                                 | **Importance in the Project**                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| `ls`                     | Lists the files and directories in the current directory. Useful for checking the directory contents. | Ensures that George is in the correct directory on the EC2 instance, allowing him to verify the files needed for the project. |
| `pwd`                    | Prints the current working directory. Helps identify the location you're operating in within the system. | Verifies that George is in the correct location for executing further commands related to the project.             |
| `sudo yum update`        | Updates the system's packages using `yum` (a package manager for Red Hat-based Linux distributions). This ensures that the system has the latest security patches and updates. | Ensures the EC2 instance is secure and up to date, preventing vulnerabilities that could interfere with project tasks, like security emulation. |


![ec2](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/george%20ec2.png)


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

1. Access **EC2 > The server**.

![ec2 logged in](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/edward%20ec2%20log%20in.png)

2. In the terminal, run:
   ```
   aws configure
   ```
3. Enter Edward’s **Access Key & Secret**.
4. Set the region as `us-east-1`.

#### ⚠️ Unauthorized Access to S3 and DynamoDB

1. Copy the **S3 bucket name** (e.g., `aerosecurestack-databucket-xxxxx`).
2. In **EC2 Instance Connect**, attempt an unauthorized S3 download:
   ```
   aws s3 cp s3://<name of your s3 bucket>/<name of the document in s3 that you want to download>
   ```
   ```
   aws dynamodb scan --table-name <enter your DB name>
   ```
![scan](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/edward%20scan%20db.png)

### ➡️ Why is it an issue that Edward scanned these resources ?

| **Issue**                      | **Explanation**                                                                 |
|---------------------------------|---------------------------------------------------------------------------------|
| **Lack of Granular Access**    | Over-permissioned access, violating least privilege principle.                  |
| **No Logging/Monitoring**      | CLI actions often bypass centralized logging, making auditing difficult.        |
| **Exposing Secrets**           | CLI might expose AWS credentials if not handled securely.                       |
| **Risk of Overexposure**       | Scanning entire S3 & DB could lead to exposing unnecessary or sensitive data.  |
| **Accidental Modifications**   | Direct CLI access increases risk of human error (deletion, modification).       |

**Better Alternatives:**
- Use IAM roles with least privilege
- Leverage AWS Security Hub/CloudTrail for monitoring
- Use scripts with logging and encryption for scanning

---

## 🔎 Phase 5: Security Analysis  

After simulating user activities, we analyzed the findings in the Security Hub console.

### 🔴 Critical Security Findings

![sechub](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/phase%205%20findings%20after%20edwards.png)

- High-severity findings were detected, including:
  - Unauthorized access to honeypot resources.
  - Suspicious DynamoDB scan operations.
  - Potential data exfiltration identified through S3 copy operations.

### ⏱️ Timeline Correlation

![timeline](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/phase%205%20%20timestamp%20match.png)

- Suspicious activity timestamps perfectly aligned with Edward's session.
- Security Hub pinpointed the user involved in the malicious actions.
- The behavioral pattern (mass data access and copying) indicated possible data theft.

### ✅ Finding Confirmation
- CloudTrail logs confirmed Security Hub findings.
- API calls matched those made during Edward's session.
- The targets of these calls were the honeypot resources we deployed.

➡️ **Verdict:** **Edward is the compromised account! 🏴‍☠️**  
> **🚨 Caught red-handed! Edward is the intruder! 🚨**

---

## 🎯 Incident Response and Remediation Plan

Now that we've identified our hacker, here are the recommended remediation steps:

1. **Immediate Access Revocation**:

![kp removed](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/revoke%20all%20%20AK.png)

   - Disable all of Edward's access keys, as well as the other users.
   - Revoke his IAM permissions or take drastic actions : **Delete his compromised account.**

![delete edward](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/edward%20deleted.png)


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

![kp best practice](https://github.com/Kzax01/AWS-Security-Aerosecure/blob/main/SecurityHub/02_Identifying_Threats_AWS_Security_Hub/screenshots/Access%20key%20best%20practices.png)


> ➡️ [**You can find here how to implement the least privilege + with Permissions Boundaries in AWS IAM.**](https://github.com/Kzax01/AWS-Security-Aerosecure/tree/main/IAM/01_Limiting_Privileged_Access_IAM)

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

