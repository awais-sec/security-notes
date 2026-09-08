# AWS Fundamentals and Forensics

## 1. Introduction to Amazon Web Services

- AWS is a global CSP offering a suite of cloud-based products and services
- Services can be accessed via:
  - AWS Management Console
  - AWS Command Line Interface (CLI)
  - AWS Software Development Kits (SDKs)
  - Query APIs

**Top AWS cloud services:**

- Compute: EC2, Lambda, Elastic Beanstalk
- Database: RDS, DynamoDB, ElastiCache
- Storage: S3, Elastic Block Store, Elastic File System
- Networking: Virtual Private Cloud (VPC), CloudFront, Route 53
- Management & Governance: CloudWatch, CloudTrail, Config
- Security, Identity & Compliance: IAM, GuardDuty, Security Hub, Inspector

## 2. Shared Responsibility Model for AWS

Security and compliance is a shared responsibility between AWS and the customer, described as **security "of" the cloud** vs. **security "in" the cloud**.

**AWS responsibility (security OF the cloud):**
- Hardware / AWS global infrastructure
- Regions, availability zones, edge locations

**Customer responsibility (security IN the cloud):**
- Customer data
- Platform, applications, identity & access management (IAM)
- Operating system, network & firewall configuration
- Client-side data encryption & data integrity authentication
- Server-side encryption
- Networking traffic protection

## 3. Data Storage in AWS

- Customers can select AWS regions where their data will be stored
- Customers hold ownership of their data
- Customers can replicate data in more than one region for backup

**AWS infrastructure hierarchy:**
- **AWS Regions** — a physical location with clusters of data centers, isolated from other regions
- **Availability Zones** — one or more physically separate data centers within a region, connected via low-latency links
- **AWS Local Zones** — extend AWS regions to bring services closer to end users for lower latency

**AWS cloud storage services:** Amazon Elastic Block Store (EBS), Amazon Elastic File System (EFS), Amazon Simple Storage Service (S3), AWS Backup, AWS Storage Gateway, and the FSx family of specialized file services.

## 4. Logs in AWS

**A. AWS CloudWatch**
- Inspects, accesses, and stores log files from various AWS sources (e.g. CloudTrail, EC2, Route 53)
- Centralizes all log data for analysis with custom search queries
- Logs are viewed as log streams (a sequence of events from the same resource)
- Investigators can use it to examine system/application data from EC2 instances, review unusual API activity from CloudTrail, and monitor DNS queries from Route 53

**B. S3 Server Access Logs**
- Record detailed information about access requests to an S3 bucket (PUT, GET, DELETE)
- Entries contain fields like bucket owner, bucket name, requester IP, request time, Request-URI, and response status

**C. VPC Flow Logs**
- Record information about the inbound and outbound IP traffic on network interfaces within a VPC
- Entries contain source/destination IPs and ports, timestamp, and action

**D. Elastic Load Balancing (ELB) Access Logs**
- Record details about requests made to a load balancer
- Used for traffic pattern analysis during investigations

**E. AWS Database Logs**
- Investigators can view and analyze database log files via the AWS Management Console (e.g. for Amazon RDS)
- The tail log is refreshed by RDS every 5 seconds

## 5. Forensic Acquisition of an EC2 Instance: Methodology

If an EC2 instance is suspected to be compromised:

**Step 1: Isolate the compromised instance**
- Create a restricted security group that denies all outbound network traffic
- Configure ingress rules to only allow SSH (Linux) or RDP (Windows) traffic from a single, secure IP address used by investigators
- Attach this new security group to the compromised instance immediately

**Step 2: Take a snapshot of the EC2 instance**
- EC2 instances use EBS volumes as virtual hard drives — investigators must take an offline snapshot of the EBS volume for forensic evidence
- Process (AWS Management Console):
  1. Stop the affected EC2 instance
  2. In the EC2 console, select the instance and go to the Storage tab
  3. Locate the Volume ID and click on it (redirects to the Volumes page)
  4. With the volume selected, click **Actions → Create Snapshot**
  5. Add a description and click **Create Snapshot**
- Once the snapshot is created, the affected EC2 instance should be terminated

**Step 3: Provision and launch a forensic workstation**
- Use a separate AWS security account for forensic workstations
- Launch a new EC2 instance from a base AMI (Windows or Linux)
- Configure the security group's inbound rules to allow SSH/RDP only from the investigator's IP
- Harden the OS and install all necessary forensic software
- Stop the instance and create a new AMI from it — this becomes a golden template for future investigations, kept updated with the latest patches

**Step 4: Create an evidence volume from the snapshot**
- From the AWS Management Console, create a new EBS volume using the snapshot from Step 2 as the source
- Create it in the same availability zone as the forensic workstation

