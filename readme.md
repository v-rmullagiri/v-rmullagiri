# Integrating AWS Network Firewall into Microsoft Sentinel
## Table of contents
- [Introduction](#intro)
- [Configuration process](#intro)
- [Prerequisites](#step2)
 
 
<a name="intro">
 
## Introduction
AWS Network Firewall connector ingests many AWS service logs into Azure Sentinel. Currently supported logs include: flow logs & alert logs.
 
This connector requires that each AWS service publish its logs to an S3 bucket in your account. In addition you must configure SQS notifications and permissions for the connector to retrieve the logs.
 
More information on the connector and configuration instructions can be found on the Azure Sentinel data connector page in the Azure portal.
 
## Configuration process
1. Create an AWS assumed role and grant access to the AWS Sentinel account.
2. Configure the AWS service ( Flow Logs& Event) to export logs to an S3 bucket.
3. Create a standard Simple Queue Service (SQS) in AWS.
4. Enable SQS notification.
5. Grant the Sentinel AWS account access to the S3 bucket & SQS.

<a name="step2">
  
## Prerequisites

The below mentioned resources are required to connect Aws Network Firewall with Sentinel.
- Aws account
- VPC, Subnets,Internet gatway
- Ec2 instance
- S3 bucket
- SQS
- AWS Network Firewall
- AWS cloud Watch

 
To generate the above resources, you must execute the following steps.
 
- Log Setup File
<a name="log">
 
## Deployment Steps
 
### **1 Create the Stacks**  
1. Navigate to **AWS Console → cloud formation**  
2. Click **create stack**
3. choose template & upload template file Click on next
4. fill the required fills ( Stack name,SentinelWorkspaceId) & Parameters can auto fill depends on template file, if we you want you to make any changes edit accordingly and click on next
5. in Configure stack options enable the check box & click on next
6. Set deployment options- in this no need to change any thing if you want to change anything go through that and click on next 
7. Review & submit it ( stack has created)
8. Stack
- This stack integrates Microsoft Sentinel by creating an IAM role with minimal permissions. This role allows Microsoft Sentinel to access your logs stored in a specified S3 bucket and SQS queue. The stack also creates an S3 bucket, an SQS Queue, and sets up S3 notifications. Additionally, it includes necessary IAM policies.
- 
**Configuring the existing firewall**
1. Navigate to the **Firewall → Logging**
2. click on edit - select log type(Alert,Flow)
3. select log destination  - S3 bucket
4. fill the S3 bucket name for Flow & Alert log destination columns    

- This will use full for the send the notifications to SQS bucket and send the logs to sentinal 
- SQS will show the Messages like send, rerceived,deleted & etc
  

 
<a name="auth">
