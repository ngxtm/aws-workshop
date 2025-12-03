---
title: "Event 5"
date: 2025-11-29
weight: 5
chapter: false
pre: " <b> 4.5 </b> "
---

## AWS Cloud Mastery Series #3

### Event Information

&emsp; **Event Name:** AWS Well-Architected Security Pillar <br>
&emsp; **Date:** Saturday, November 29, 2025 <br>
&emsp; **Time:** 8:30 AM – 12:00 PM <br>
&emsp; **Location:** AWS Vietnam Office <br>
&emsp; **Main Topic:** Security Pillar - Identity & Access Management, Detection, Infrastructure Protection, Data Protection, Incident Response <br>
&emsp; **Role:** Attendee <br>

### Event Overview

This half-day workshop focused on the Security Pillar of the AWS Well-Architected Framework. What caught my attention right from the opening was how the speakers connected theoretical security principles to real-world scenarios in Vietnamese enterprises. The session covered five core pillars: IAM, Detection, Infrastructure Protection, Data Protection, and Incident Response - each with practical demonstrations and actionable insights.

![AWS Well-Architected Security Pillar Workshop](/images/Event-Participated/Event-5/IMG_2488.jpg)

<figure>
    <figcaption>Workshop opening at AWS Vietnam Office - Security Pillar deep dive session</figcaption>
</figure>

---

### Opening Session: Security Foundation (8:30 - 8:50 AM)

The workshop began with a fundamental question: Why is the Security Pillar considered the backbone of any Well-Architected workload? The answer became clear as we discussed the Shared Responsibility Model and the three core principles that guide AWS security design:

**Core Security Principles:**
- **Least Privilege:** Grant only the minimum permissions required for a task
- **Zero Trust:** Never trust, always verify - regardless of network location
- **Defense in Depth:** Implement multiple security layers to protect resources

What resonated with me was the discussion on top security threats in Vietnamese cloud environments. Many organizations still struggle with over-permissive IAM policies, lack of proper logging, and insufficient encryption practices. These are not sophisticated attacks - they are basic security hygiene issues that the Well-Architected framework directly addresses.

---

### Pillar 1: Identity & Access Management (8:50 - 9:30 AM)

This session was where theory met practice. The speaker walked us through modern IAM architecture, emphasizing one critical point: **avoid long-term credentials at all costs**.

![IAM Architecture and Best Practices](/images/Event-Participated/Event-5/IMG_2489.jpg)

<figure>
    <figcaption>IAM session covering Users, Roles, Policies, and the importance of eliminating long-term credentials</figcaption>
</figure>

**Key IAM Concepts Covered:**

**IAM Users, Roles, and Policies:**
- Users should be reserved for human identities, and even then, prefer federation
- Roles are the preferred mechanism for granting permissions to services and applications
- Policies should follow the principle of least privilege - start with deny all, then grant specific permissions

**IAM Identity Center (formerly AWS SSO):**
- Centralizes user management for multi-account environments
- Permission Sets provide a cleaner way to manage access across accounts
- Single sign-on experience reduces the need for multiple credentials

**Multi-Account Security:**
- Service Control Policies (SCPs) act as guardrails at the organization level
- Permission Boundaries limit the maximum permissions a role can have
- Together, they create a layered permission model

**Credential Security:**
- MFA should be mandatory for all human users
- Credential rotation policies for any remaining access keys
- IAM Access Analyzer continuously monitors for unintended access

**Live Demo: IAM Policy Validation**

The highlight of this session was the live demo where we validated an IAM policy and simulated access. Seeing how Access Analyzer identifies overly permissive policies in real-time was eye-opening. Many of us realized our existing policies would likely fail this validation.

---

### Pillar 2: Detection (9:30 - 9:55 AM)

Security without visibility is just hope. This pillar focused on building a detection foundation that can identify threats before they become incidents.

![Detection and Continuous Monitoring](/images/Event-Participated/Event-5/IMG_2490.jpg)

<figure>
    <figcaption>Detection pillar covering CloudTrail, GuardDuty, Security Hub, and logging strategies</figcaption>
</figure>

**Detection Components:**

