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
## Part 1: Setting Up VPC Flow Logs

We'll configure VPC Flow Logs to send data to both S3 and CloudWatch for different analysis capabilities.

### Creating S3 Flow Logs

1. **Create or identify an S3 bucket**
   - Navigate to S3 and create a new bucket or use an existing one
   - Copy the bucket's ARN by selecting the bucket and clicking "Copy ARN"
   

2. **Set up VPC Flow Logs to S3**
   - Navigate to VPC in the AWS console
   - Select your VPC and click on the "Flow logs" tab
   - Click "Create flow log" & name it. Mine was : AeroSecure kzax01
   - Configure the flow log with these settings:
     - Filter: **All**
     - Maximum aggregation interval: **1 minute** (for faster results during testing)
     - Destination: **Send to an Amazon S3 bucket** (I named it S3-logs)
     - S3 bucket ARN: Paste your copied ARN
     - Log record format: **AWS default format**
   - Click "Create flow log"
   

3. **Verify the bucket policy**
   - Return to your S3 bucket and select the "Permissions" tab
   - Notice that AWS automatically updated the bucket policy to allow the VPC Flow Logs service to write to your bucket
   - The policy will include the `s3:PutObject` permission for the Flow Logs service
   

> 💡 **Note**: It may take 5-15 minutes for flow logs to start appearing in your S3 bucket.

### Creating CloudWatch Flow Logs

1. **Create a CloudWatch Log Group**
   - Navigate to CloudWatch in the AWS console
   - Go to "Logs" > "Log groups" in the sidebar
   - Click "Create log group"
   - Name your log group (mine is AeroSecureVPCLog)
   - Click "Create"
   
   

2. **Set up VPC Flow Logs to CloudWatch**
   - Return to your VPC and the "Flow logs" tab
   - Click "Create flow log" & name it (Mine was CW-logs)
   - Configure the flow log with these settings:
     - Filter: **All**
     - Maximum aggregation interval: **1 minute**
     - Destination: **Send to CloudWatch Logs**
     - Destination log group: Select your created log group
     - IAM role: Select an existing role with appropriate permissions or create a new one
     - Log record format: **AWS default format**
   - Click "Create flow log"
   

3. **Verify your flow logs**
   - On the VPC "Flow logs" tab, you should now see two Active flow logs—one sending to S3 and another to CloudWatch
   - Navigate to your CloudWatch Log Group to see if log streams are being created (remember, this may take a few minutes)
   

---

## Part 2: Generating Network Traffic

To generate meaningful flow logs, we need to create some network traffic, including both successful and rejected connections.

1. **Connect to your EC2 instance** - AeroSecure Server 
   - Navigate to EC2 in the AWS console
   - Create your instance and copy its Public IPv4 address ( Be sure to enable the auto-assign Ipv4)
   - Open a terminal and connect via SSH:
     ```
     ssh user@<public-ip-address>
     ```
   - Once connected, log out using the `logout` command
   

2. **Generate rejected traffic**
   - In the EC2 console, select your instance (AeroSecure Server)
   - Go to Actions > Security > Change security groups
   - Remove the security group that allows SSH access (typically port 22)
   - Add a security group that only allows HTTP (port 80)
   - Try to connect to your instance again via SSH—this connection will time out and generate a REJECT record in your flow logs
   - Cancel the SSH attempt with Ctrl+C
   

3. **Restore SSH access**
   - Go back to your instance's security groups
   - Revert to the original security group that allows SSH
   - Connect to your instance again via SSH to verify access is restored
   

> 🔄 This process of allowing and blocking SSH access generates both ACCEPT and REJECT records in your VPC Flow Logs, which we'll use for monitoring and analysis.

---

## Part 3: Monitoring and Alerting

Now that we have flow logs data and have generated some traffic, let's create monitoring and alerting systems.

### Creating CloudWatch Metric Filters

1. **Access your CloudWatch Log Group**
   - Navigate to CloudWatch > Logs > Log groups
   - Select your VPC Flow Logs log group
   - Verify that log streams are now visible
   

2. **Create a Metric Filter for SSH rejections**
   - Select the "Metric filters" tab and click "Create metric filter"
   - In the "Filter pattern" field, enter:
     ```
     [version, account, eni, source, destination, srcport, destport="22", protocol="6", packets, bytes, windowstart, windowend, action="REJECT", flowlogstatus]
     ```
   - This pattern will detect rejected SSH connection attempts (port 22, TCP protocol)
   - Click "Next" after testing the pattern
   

3. **Configure the metric details**
   - Filter name: **dest-port-22-reject**
   - Metric namespace: **AeroSecureVPCLog** (or another name of your choice)
   - Metric name: **SSH Rejects**
   - Metric value: **1** (this counts each matching event as 1)
   - Click "Next" and then "Create metric filter"
   

### Setting Up CloudWatch Alarms

1. **Create an alarm based on your metric filter**
   - From the "Metric filters" tab, select your newly created filter
   - Click "Create alarm" & name it. Mine was Rejects-alert
   - Configure the alarm conditions:
     - Period: **1 minute**
     - Threshold type: **Static**
     - Condition: **Greater/Equal than 1**
     - This will trigger when there's at least one SSH rejection per minute
     

2. **Configure alert notifications**
   - Select "Create a new topic" for SNS notifications
   - Enter your email address for notifications
   - Confirm the subscription via the email you receive
   - Name your alarm (e.g., "SSH rejects")
   - Review and create the alarm
   

3. **Test your alarm**
   - Generate more rejected SSH traffic by changing the security group again
   - Wait a few minutes for the data to propagate
   - Check your alarm status—it should change to "In alarm"
   - You should receive an email notification about the alarm
   
---

## Part 4: Data Analysis

Now let's explore how to analyze the VPC Flow Logs data using different AWS services.

### Using CloudWatch Logs Insights

1. **Run a query with Logs Insights**
   - Navigate to CloudWatch > Logs > Logs Insights
   - Select your VPC Flow Logs log group
   - Use the sample queries for VPC Flow Logs or create your own
   - Try the "Top 20 source IP addresses with highest number of rejected requests" query
   - Click "Run query" and analyze the results

### Analyzing Data with Athena

1. **Create an Athena table for your flow logs**
   - Navigate to your S3 bucket with flow logs
   - Navigate through the folder structure (AWSLogs/account-id/AeroSecureVPCLog/region/year/month/day)
   - Copy the S3 URI for this path
   - Go to Athena and launch the query editor
   - Configure Athena settings to store query results in your S3 bucket
   

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
     WHERE dt = '2025-03-17'  -- Filtre sur la partition correcte
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
- 📝 Set up VPC Flow Logs to capture network traffic data in both S3 and CloudWatch
- 🚦 Generated test traffic to populate your flow logs
- 🔔 Created a CloudWatch metric and alarm to detect and alert on suspicious activities
- 🔍 Used CloudWatch Logs Insights for real-time analysis
- 📊 Set up Athena for SQL-based analysis of your historical flow logs data

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
