# 📌 AWS VPC Flow Logs: Setup, Monitoring, and Analysis project

Welcome back to this exciting project! 

## 🎯 The Mission :
This project demonstrates how to set up VPC Flow Logs to both S3 and CloudWatch, create alerts for failed SSH attempts, and perform advanced analysis of the collected data.


## 📋 Table of Contents
- 🛠️ [Part 1: Setting Up VPC Flow Logs](#part-1-setting-up-vpc-flow-logs)
- 🌐 [Part 2: Generating Network Traffic](#part-2-generating-network-traffic)
- 📊 [Part 3: Monitoring and Alerting](#part-3-monitoring-and-alerting)
- 📈 [Part 4: Data Analysis](#part-4-data-analysis)
- 🚀 [Conclusion](#conclusion)


---
# 🚀 Part 1: Setting Up VPC Flow Logs

We'll configure VPC Flow Logs to send data to both **S3** and **CloudWatch** for different analysis capabilities. 

## 📂 Creating S3 Flow Logs

1. **Create or identify an S3 bucket**
   - Navigate to **S3** and create a new bucket or use an existing one
   - Copy the bucket's **ARN** by selecting the bucket and clicking "Copy ARN"
   

2. **Set up VPC Flow Logs to S3**
   - Navigate to **VPC** in the AWS console
   - Select your VPC and go to the **Flow logs** tab
   - Click **Create flow log** & name it (e.g., `AeroSecure-kzax01`)
   - Configure the log settings:
   
     | Setting                | Value                        |
     |------------------------|------------------------------|
     | **Filter**            | All                          |
     | **Aggregation**       | 1 minute                     |
     | **Destination**       | Send to Amazon S3           |
     | **S3 bucket ARN**     | Paste copied ARN            |
     | **Log format**        | AWS default format          |
   - Click **Create flow log** ✅
   

3. **Verify the bucket policy**
   - Go to your **S3 bucket > Permissions** tab
   - AWS updates the policy to allow `s3:PutObject` for Flow Logs ✅

> 💡 **Note**: It may take **5-15 minutes** for logs to appear in S3.

## 📊 Creating CloudWatch Flow Logs

1. **Create a CloudWatch Log Group**
   - Navigate to **CloudWatch > Logs > Log groups**
   - Click **Create log group**, name it (e.g., `AeroSecureVPCLog`), and click **Create**
   

2. **Set up VPC Flow Logs to CloudWatch**
   - Go to your VPC **Flow logs** tab, click **Create flow log**
   - Configure the log settings:
   
     | Setting                | Value                        |
     |------------------------|------------------------------|
     | **Filter**            | All                          |
     | **Aggregation**       | 1 minute                     |
     | **Destination**       | Send to CloudWatch Logs     |
     | **Log Group**         | Select created log group    |
     | **IAM Role**          | Existing or new IAM role    |
   - Click **Create flow log** ✅

3. **Verify logs**
   - Go to **VPC > Flow logs** tab to see two **Active** logs (S3 & CloudWatch)
   - Navigate to **CloudWatch Log Group** to check for new logs

---

# 🎯 Part 2: Generating Network Traffic

We'll create network traffic to generate useful **ACCEPT** and **REJECT** logs, which we'll use for monitoring and analysis.

### 🌍 Connect to your EC2 instance (AeroSecure Server)

1. **Create & connect**
   ```bash
   ssh user@<public-ip-address>
   ```
   - Log out with `logout`
   

2. **Generate rejected traffic**
   - Remove **SSH access** security group & allow **only HTTP (port 80)**
   - Try reconnecting via SSH ❌ (should time out)
   

3. **Restore SSH access**
   - Re-add the **SSH security group**
   - Reconnect ✅
   

> 🔄 **This process generates ACCEPT & REJECT records in your Flow Logs.**

---

# 🔔 Part 3: Monitoring & Alerting

### 📈 Create CloudWatch Metric Filters

1. **Go to CloudWatch > Logs > Log groups**
2. **Select your VPC Flow Logs group**
3. **Create a Metric Filter for SSH rejections**
   ```
   [version, account, eni, source, destination, srcport, destport="22", protocol="6", packets, bytes, windowstart, windowend, action="REJECT", flowlogstatus]
   ```

4. **Set metric details:**
   - **Filter name:** `dest-port-22-reject`
   - **Metric namespace:** `AeroSecureVPCLog`
   - **Metric name:** `SSH Rejects`
   - **Metric value:** `1`
   - Click **Create metric filter** ✅
   

### 🚨 Create CloudWatch Alarms

1. **Create an alarm** from your new metric filter
2. **Set alarm conditions:**
   - **Threshold:** `Greater/Equal than 1`
   - **Period:** `1 minute`
3. **Set up SNS notifications** (email alerts)
4. **Test the alarm** by generating SSH rejection traffic 🚨


---

# 📊 Part 4: Data Analysis

### 🔍 Using CloudWatch Logs Insights

1. **Go to CloudWatch > Logs Insights**
2. **Run a query:**
   ```
   fields @timestamp, @message 
   | sort @timestamp desc 
   | limit 20
   ```
3. **View the top 20 rejected SSH attempts**

### 🛠️ Analyzing with Athena

1. **Go to S3 > Find your Flow Logs bucket**
2. **Copy the S3 URI** of your logs
3. **Use Athena** to query the logs directly

---

## 🎉 Congratulations! You've successfully set up & analyzed VPC Flow Logs 🔥

   

2. **Create the table structure**
   - Run the following SQL query, replacing the placeholders with your actual values:
     ```sql
     CREATE EXTERNAL TABLE IF NOT EXISTS default.vpc_flow_logs (
        version int,
     account string,
     interfaceid string,
     sourceaddress string,
     destinationaddress string,
     sourceport int,
     destinationport int,
     protocol int,
     numpackets int,
     numbytes bigint,
     starttime int,
     endtime int,
     action string,
     logstatus string
     ) PARTITIONED BY (
     dt string
     ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' LOCATION 's3://{your_log_bucket}/AWSLogs/{account_id}/vpcflowlogs/us-east-1/' TBLPROPERTIES ("skip.header.line.count"="1");
     ```
_You'll need to add your S3 bucket URL but this time; it should end by your region : mine is us-east-1_

➡ Then you do ctrl + enter, & it'll run your query automatically.


3. **Add a partition for your data**
   - Run the following SQL to add a partition for today's date:
     ```sql
     ALTER TABLE default.vpc_flow_logs
     ADD PARTITION (dt='YYYY-MM-DD')
     location 's3://{your-log-bucket}/AWSLogs/{account-id}/vpcflowlogs/us-east-1/YYYY/MM/DD/';
     ```
   - Replace YYYY-MM-DD with the actual date
   

4. **Query and analyze your data**
   - Run an analytical query on your flow logs:
     ```sql
     SELECT day_of_week(from_iso8601_timestamp(dt)) AS day,
       dt,
       interfaceid,
       sourceaddress,
       destinationport,
       action,
       protocol
     FROM vpc_flow_logs
     WHERE dt = '2025-03-17'  
     AND action = 'REJECT' 
     AND protocol = 6
     ORDER BY sourceaddress
     LIMIT 100;
     ```
   - This query shows rejected TCP connections (protocol 6), ordered by source IP address
   
➡️ Then you'll see Athena analysing all the queries related to "REJECT" action as requested earlier. 
Athena is a powerful tool for analysis. 

---

## Conclusion

You've successfully:
- ✔️  Set up VPC Flow Logs to capture network traffic data in both S3 and CloudWatch
- ✔️  Generated test traffic to populate your flow logs
- ✔️  Created a CloudWatch metric and alarm to detect and alert on suspicious activities
- ✔️  Used CloudWatch Logs Insights for real-time analysis
- ✔️  Set up Athena for SQL-based analysis of your historical flow logs data

This infrastructure gives you powerful capabilities for network monitoring, security analysis, and troubleshooting in your AWS environment.

AWS VPC Flow Logs capture information about IP traffic going to and from network interfaces in your VPC. This data is invaluable for:
- 🛡️ Security monitoring (detecting unauthorized access attempts)
- 🔎 Troubleshooting connectivity issues
- 🧪 Analyzing network traffic patterns
- 📊 Compliance verification

By combining them with CloudWatch metrics, alarms, and analysis tools like Athena, you've built a comprehensive solution for network visibility and security monitoring.

---

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
