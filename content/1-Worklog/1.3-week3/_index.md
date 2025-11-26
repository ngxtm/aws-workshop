---
title: "Week 3 Worklog"
date: 2025-09-22
weight: 3
chapter: false
pre: " <b> 1.3 </b> "
---

### Week 3 objectives:

- **Master Amazon S3**: Understand definitions, storage classes, security, performance, and static website hosting.
- **Advanced S3 Features**: Learn about Versioning, Replication, Object Lock, Access Points, and CloudFront integration.
- **Explore Data Transfer & Hybrid Storage**: AWS Snow Family and Storage Gateway.
- **Learn AWS RDS**: Understand Relational Database Service and practice Module 05.
- **VM Migration**: Practice importing/exporting VMs between local/on-prem environments and AWS EC2.

### Tasks to complete this week:

| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Start Date | Completion Date | References                                                                                                                                                                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | - Learn and practice Module 57:<br>&emsp; + What is S3 (definition, characteristics)<br>&emsp; + Distinguish between S3 Buckets and Objects<br>&emsp; + Existing S3 storage classes<br>&emsp; + S3 Security and Performance<br>&emsp; + Practice: create S3 and configure static web                                                                                                                                                                                                                                                                                                                                                                                                                      | 22/09/2025 | 22/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 3   | - Connect S3 with CloudFront to optimize transmission speed<br>- Learn about S3 Versioning (Definition, Benefits, How it works)<br>- Move Objects in S3<br>- Replicate objects multi-region<br>- Learn about Module 4:<br>&emsp; + Amazon Access Point<br>&emsp; + More details on S3<br>&emsp; + CORS support for S3<br>&emsp; + S3 Endpoint and S3 Versioning<br>&emsp; + Disaster Recovery <br>&emsp; + Snow family (Snowball, Snow Edge, Snowmobile)<br>&emsp; + Object Lock<br>&emsp; + Data retrieval options (Expedited, Standard, Bulk)<br>&emsp; + Main storage methods of Hybrid Storage Gateway (File Gateway, Volume Gateway, Tape Gateway)<br>&emsp; + Practice full small modules in Lab 14 | 23/09/2025 | 23/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 4   | - Learn about AWS RDS<br>- Learn and master practice steps in Module 05<br>- Memorize knowledge about AWS RDS                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 24/09/2025 | 24/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 5   | - Learn about VMWare export, how to import drives from local environment to EC2 and export back<br>- Import virtual machines into AWS<br>- Export virtual machines from on-premises<br>- Upload virtual machines to AWS<br>- Import virtual machines into AWS<br>- Deploy virtual machines from AMI                                                                                                                                                                                                                                                                                                                                                                                                       | 25/09/2025 | 25/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 6   | - How to use Storage Gateway<br>- How to create File Share<br>- How to mount File Share on local machine                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | 26/09/2025 | 26/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 7   | - Review learned theories                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 27/09/2025 | 27/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| CN  | - Review learned theories                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 28/09/2025 | 28/09/2025      | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |

### Week 3 Achievements:

#### **Amazon S3 (Simple Storage Service)**

- **Core Concepts**:
  - Understood S3 definitions, characteristics, and the difference between Buckets and Objects.
  - Explored various S3 Storage Classes and their use cases.
  - Learned about S3 Security and Performance optimization.
- **Advanced Features**:
  - **Static Web Hosting**: Successfully created an S3 bucket and configured it as a static website.
  - **CloudFront Integration**: Connected S3 with CloudFront to improve content delivery speed.
  - **Versioning**: Enabled S3 Versioning to manage object variants and protect against accidental deletion.
  - **Replication**: Configured Cross-Region Replication (CRR) for disaster recovery.
  - **Object Lock**: Implemented WORM (Write Once Read Many) models.
  - **Access Points & CORS**: Configured specific access points and Cross-Origin Resource Sharing settings.

#### **Storage & Data Transfer**

- **AWS Snow Family**:
  - Learned about Snowball, Snow Edge, and Snowmobile for large-scale data migration.
- **Storage Gateway**:
  - Understood Hybrid Storage Gateway types: File Gateway, Volume Gateway, and Tape Gateway.
  - Practiced creating File Shares and mounting them on local machines.
- **Data Retrieval**:
  - Explored retrieval options: Expedited, Standard, and Bulk.

#### **AWS RDS (Relational Database Service)**

- **Database Management**:
  - Gained knowledge on AWS RDS and its managed service capabilities.
  - Practiced deploying and managing RDS instances (Module 05).

#### **Migration & VM Management**

- **VM Import/Export**:
  - Learned how to export VMWare VMs and import drives from local environments to EC2.
  - Practiced exporting VMs from on-premises and uploading/importing them to AWS.
- **AMI Deployment**:
  - Successfully deployed virtual machines using Amazon Machine Images (AMI).

#### **Practical Skills Achieved**

- Configured S3 for static hosting and integrated with CloudFront.
- Managed S3 lifecycle, versioning, and security.
- Deployed and managed RDS instances.
- Performed VM migrations between local environments and AWS.
- Set up Hybrid Storage Gateway for file sharing.