**Step 5: Attach the evidence volume to the forensic workstation**
- Ensure the forensic workstation instance is stopped
- In the Volumes section of the EC2 console, select the evidence volume
- Click **Actions → Attach Volume**, select the forensic workstation's instance ID
- Note the device name (e.g. `/dev/sdf` for Linux) and click **Attach volume**

**Step 6: Mount the evidence volume onto the forensic workstation**
- Start the forensic workstation
- For Linux workstations:
  - Run `lsblk` to verify the evidence volume is attached (e.g. appears as `xvdf1`)
  - Run `file -s /dev/xvdf1` to identify the filesystem (e.g. XFS, EXT4)
  - Create a mount directory: `sudo mkdir /mnt/evidence`
  - Mount the volume: `sudo mount /dev/xvdf1 /mnt/evidence`
- The data on the compromised instance's disk is now accessible for analysis at the mount point

## 6. Collecting Information Using the AWS CLI

Useful AWS CLI commands for investigators:

```bash
# View all AWS regions
aws ec2 describe-regions

# View EC2 instances with ID, type, and name
aws ec2 describe-instances | jq -r '.Reservations[].Instances[] | .InstanceId + " " + .InstanceType + " " + (.Tags[] | select(.Key=="Name").Value)'

# View security groups
aws ec2 describe-security-groups | jq -r '.SecurityGroups[] | .GroupId + " " + .GroupName'

# View CloudWatch alarms
aws cloudwatch describe-alarms | jq -r '.MetricAlarms[] | .AlarmName + " " + .Namespace'

# View subnets for a specific VPC
aws ec2 describe-subnets --filters Name=vpc-id,Values=<Your_VPC_ID> | jq -r '.Subnets[] | .SubnetId + " " + .CidrBlock + " " + (.Tags[] | select(.Key=="Name").Value)'
```

## 7. Investigating CloudWatch Logs

CloudWatch Logs provides a centralized platform to access, store, and monitor logs from various AWS sources.

**Key features for investigators:**
- Real-time monitoring of log data from multiple sources
- Log data querying using CloudWatch Logs Insights to find relevant data with custom queries
- Centralized view of logs from CloudTrail, EC2 instances, Route 53, etc.
- Customizable log retention periods

**Searching logs:**

Using the AWS CLI:
```bash
aws logs filter-log-events --log-group-name my-group [--log-stream-names STREAM1 STREAM2] [--filter-pattern "ERROR"]
# Can also use --start-time and --end-time (epoch milliseconds) to define a time window
```

Using the CloudWatch Console:
1. Open the CloudWatch console
2. Select **Log groups** from the navigation pane
3. Select the relevant log group, then the log stream
4. Use the filter syntax in the Log events section to search

## 8. Investigating S3 Server Access Logs

S3 Server Access Logs record detailed information for all requests made to a bucket (PUT, GET, DELETE) — crucial for investigating data breaches.

**Investigating with Amazon Athena** — run SQL queries directly on log files stored in S3.

Step-by-step investigation:

1. **Create a database:**
   ```sql
   CREATE DATABASE s3_access_logs_db
   ```
2. **Create a table schema** mapped to the S3 log location (a `CREATE EXTERNAL TABLE` statement defining the structure of the log data and its location in S3, e.g. `s3://DOC-EXAMPLE-BUCKET1-logs/prefix/`)
3. **Run investigative queries:**

   Top requesters:
   ```sql
   SELECT DISTINCT requester, COUNT(*) as requester_count
   FROM s3_access_logs_db.mybucket_logs
   WHERE requester != '-'
   GROUP BY requester
   ORDER BY COUNT(*) DESC
   ```

   Who deleted an object:
   ```sql
   SELECT requestdatetime, remoteip, requester, key
   FROM s3_access_logs_db.mybucket_logs
   WHERE key = 'images/picture.jpg' AND operation LIKE '%DELETE%'
   ```

   All actions by an IAM user:
   ```sql
   SELECT * FROM s3_access_logs_db.mybucket_logs
   WHERE requester = 'arn:aws:iam::123456789123:user/user_name'
   ```

   Actions on an object in a timeframe:
   ```sql
   SELECT * FROM s3_access_logs_db.mybucket_logs
   WHERE Key = 'prefix/images/picture.jpg'
   AND parse_datetime(requestdatetime,'dd/MMM/yyyy:HH:mm:ss Z')
       BETWEEN parse_datetime('2023-02-18:07:00:00','yyyy-MM-dd:HH:mm:ss')
       AND parse_datetime('2023-02-18:08:00:00','yyyy-MM-dd:HH:mm:ss')
   ```

   Data transferred to an IP:
   ```sql
   SELECT coalesce(SUM(bytessent), 0) AS bytessenttotal
   FROM s3_access_logs_db.mybucket_logs
   WHERE remoteip = '192.0.2.1'
   AND parse_datetime(requestdatetime,'dd/MMM/yyyy:HH:mm:ss Z')
       BETWEEN parse_datetime('2023-06-01','yyyy-MM-dd')
       AND parse_datetime('2023-07-01','yyyy-MM-dd')
   ```

   - Find PUT/GET requests in a period: modify the operation filter to `'REST.PUT.OBJECT'` or `'REST.GET.OBJECT'`
   - Find anonymous requests: `WHERE requester IS NULL`
   - Find requests requiring ACL: `WHERE aclrequired = 'Yes'`