**CloudTrail at Organization Level:**
- Enable organization-wide trails to capture all API activity
- Store logs in a centralized, protected S3 bucket
- Enable log file validation to detect tampering

**Amazon GuardDuty:**
- Intelligent threat detection using ML and threat intelligence
- Analyzes VPC Flow Logs, DNS logs, and CloudTrail events
- Identifies compromised instances, reconnaissance, and data exfiltration

**AWS Security Hub:**
- Aggregates findings from multiple security services
- Provides compliance checks against security standards (CIS, PCI DSS)
- Central dashboard for security posture management

**Multi-Layer Logging:**
- VPC Flow Logs capture network traffic metadata
- Application Load Balancer logs record request details
- S3 access logs track bucket operations

**EventBridge for Automation:**
- Route security findings to automated response workflows
- Trigger Lambda functions for immediate remediation
- Send notifications to the security team

The concept of **Detection-as-Code** was new to me. Just as we treat infrastructure as code, security detection rules should be version-controlled, tested, and deployed through CI/CD pipelines. This approach ensures consistency and allows for rapid iteration on detection capabilities.

---

### Coffee Break (9:55 - 10:10 AM)

The networking during coffee break was valuable. I connected with several security engineers from local companies who shared their challenges implementing these practices. The common theme: getting buy-in from development teams who see security as a blocker rather than an enabler.

---

### Pillar 3: Infrastructure Protection (10:10 - 10:40 AM)

Returning from the break, we dove into network and workload security. The speaker framed this section around one question: How do we protect resources while maintaining the agility that cloud promises?

![Infrastructure Protection - Network and Workload Security](/images/Event-Participated/Event-5/IMG_2491.jpg)

<figure>
    <figcaption>Infrastructure protection session covering VPC design, Security Groups, NACLs, and edge security services</figcaption>
</figure>

**Network Security Architecture:**

**VPC Segmentation:**
- Separate workloads into different VPCs based on sensitivity
- Use private subnets for databases and application servers
- Public subnets only for resources that absolutely need internet access
- VPC peering and Transit Gateway for controlled inter-VPC communication

**Security Groups vs NACLs:**
| Aspect | Security Groups | NACLs |
|--------|----------------|-------|
| Level | Instance | Subnet |
| State | Stateful | Stateless |
| Rules | Allow only | Allow and Deny |
| Use Case | Application firewall | Subnet-level filtering |

The practical application model presented was: Use Security Groups as the primary defense (deny by default, allow specific), and NACLs for additional subnet-level controls like blocking known malicious IP ranges.

**Edge Security Services:**

**AWS WAF (Web Application Firewall):**
- Protects against common web exploits (SQL injection, XSS)
- Custom rules for application-specific threats
- Managed rule groups from AWS and partners

**AWS Shield:**
- DDoS protection at no additional cost (Standard tier)
- Shield Advanced for enhanced protection and 24/7 access to DRT

**AWS Network Firewall:**
- Stateful inspection and intrusion prevention
- Domain filtering and protocol-level controls
- Centralized firewall management for multi-VPC architectures

**Workload Protection:**
- EC2: Use IMDSv2, disable SSH where possible, apply security patches
- ECS/EKS: Non-root containers, read-only file systems, limit capabilities
- The principle of immutable infrastructure: replace, don't patch

---

### Pillar 4: Data Protection (10:40 - 11:10 AM)

Data is the crown jewel. This pillar addressed how to protect data at rest and in transit, manage encryption keys, and classify data appropriately.

![Data Protection - Encryption and Secrets Management](/images/Event-Participated/Event-5/IMG_2492.jpg)

<figure>
    <figcaption>Data protection session covering KMS, encryption strategies, and secrets management</figcaption>
</figure>

**AWS KMS (Key Management Service):**

**Key Policies and Grants:**
- Key policies define who can use and manage the key
- Grants provide temporary, delegated access
- Separation between key administrators and key users

**Key Rotation:**
- Automatic annual rotation for customer managed keys
- Key material is rotated, but the key ID remains the same
- No re-encryption needed for data encrypted with the key

**Encryption Strategies:**

