---
title : "Week 2 Worklog"
date :  2025-09-15
lastmod : 2025-01-17
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---

### Week 2 objectives:
- **Practice Advanced Networking**: VPC Peering, Cross-Peer DNS, Transit Gateway
- **Learn Module 03 - Compute Services**: AMI, EC2, EBS, Instance Store, Auto Scaling
- **Explore Storage Services**: EFS, FSx, Lightsail, AWS Backup
- **Practice CloudFormation**: Security Groups, RDGW, Stack management
- **Attend Events**: Vietnam Cloud Day 2025, explore GenAI
- **Practice AWS Migration**: MGN (Application Migration Service)

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2 | - Go deeper with Auto Scaling Group + Application Load Balancer <br>&emsp; + Learn and create: target group, load balancer, auto scaling group, attach EC2 to ASG<br>- Go deeper into using EC2 + S3: <br>&emsp; + Apply knowledge to fetch data and upload to S3 without access keys<br>- Learn about Hybrid DNS and Route 53 Resolver<br>- Practice Module2-Lab10-01 => 02.2:<br>&emsp; + Learn about Key Pairs<br>&emsp; + Install CloudFormation | 15/09/2025 | 15/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 3 | - Practice Module2-Lab10-02.03 => Lab19-07:<br>&emsp; + Configure Security Group for CloudFormation <br>&emsp; + Connect to RDGW<br>&emsp; + What is a Stack in CloudFormation<br>&emsp; + Peering DNS<br>&emsp; + Cross-peer DNS<br>- Learn about Transit Gateway:<br>&emsp; + Prepare before creating TGW<br>&emsp; + Create TGW | 16/09/2025 | 16/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 4 | - Create TGW Attachment<br>- Create TGW Route Table<br>- Add TGW routes to VPC Route Tables<br>- Module 03:<br>&emsp; + Learn about Compute VMs on AWS<br>&emsp; + What is an AMI, what does it include, and AMI usage rights?<br>&emsp; + Where are EC2 instances backed up?<br>&emsp; + What is EBS, what does it include, and its characteristics?<br>&emsp; + What is the Nitro Hypervisor?<br>&emsp; + What is Instance Store and when to use it?<br>&emsp; + Pros and cons of Instance Store<br>&emsp; + What is EC2 User Data?<br>&emsp; + What is EC2 Metadata?<br>&emsp; + What is EC2 Auto Scaling?<br>&emsp; + What EC2 pricing options are available?<br>&emsp; + What is Amazon Lightsail? Characteristics, features, pricing, ...?<br>&emsp; + What is Amazon EFS? Characteristics, features, pricing, comparison with EBS, ...?<br>&emsp; + What is Amazon FSx? Definition, use cases, features, ...?<br>&emsp; + What is AWS Application Migration Service (MGN)? Purpose, ...? | 17/09/2025 | 17/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 5 | - Attend Vietnam Cloud Day 2025: Ho Chi Minh City Connect Edition for Builders<br>- Explore GenAI: Amazon Nova, Bedrock AgentCore, ADDL,... | 18/09/2025 | 18/09/2025 | [FCJ FB Group](https://www.facebook.com/pages/Th%C3%A0nh-ph%E1%BB%91-H%E1%BB%93-Ch%C3%AD-Minh/108458769184495) |
| 6 | - Learn about AWS Backup:<br>&emsp; + Practice creating CloudFormation, CloudWatch, S3 bucket, SNS, backup plan,...<br>&emsp; + Clean resources<br>&emsp; + Complete full Module3-lab13 | 19/09/2025 | 19/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 7 | - Practice Module 03 - Lab 24: Using AWS Storage Gateway | 20/09/2025 | 20/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| CN | - Review previous knowledge and take notes on important sections | 21/09/2025 | 21/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |

### Week 2 Achievements:

#### **Advanced Networking & Connectivity**
- **VPC Peering**: Practice creating and configuring VPC Peering connections
- **Cross-Peer DNS**: Configure DNS resolution between VPCs
- **Transit Gateway (TGW)**:
  - Prepare and create Transit Gateway
  - Create TGW Attachments
  - Create TGW Route Tables
  - Configure routes from TGW to VPC Route Tables

#### **Module 03 - Compute Services**
- **AMI (Amazon Machine Image)**:
  - Understand AMI can provision one or multiple EC2 Instances simultaneously
  - AMI includes: Root OS volume, EBS volume mapping, AMI usage rights
- **Keypair**: Encrypt login information for EC2 Instance
- **EC2 Backup & EBS**:
  - EC2 backup uses snapshots to S3 (first snapshot full, subsequent snapshots incremental)
  - **Amazon EBS (Elastic Block Store)**: Block storage attached directly to EC2
  - EBS operates independently from EC2, connects to EBS private network
  - 2 main disk groups: HDD & SSD, 99.9% availability through replication between 3 Storage Nodes in 1 AZ
  - EC2 always paired with EBS, without EBS EC2 cannot operate
  - EBS Multi attach: 1 EBS can attach to multiple EC2 Instances (with Nitro Hypervisor)
- **Instance Store**:
  - NVME disk area attached directly to hardware node
  - High speed but not backed up (not recommended for storage)
  - Stopping server loses all data, restart doesn't lose data
  - Used for: log, swap, paging, buffer/cache
- **EC2 Features**:
  - **EC2 User Data**: Commands run first time when initializing EC2 from AMI
  - **EC2 Metadata**: Information about EC2 Instance (Private/Public IP, Hostname, Security Group)
  - **EC2 Auto Scaling**: Increase/decrease instance count based on specific conditions
- **EC2 Pricing Options**:
  - **On-demand**: Hourly billing, most expensive, use under 6 hours
  - **Reserved Instance**: Over 6 hours, discount varies by instance type
  - **Saving Plans**: Not limited by instance type
  - **Spot Instance**: Utilize excess resources, cheap but can terminate in 2 minutes
  - **Best practice**: Combine multiple pricing options in EC2 Auto Scaling Group

#### **Storage Services**
- **Amazon Lightsail**:
  - Low-cost computing service
  - Each Lightsail Instance includes data transfer (cheaper than EC2)
  - Has backup capability
  - Runs in special VPC, connects to regular VPC = 1 click peering
- **Amazon EFS**:
  - Create NFSv4 Network volume attached to multiple EC2 Instances simultaneously
  - Scale to petabyte
  - EFS only supports Linux
  - Billed by usage capacity (EBS billed by allocated capacity)
  - Can mount to on-premise environment via DX or VPN
- **Amazon FSx**:
  - Create NTFS volume attached to multiple EC2s using SMB protocol
  - Billed by usage capacity
  - Supports deduplication feature
  - **FSx supports both Windows and Linux**

#### **Migration & Backup Services**
- **AWS Application Migration Service (MGN)**:
  - Migrate and replicate to build Disaster Recovery Site
  - Use staging machines smaller than main machines
  - Cut-over: MGN creates and runs on EC2 (complete migration to AWS)
- **AWS Backup**:
  - Practice creating CloudFormation, CloudWatch, S3 bucket, SNS
  - Create backup plan and clean resources
  - Complete Module3-lab13

#### **CloudFormation & Infrastructure as Code**
- **CloudFormation Stack Management**:
  - Configure Security Groups for CloudFormation
  - Connect to RDGW (Remote Desktop Gateway)
  - Understand what Stack is in CloudFormation
- **Module2-Lab10-01 to Lab19-07**: Complete practice labs

#### **Events & Research**
- **Vietnam Cloud Day 2025**: Attend Ho Chi Minh City Connect Edition for Builders
- **GenAI Research**: Explore Amazon Nova, Bedrock AgentCore, ADDL
- **AWS Storage Gateway**: Practice Module 03 - Lab 24

#### **Practical Skills Achieved**
- Configure VPC Peering and Cross-Peer DNS
- Create and manage Transit Gateway
- Deep understanding of EC2, EBS, Instance Store and pricing options
- Practice with EFS, FSx, Lightsail
- Use AWS Backup and Migration services
- Work with CloudFormation and Infrastructure as Code
- Attend tech events and research GenAI