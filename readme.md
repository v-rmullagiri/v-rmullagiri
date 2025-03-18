# Integrating AWS Network Firewall into Microsoft Sentinel
## Table of contents
- [Introduction](#intro)
- [Configuration process](#intro)
- [Prerequisites](#step2)
 
 
<a name="intro">
 
## Introduction
AWS Network Firewall connector ingests  AWS service logs into Azure Sentinel. Currently supported logs include: flow logs,Alert logs& Tls logs.
 
This connector requires that each AWS service publish its logs to an S3 bucket in your account. In addition you must configure SQS notifications and permissions for the connector to retrieve the logs.
 
More information on the connector and configuration instructions can be found on the Azure Sentinel data connector page in the Azure portal.
 
## Configuration process
1. Create an AWS assumed role and grant access to the AWS Sentinel account.
2. Configure the AWS service ( Flow Logs, Alert Logs& Tls Logs) to export logs to an S3 bucket.
3. Create a standard Simple Queue Service (SQS) in AWS.
4. Enable SQS notification.
5. Grant the Sentinel AWS account access to the S3 bucket & SQS.

<a name="step2">
  
## Prerequisites

The below mentioned resources are required to connect Aws Network Firewall with Sentinel.
- Aws account(project Id,project name)
- VPC, Subnets,Internet gatway
- Ec2 instance
- S3 bucket
- SQS
- AWS Network Firewall
- AWS cloud Watch
- Stacks
- TLS

 
To generate the above resources, you must execute the following steps.
 
- Log Setup File
<a name="log">
 
## Deployment Steps
 
### **1 Create the Stacks**  
1. Navigate to **AWS Console → cloud formation**  
2. Click **create stack**
3. choose an exisiting template .
4. chose upload template upload the Aws resourse stacks which you download earlier click on next 
5. Provide the stack name
6. Under Parameters fill the required fills
   - **Aws role name:** In AWS CloudFormation stack refers to an IAM Role that is used to manage permissions for the stack's resources. It helps CloudFormation assume a 
      specific IAM role to deploy, update, or delete resources within the stack.
   - **Bucket Name** The bucket name will auto-populate by default. However, if you need to change it according to your organization's policies, you can modify it.
      * used for storing, managing, and retrieving data..
      * It helps in defining clear IAM policies based on the bucket name.
   - **Firewall Name**firewall is used to monitor, filter, and control network traffic to enhance security. AWS provides multiple firewall options to protect cloud 
       resources from unauthorized access, cyber threats, and malicious activities.
   - It Helps in identifying and managing firewalls across environments.& it will Links firewall rules and policies to a specific firewall.
   - **Sentinel workspace Id** It  is used to integrate AWS services with Microsoft Sentinel by specifying the Log Analytics Workspace ID where logs to be stored and 
    Monitor.
     To get the workspace id navigate to Azure portal choose your log analytical workspace  head to Overview you will find the workspace Id 
       
9. in Configure stack options enable the check box & click on next
10. Set deployment options- in this no need to change any thing if you want to change anything go through that and click on next 
11. Review & submit it ( stack has created)
12. Stack
- This stack integrates Microsoft Sentinel by creating an IAM role with minimal permissions. This role allows Microsoft Sentinel to access your logs stored in a specified S3 bucket and SQS queue. The stack also creates an S3 bucket, an SQS Queue, and sets up S3 notifications. Additionally, it includes necessary IAM policies.
**Configuring the existing firewall** 
1. Navigate to the **Firewall → Logging**
2. click on edit - select log type(Alert,Flow,TLS)
3. select log destination  - S3 bucket
4. fill the S3 bucket name for Flow ,TLS & Alert log destination columns    

- This will use full for the send the notifications to SQS bucket and send the logs to sentinal 
- SQS will show the Messages like send, rerceived,deleted & etc     
  

**TLS**
- Navigate to the TLS inspection configurations in AWs sonsole
- click on Create TLS inspection configurations
- Attach CA certificate for outbound SSL/TLS inspection - new (which is alerady created)
- click on next Describe TLS inspection configuration page will open
- in that fill the Name,Description and clock on next then Define scope  page will open fill the Source IP& source port range and Destination IP& destination port range, add scope configuration and click on next.
- Advanced settings  page will open in that nothing is required to fill and click on next  // enable the two check box like (Customer managed key,Certificate revocation status)
- no need to add any tags & click on next and Review and create TLS inspection configuration has done.
AWS Network Firewall provides deep packet inspection and stateful filtering capabilities, including TLS inspection for secure traffic monitoring and control. It allows organizations to enforce security policies for encrypted traffic without compromising confidentiality.
**Features of TLS in AWS Network Firewall**

TLS Decryption (TLS Inspection): Decrypts and inspects encrypted traffic for threat detection.

SSL/TLS Rules: Create rules to allow, block, or monitor encrypted sessions.

Certificate Management: Uses AWS Certificate Manager (ACM) for secure key management.

Integration with AWS Services: Works with VPC, AWS Security Hub, CloudWatch, and GuardDuty.

Compliance & Security: Helps meet PCI DSS, HIPAA, and GDPR requirements.

TLS in AWS Network Firewall →  Ensures inbound and outbound traffic is inspected securely.
- 

  

 
<a name="auth">