## 9. Investigating AWS CloudTrail for IAM-based Incidents

CloudTrail logs API activity, making it essential for tracking attacker actions related to user and resource access.

**Using CloudWatch Logs Insights queries (AWS Console):**

```
# Access denied attempts
filter errorCode like /Unauthorized|Denied|Forbidden/
| fields awsRegion, userIdentity.arn, eventSource, eventName, sourceIPAddress, userAgent

# Actions by a specific access key
filter userIdentity.accessKeyId = "<Access Key ID>"
| fields awsRegion, eventSource, eventName, sourceIPAddress, userAgent

# Search by suspect IP
filter sourceIPAddress = "<IP Address>"
| fields awsRegion, userIdentity.arn, eventSource, eventName, sourceIPAddress, userAgent

# Creation of an IAM access key
filter responseElements.createDetails.accessKeyId = "<Access Key ID>"
| fields awsRegion, eventSource, eventName, sourceIPAddress, userAgent

# Creation of IAM users/roles
filter eventName="CreateUser" or eventName="CreateRole"
| fields requestParameters.userName, requestParameters.roleName, responseElements.user.arn, responseElements.role.arn, sourceIPAddress, eventTime, errorCode

# S3 bucket listing attempts
filter eventName="ListBuckets"
| fields awsRegion, eventSource, eventName, sourceIPAddress, userAgent
```

**Using the AWS CLI:**

```bash
# Access denied attempts
aws logs filter-log-events --region us-east-1 --log-group-name CloudTrail/DefaultLogGroup --filter-pattern "AccessDenied" --output json

# Search by access key ID
aws logs filter-log-events ... --filter-pattern "<Access Key ID>" ...

# Search by source IP
aws logs filter-log-events ... --filter-pattern "<Source IP address>" ...
```

## 10. Investigating Amazon VPC Flow Logs

VPC Flow Logs capture information about IP traffic going to and from network interfaces in a VPC.

**Sample queries (CloudWatch Logs Insights):**

```
# Rejected requests (unauthorized access)
filter action="REJECT" | stats count(*) as numRejections by srcAddr | sort numRejections desc

# Rejected requests from within the VPC
filter action="REJECT" and srcAddr like /^10\./ | stats count(*) as numRejections by srcAddr | sort numRejections desc

# All requests from a specific IP
filter srcAddr = "<Source IP Address>" | fields @timestamp, interfaceId, dstAddr, dstPort, action

# Connection counts from a private IP
filter srcAddr = "<Source IP Address>" | stats count(*) as numConnections by dstAddr | sort numConnections desc
```

## 11. Analyzing AWS Security Incidents Using GuardDuty

Amazon GuardDuty is a threat detection service that continuously monitors AWS environments for malicious activity by analyzing CloudTrail, VPC Flow Logs, DNS logs, and S3 data events.

**Steps for analysis:**
1. Open the GuardDuty console
2. Go to **Findings** to view the list of security findings
3. Select a finding to analyze its details

**Sample GuardDuty findings:**

- **Backdoor:EC2/C&CActivity.B** — an EC2 instance is querying an IP address associated with a known command-and-control (C&C) server; the instance is likely compromised and part of a botnet (`service.additionalInfo.threatListName = Amazon`, `service.additionalInfo.threatName = Log4j Related`)
- **CredentialAccess:IAMUser/AnomalousBehavior** — an API used to access credentials (e.g. `GetPasswordData`, `GetSecretValue`, `GenerateDbAuthToken`) was invoked in an anomalous way, suggesting credential theft
- **Backdoor:EC2/DenialOfService.Dns** — an EC2 instance is behaving in a way that indicates it's being used to perform a DoS attack via the DNS protocol (high severity)
- **Discovery:S3/AnomalousBehavior** — an API commonly used to discover S3 buckets (`ListBuckets`) was invoked in an unusual way, typical of the reconnaissance phase of an attack (low severity)

---
*Source: coursework notes (AI-compiled study notes), Cloud Forensics module. The source material split AWS content across two modules (fundamentals, then forensic acquisition/investigation); a later module explicitly referenced the earlier fundamentals rather than repeating them, so both are combined here into one AWS-focused file.*
