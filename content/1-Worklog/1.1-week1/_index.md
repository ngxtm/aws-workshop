---
title : "Week 1 Worklog"
date :  2025-09-10 
weight : 1 
chapter : false
pre : " <b> 1.1 </b> "
---
### Week 1 objectives:
- **Create AWS account** and familiarize with basic services
- **Complete Module 01-02**: Leadership Principles, Global Infrastructure, Management Tools
- **AWS Cost Management**: AWS Budgets, Cost Optimization, Support Plans
- **Learn VPC & Networking**: VPC, Subnet, Security Groups, Load Balancers
- **Hands-on practice**: Create VPC, EC2, NAT Gateway, VPC Peering
- **Additional research**: Well Architecture Framework, Advanced Networking

### Tasks to complete this week

| Day | Task | Start date | Completion date | References |
|---|---|---|---|---|
| 2 | - Get acquainted with FCJ members<br>- Read and take notes of the internship unit's rules and regulations | 08/09/2025 | 08/09/2025 |  |
| 3 | - Create an AWS account<br>- Learn about AWS services:<br>&emsp; + What is MFA, IAM user groups, IAM users<br>&emsp; + How to view and update account information<br>&emsp; + Create an account alias for IAM users<br>- Learn to draw architectures on [draw.io](https://draw.io)<br>- What is cloud computing (definition, benefits)<br>- What is an Access Key<br>- Complete modules 01-02<br>&emsp; + Leadership Principles<br>&emsp; + AZ - What is an Availability Zone<br>&emsp; + What is a Region<br>&emsp; + What is an Edge location<br>&emsp; &emsp; * CloudFront (CDN)<br>&emsp; &emsp; * Web Application Firewall (WAF)<br>&emsp; &emsp; * Route 53 (DNS Service)<br>&emsp; + AWS Management Console (Service Search, Support Center, CLI)<br>&emsp; + Cost management and optimization in AWS<br>&emsp; + How to use AWS Support<br>- Complete all labs in module 01<br>- Additional research on the Well-Architected Framework | 09/09/2025 | 09/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 4 | - Manage AWS usage costs with AWS Budgets<br>&emsp; + Learn to set recurring budgets<br>&emsp; + Learn to configure multiple cost alert thresholds<br>&emsp; + Create a Cost Budget<br>&emsp; + Define budget types (Cost budget, Usage budget, Savings plans budget, RI Budget)<br>- Learn and get more familiar with AWS Support:<br>&emsp; + Current AWS Support packages (Definition, pricing, features)<br>&emsp; + Types of support requests (Account and Billing Support, Service Limit Increase, Technical Support)<br>&emsp; + Details on how to change support packages<br>- AWS networking services<br>&emsp; + Amazon Virtual Private Cloud (VPC)<br>&emsp; &emsp; * What are Subnet, Public Subnet, Private Subnet<br>&emsp; &emsp; * What are Route table, custom and default route table<br>&emsp; &emsp; * Understanding ENI, EIP<br>&emsp; &emsp; * VPC Endpoint<br>&emsp; &emsp; * Internet Gateway<br>&emsp; + VPC Peering & Transit Gateway<br>&emsp; + VPN & Direct Connect<br>&emsp; + Elastic Load Balancing | 10/09/2025 | 10/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 5 | - VPC:<br>&emsp; + NAT Gateway and its architecture<br>&emsp; + Security Group<br>&emsp; + NACL<br>&emsp; + VPC Flow Logs<br>- VPC Peering & Transit Gateway:<br>&emsp; + VPC Peering (Definition, Configuration, Examples, Use cases, Architecture)<br>&emsp; + What is Transit Gateway, Transit Gateway Attachment<br>- VPN & Direct Connect:<br>&emsp; + What is a hybrid environment<br>&emsp; + VPN Site to Site & VPN Client to Site<br>&emsp; + AWS Direct Connect<br>&emsp; + Elastic Load Balancing (Protocols, Functions, Health check)<br>&emsp; &emsp; * Application, Network, Classic, Gateway Load Balancer (Features, Properties)<br>&emsp; &emsp; * Sticky Session, ALB (Definition, Where it operates)<br>&emsp; &emsp; * Path-based routing feature in ALB<br>- Deeper practice with IAM<br>&emsp; + Learn more about IAM (Shared Account Access, Granular Access Control, Secure Application Access)<br>&emsp; + IAM Policy, IAM Role | 11/09/2025 | 11/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 6 | - Details about firewalls in VPC<br>&emsp; + Security Group<br>&emsp; + Network ACLs<br>&emsp; + VPC Resource Map<br>- Practice:<br>&emsp; + Create VPC<br>&emsp; + Create subnet<br>&emsp; + Create Internet Gateway<br>&emsp; + Create Route Table<br>&emsp; + Create Security Group<br>&emsp; + Enable VPC Flow Logs<br>&emsp; + Create EC2 Server<br>&emsp; + Create NAT Gateway<br>&emsp; + Use Reachability Analyzer | 12/09/2025 | 12/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| 7 | - Install CloudWatch Monitoring and Alerting<br>- Learn and install Site-to-Site VPN | 13/09/2025 | 13/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |
| CN | - Review and redo EC2 setup steps,..<br>- Purpose, usage, practice with personal project:<br> + Auto scaling group + Load Balancer<br> + EC2 + S3 | 13/09/2025 | 13/09/2025 | [FCJ Study Web](https://cloudjourney.awsstudygroup.com)<br>[Youtube First Cloud Journey](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |

<!-- End of table -->

### Week 1 Achievements:

#### **AWS Fundamentals & Global Infrastructure**
- **AWS Leadership Principles**: Understanding of 16 AWS leadership principles, especially "Customer Obsession" and "The top for 13 consecutive years"
- **AWS Global Infrastructure**:
  - **Availability Zone (AZ)**: Contains one or multiple data centers, provides fault isolation, should deploy apps on 2 AZs
  - **Region**: Minimum 3 AZs per region
  - **Edge Location**: 
    - CloudFront (CDN)
    - Web Application Firewall (WAF) 
    - Route 53 (DNS Service)
- **AWS Management Tools**:
  - AWS Management Console (Service Search, Support Center, CLI)
  - AWS SDK (Credential management, retry, data marshalling, serialization, deserialization)
- **Additional Research**: Well Architecture Framework

#### **Cost Management & Support**
- **Cost Optimization Strategies**:
  - Choose appropriate configurations, use saving plans, reserved instances, spot instances
  - Design optimized architecture, setup AWS Budget
  - Departmental cost management with cost allocation tags
- **AWS Budgets**:
  - Create recurring budgets for continuous monitoring
  - Configure multiple alert thresholds (50%, 80%, 90%, 100%)
  - Use actual and forecasted alerts
  - Filtering by tags to track costs by project/department
  - Create Cost Budget and Usage Budget (account must be active > 1 month)
- **Saving Plans**:
  - Budget only tracks total saving plan, not per plan
  - Best practice: Cost Explorer → Reports → SP Utilization/Coverage → filter
- **AWS Support Plans**:
  - **Basic**: 24/7 access, best practices, building block architecture support
  - **Business**: Use-case specific guidance, AWS Trusted Advisor, third-party software support
  - **Enterprise**: Software architecture guide, Infrastructure Event Management, TAM, prioritized support
- **Support Request Types**: Account/Billing, Service Limit Increase, Technical Support

#### **VPC & Networking Fundamentals**
- **VPC (Virtual Private Cloud)**:
  - Located in 1 region, requires CIDR IPv4 declaration (mandatory)
  - Limit: 5 VPCs/region/account
  - Purpose: Environment separation (Production/Dev/Test/Staging)
  - Each subnet reserves 5 IPs: Network, broadcast, router, DNS, future features
  - Each subnet only exists in 1 AZ
  - **Subnet Types**: Public (external data transmission), Private (internal data transmission)
- **Network Components**:
  - **ENI Components**: EIP, MAC, Private IP Address
  - **VPC Endpoints**: Interface endpoint, Gateway endpoint
  - **Internet Connectivity**: Internet Gateway + Public IP + Route Table mapping
  - **NAT Gateway**: Allows private subnet outbound communication only, no direct inbound access
- **Security & Monitoring**:
  - **Security Groups**: Stateful, allow rules only, applied to ENI, default blocks inbound, allows outbound
  - **NACL (Network Access Control List)**: Stateless, applied to subnets, rules read from top to bottom
  - **VPC Flow Logs**: Monitor IP traffic, stored on S3 or CloudWatch Logs
- **Load Balancers**:
  - **ALB**: Layer 7, path-based routing, HTTP/HTTPS, routing outside VPC
  - **NLB**: Layer 4, auto-scale, static IP, highest performance (millions of requests/second)
  - **Classic LB**: Layer 4&7, higher cost, rarely used
  - **Gateway LB**: Layer 3, forward to virtual appliances

#### **Advanced Networking & Connectivity**
- **VPC Peering & Transit Gateway**:
  - **VPC Peering**: 1:1 connection, no transitive routing support, requires route table configuration
  - **Transit Gateway**: Central hub connecting multiple VPCs, create TGW, attachments, route tables
- **VPN & Direct Connect**:
  - **VPN Site-to-Site, VPN Client-to-Site**: Recommended to use third-party solutions from AWS Marketplace
  - **AWS Direct Connect**: Stable connection, Hosted Connection allows customizable bandwidth configuration
- **Hybrid DNS**: Outbound/inbound endpoints, S3 resolver rules

#### **Hands-on Labs & Practice**
- **VPC Setup & Configuration**:
  - Create VPC, Subnet, Internet Gateway, Route Table
  - Configure Security Groups, NACL
  - Enable VPC Flow Logs
  - Create EC2 Server, NAT Gateway
  - Use Reachability Analyzer
- **Session Manager**: Create EC2 connections, manage session logs, Port Forwarding
- **Auto Scaling Group + ALB**: Detailed practice
- **Key-pair Management**: For getting administrator password and SSH into EC2

#### **Additional Research**
- **Advanced Networking - Specialty Study Guide**: Preparation for AWS Advanced Networking - Specialty exam
- **Well Architecture Framework**: Best practices for AWS architecture
- **Cost Management**: AWS Budgets, Cost Explorer, cost optimization strategies

#### **Practical Skills Achieved**
- Create and configure complete VPC
- Setup networking components (IGW, NAT, Route Tables)
- Configure security (Security Groups, NACL)
- Practice VPC Peering and Transit Gateway
- Use AWS Management Console and CLI
- Manage costs with AWS Budgets
- Understand different Load Balancer types and use cases