| Service | At Rest | In Transit |
|---------|---------|------------|
| S3 | SSE-S3, SSE-KMS, SSE-C | HTTPS, S3 endpoint policies |
| EBS | Default encryption with KMS | N/A (internal) |
| RDS | Encryption at creation | SSL/TLS connections |
| DynamoDB | Encryption by default | HTTPS endpoints |

**Secrets Management:**

**AWS Secrets Manager:**
- Store database credentials, API keys, OAuth tokens
- Automatic rotation with Lambda functions
- Cross-account secret sharing with resource policies

**AWS Systems Manager Parameter Store:**
- Store configuration data and secrets
- SecureString parameters encrypted with KMS
- Hierarchical organization for different environments

**Choosing Between Them:**
- Secrets Manager: When you need automatic rotation
- Parameter Store: For configuration data and simpler secret storage

**Data Classification and Guardrails:**

The discussion on data classification was practical. Not all data needs the same level of protection. The approach presented:
1. Tag resources with data classification levels
2. Use SCPs to enforce encryption requirements based on classification
3. Monitor with Config rules to detect non-compliant resources

---

### Pillar 5: Incident Response (11:10 - 11:40 AM)

The final pillar addressed what happens when things go wrong. Preparation is everything.

![Incident Response Playbook and Automation](/images/Event-Participated/Event-5/IMG_2493.jpg)

<figure>
    <figcaption>Incident response session covering IR lifecycle, playbooks, and automated remediation</figcaption>
</figure>

**AWS Incident Response Lifecycle:**

1. **Preparation:** Define runbooks, set up tooling, conduct game days
2. **Detection:** Leverage GuardDuty, CloudTrail, Security Hub findings
3. **Containment:** Isolate affected resources, preserve evidence
4. **Eradication:** Remove threats, patch vulnerabilities
5. **Recovery:** Restore services from clean backups
6. **Post-Incident:** Document lessons learned, update runbooks

**Incident Response Playbooks:**

The workshop covered three common scenarios with detailed response steps:

**Playbook 1: Compromised IAM Key**
- Immediately disable the access key
- Review CloudTrail for API calls made with the key
- Identify and revert any unauthorized changes
- Rotate all credentials that may have been exposed
- Enable MFA if not already enabled

**Playbook 2: S3 Public Exposure**
- Apply S3 Block Public Access at account level
- Review bucket policies and ACLs
- Check CloudTrail for data access events
- Enable S3 Object Lock for sensitive data
- Set up Config rules to prevent future exposure

**Playbook 3: EC2 Malware Detection**
- Isolate the instance (modify security group to deny all)
- Create EBS snapshots for forensic analysis
- Terminate the instance
- Analyze snapshots in an isolated forensic environment
- Deploy new instance from clean AMI

**Evidence Collection:**
- Create snapshots before terminating resources
- Export CloudTrail logs to a protected bucket
- Capture memory dumps if possible
- Document the timeline of events

**Automated Response with Lambda and Step Functions:**

This was the most exciting part. The speaker demonstrated how to build automated response workflows:
- GuardDuty finding triggers EventBridge rule
- EventBridge invokes Step Functions workflow
- Step Functions orchestrates multiple Lambda functions
- Actions: isolate instance, notify team, capture evidence, create ticket

The benefit: response time drops from hours to seconds, and human error is eliminated from the initial containment steps.

---

### Wrap-Up Session (11:40 - 12:00 PM)

![Workshop Wrap-Up and Q&A](/images/Event-Participated/Event-5/IMG_2494.jpg)

<figure>
    <figcaption>Closing session summarizing the five security pillars and addressing questions from attendees</figcaption>
</figure>

**Summary of Five Pillars:**

| Pillar | Key Focus | Critical Service |
|--------|-----------|------------------|
| IAM | Least privilege, no long-term credentials | IAM Identity Center, Access Analyzer |
| Detection | Visibility at every layer | CloudTrail, GuardDuty, Security Hub |
| Infrastructure | Network segmentation, edge protection | VPC, WAF, Shield, Network Firewall |
| Data | Encryption everywhere, key management | KMS, Secrets Manager |
| Incident Response | Preparation and automation | Step Functions, Lambda, EventBridge |

**Common Pitfalls in Vietnamese Enterprises:**

The speakers shared observations from their consulting experience:
- Over-reliance on perimeter security without internal segmentation
- Logging enabled but never analyzed
- Encryption keys managed alongside the data they protect
- Incident response plans that exist on paper but are never tested
- Security treated as a one-time project rather than continuous process

**Security Learning Roadmap:**

For those wanting to deepen their security expertise:
1. **Foundation:** AWS Certified Cloud Practitioner + Security Fundamentals
2. **Associate:** AWS Certified Solutions Architect Associate
3. **Professional:** AWS Certified Solutions Architect Professional
4. **Specialty:** AWS Certified Security - Specialty

---

### Workshop Highlights

![Workshop Participants and Interactive Session](/images/Event-Participated/Event-5/IMG_2496.jpg)

<figure>
    <figcaption>Workshop participants engaging in interactive discussions and demonstrations</figcaption>
</figure>

![Security Architecture Discussion](/images/Event-Participated/Event-5/IMG_2498.jpg)

<figure>
    <figcaption>Deep dive into security architecture patterns and real-world implementation challenges</figcaption>
</figure>

![Group Photo - AWS Security Pillar Workshop](/images/Event-Participated/Event-5/IMG_2500.jpg)

<figure>
    <figcaption>Workshop attendees at AWS Vietnam Office</figcaption>
</figure>

![AWS Vietnam Office](/images/Event-Participated/Event-5/IMG_2501.jpg)

<figure>
    <figcaption>AWS Vietnam Office venue for the Security Pillar workshop</figcaption>
</figure>

---

### Key Takeaways

**Technical Insights:**

1. **IAM is the foundation:** Without proper identity management, all other security controls can be bypassed. Invest time in getting IAM right.

2. **Detection enables response:** You cannot respond to what you cannot see. Enable comprehensive logging and actually review the logs.

3. **Automation is not optional:** Manual security processes do not scale. Build automated detection and response from day one.

4. **Defense in depth works:** No single control is sufficient. Layer multiple controls so that if one fails, others still protect the asset.

5. **Encryption should be default:** With KMS, encryption adds minimal overhead. There is no excuse for unencrypted data in 2025.

**Practical Applications:**

From this workshop, I am taking away several actionable items:
- Audit existing IAM policies using Access Analyzer
- Enable organization-level CloudTrail with log validation
- Implement automated response for common GuardDuty findings
- Review encryption status across all data stores
- Create and test incident response playbooks for our environment

**Cultural Shift:**

The most important insight was not technical. Security must be a shared responsibility across development, operations, and security teams. The Well-Architected framework provides a common language for these conversations.

---

### Personal Reflections

This workshop reinforced something I have been thinking about: security in the cloud is fundamentally different from traditional on-premises security. The attack surface is larger, but so are the defensive capabilities. Services like GuardDuty and Security Hub provide detection capabilities that would require significant investment to build in-house.

What struck me most was the emphasis on automation. In traditional security, we often rely on humans to detect and respond to threats. In AWS, we can codify our security knowledge into automated workflows that respond in seconds. This is a paradigm shift that many organizations have not yet embraced.

The discussion on Vietnamese enterprise challenges was particularly relevant. Many companies are in the early stages of cloud adoption and are making the same mistakes that others have made before. The Well-Architected framework, and specifically the Security Pillar, provides a roadmap to avoid these pitfalls.

Moving forward, I plan to:
1. Conduct a Security Pillar review of our current workloads
2. Implement the IAM best practices discussed, particularly around eliminating long-term credentials
3. Build automated incident response playbooks for the most common scenarios
4. Share these learnings with my team and advocate for security as a development priority

The half-day format was efficient - focused content without unnecessary filler. I appreciated that the speakers did not just present theory but shared real-world experiences and common mistakes they have seen in their consulting work. This practical perspective made the content immediately applicable.

---

*This workshop provided a comprehensive foundation in AWS security practices aligned with the Well-Architected framework. The combination of theoretical principles, practical demonstrations, and real-world insights makes the Security Pillar approachable for teams at any stage of their cloud journey. Security is not a destination but a continuous process - this workshop gave me the tools and knowledge to improve that process in my organization.*
