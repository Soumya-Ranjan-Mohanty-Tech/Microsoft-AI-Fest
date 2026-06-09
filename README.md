# Microsoft-AI-Fest
Security Pro - Strengthen security foundations - Build a core understanding of modern security concepts, threats, and protections across identities, data, devices, infrastructure, and AI systems.



# Interactive case study: Securing apps and data

# 1. This module introduces to a **real-world scenario** focused on **securing applications** and **data** across a **multicloud enterprise environment**. 

Based on Fabrikam Inc.’s infrastructure, the case study highlights common challenges in **application security**, **data governance**, and **cloud-native threat protection** faced by organizations operating across multicloud platforms.

Through this case study, i will analyze **business requirements, align technical capabilities** to meet those needs, and **design cohesive solutions** that **span identity**, **infrastructure**, and **application layers**.

[**In this case study module, I WILL:**]

**Explore risks**: related to **application code**, **CI/CD pipelines**, and **cloud configuration drift**.
**Learn**: how these challenges align with **Zero Trust principles** and **DevSecOps best practices**.
**Apply:** **architectural reasoning** to assess **threats** and **design secure**, **scalable solutions** for **multicloud environments**.


# 1.1 Scenario summary 
Fabrikam Inc. operates across **Azure and AWS**, **deploying microservices in AKS (Azure Kubernetes Service) and EKS(Amazon Elastic Kubernetes Service)** clusters. The organization faces fragmented **DevOps security practices, including manual secret rotation, public database endpoints, and inconsistent CI/CD pipeline controls**. These issues expose the organization to misconfigurations, credential leaks, and delayed threat detection—highlighting the need for integrated policy enforcement and unified visibility across cloud-native environments.

# 1.2 Understanding the Security Problems

**Problem 1: Manual Secret Rotation**

**What are secrets?**
Secrets are sensitive credentials such as:

Database passwords
API keys 
Encryption keys 
Service account credentials  

**Example**: Database Password: P@ssw0rd123

If developers store this inside source code: db_password = "P@ssw0rd123" and push it to GitHub, attackers can steal it.

**What is Secret Rotation?**

Changing credentials periodically.

**Example**:

January:
Password = ABC123

February:
Password = XYZ789
Why Manual Rotation Is Bad

**Humans forget**.

Result:

Old passwords remain active
Credentials leak
Attackers maintain access

**Better Solution**

Store secrets in: **Azure Key Vault** 
and automate rotation.

**Problem 2:** Public Database Endpoints
**What does this mean?**

A database is directly reachable from the internet.

**Bad architecture**:

Internet
   |
   |
Database

**Anyone can attempt**:

Password guessing
Exploitation
Scanning

**Better Architecture**
Internet
   |
Application
   |
Private Network
   |
Database

Database should be private and accessible only by authorized services.

**Problem 3**: Weak CI/CD Pipeline Controls
**What is CI/CD?**

CI/CD = Continuous Integration / Continuous Deployment.

**Typical workflow**:

Developer
    ↓
Git Repository
    ↓
Build Pipeline
    ↓
Testing
    ↓
Production


**What is Configuration Drift**?

Configuration Drift occurs when systems slowly become different from their approved secure configuration.

**Example**:

Initial state: Database: Public Access = Disabled

Later an administrator changes: Public Access = Enabled--------------Nobody notices.

Months later: Database exposed to internet

This is configuration drift.

**Why Dangerous?**

Security teams think: Everything follows policy

Reality: Many systems violate policy

Attackers often exploit these mistakes.


**Why Credential Leaks Matter**

Credentials are digital keys.

Examples:

Passwords
API keys
Access tokens

If leaked:

Attacker
    ↓
Uses credential
    ↓
Appears legitimate

Many breaches begin with stolen credentials rather than sophisticated hacking.


**Why Delayed Threat Detection Is Dangerous**

Suppose an attacker enters today.

Security team notices:

30 days later

The attacker now has:

Stolen data
Moved across systems
Created backdoors

Modern security aims to detect attacks within minutes or hours instead of weeks.

**How This Relates to Zero Trust**

Zero Trust means:

Never trust automatically. Always verify.

Traditional thinking:

Inside network = trusted

**Zero Trust**:

Every request must be verified.

Key principles: Verify Explicitly

**Check**:

Identity
Device
Location
Risk level
Least Privilege

**Give only necessary access**.

Example:

Developer:
Read database logs

Not:

Developer:
Full database admin
Assume Breach

Design as if attackers are already inside.

**How This Relates to DevSecOps**

**Traditional process**:

Develop
      ↓
Deploy
      ↓
Security Review

Security comes last.

**DevSecOps**:

Develop
      ↓
Security Scan
      ↓
Build
      ↓
Security Scan
      ↓
Deploy
      ↓
Monitor

Security is integrated throughout the lifecycle.

**Security Risks**

An attacker could:

Inject malicious code
Steal secrets
Modify deployment files
Deploy malware
Secure CI/CD Requires
Code scanning
Secret scanning
Signed artifacts
Access controls
Approval workflows

**This is part of DevSecOps**.



**Securing Apps and Data**
In the scenario i will access application and data security risks, identify potential threats, explore microsoft solution components, and review key implementation steps. Throughout the experience i will make decisions aligned with Zero trust principle to address evolving buisness and security needs.
Fabrikam Inc. Operates in Dual cloud Azure and AWS deployment with AKS(Azure Kubernetes sevices) and EKS(Amazon Elastic Kubernetes services) microservices for regional failover and performance optimisation. 
Buisiness critical data is centralised in **Azure with SQL database for transactions** and **Cosmos DB for customer profiles**- Azure hosted data with SQL adtabase and Cosmos DB for Governance.
Service communication relies on kubernets secrets for service communication with manual token rotation including API tokens and Connection Strings, using long lived tokens rotated manually with TLS encryption over public endpoints.
Git hub actions handles CI/CD with private repositories. Recent incidents included misconfigured admission policies and permissive role assignments requiring manual team coordiantion.
Developers use open osurtce packages/dependecies with ACR base and images and developer driven security and azure container Registry base images---------relying on vulnerability monitoring and periodic rebuilds for security. 
Monitoring uses Azure Monitor and Microsoft Sentinel with Custom analytics.
Operation team have varying reviewing processes focused on performance and diagnostics. 
My task as a asecurity architect is to analyse Fabrikam Inc., currenmt security posture and design a resilient zero trust architect that strengthens apps and data security without compromising operational continuity.

**What Microsoft Wants me to Learn**:
How do we protect secrets?
How do we secure Kubernetes environments?
How do we secure CI/CD pipelines?
How do we detect configuration drift?
How do we protect databases?
How do we implement Zero Trust?
How do we apply DevSecOps practices?
How do we secure applications running across Azure and AWS?

# 2. **Scenario**: 

Explore the security architecture, and identify the challenges involved.

# 2.1 **Situation assessment tab**: 
Architecture Overview
Developers
    ↓
GitHub Repos
    ↓
GitHub Actions (CI/CD)
    ↓
Container Images
    ↓
Azure Container Registry (ACR)
    ↓
AKS (Azure)
    ↓
Applications & Databases

           ↘
            EKS (AWS)

At the same time, user identities and access are managed through Microsoft Entra ID.

1. Identity Layer (Who Can Access?)

On the right side is the identity system:

Microsoft Entra Tenant

This is the organization's central identity provider.

Components shown:

Users
App Registrations
Enterprise Applications
Conditional Access
Entra MFA
Security Purpose
Users

Employee identities.

Example:

Alice
Bob
Charlie

These users authenticate through Entra ID.

Entra MFA

Multi-Factor Authentication.

Instead of:

Password only

Use:

Password
+
Phone approval

Even if a password is stolen, attackers still need the second factor.

Conditional Access

Controls who gets access and under what conditions.

Example:

Allow:
✓ Managed device
✓ India
✓ MFA completed

Block:
✗ Unknown country
✗ Unmanaged device

This is a core Zero Trust control.

App Registrations

Applications themselves get identities.

Example:

Inventory App
Payment API
Reporting Service

Applications can securely authenticate to Azure services.

Enterprise Applications

Third-party or SaaS applications integrated with Entra ID.

Examples:

Salesforce
ServiceNow
GitHub
2. DevOps Layer

Bottom-left:

GitHub Repositories

Source code storage.

Example:

Frontend code
Backend APIs
Infrastructure templates

Risk:

Hardcoded secrets
Vulnerable code
Malicious commits
GitHub Actions

CI/CD automation.

Workflow:

Developer pushes code
        ↓
Build
        ↓
Test
        ↓
Create container image
        ↓
Deploy

Risk:

Secret exposure
Pipeline manipulation
Unauthorized deployments
3. Container Registry
Azure Container Registry (ACR)

Stores container images.

myapp:v1
myapp:v2
myapp:v3

Before deployment, AKS pulls images from ACR.

Security concern:

If vulnerable images are stored here, vulnerabilities are deployed everywhere.

4. Kubernetes Layer
AKS (Azure Kubernetes Service)

Runs containerized microservices in Azure.

Example:

Authentication Service
Order Service
Payment Service
EKS (Amazon Elastic Kubernetes Service)

Runs Kubernetes workloads in AWS.

Fabrikam is therefore:

Multicloud

because workloads exist in:

Azure
AWS
5. Data Layer
Azure SQL Database

Structured relational data.

Example:

Customers
Orders
Invoices
Azure Cosmos DB

NoSQL database.

Good for:

Global applications
Large-scale data
Flexible schemas
6. Security Monitoring Layer
Microsoft Defender for Cloud

Cloud Security Posture Management (CSPM).

Detects:

Misconfigurations
Vulnerabilities
Exposed resources

Example:

Public database endpoint detected
Microsoft Sentinel

SIEM + SOAR platform.

Collects logs from:

AKS
EKS
Azure
AWS
Entra ID

Detects attacks and suspicious behavior.

Example:

Impossible travel login
Privilege escalation
Credential theft
Azure Monitor

Operational monitoring.

Tracks:

CPU
Memory
Application performance
Logs

Useful for troubleshooting and incident investigations.

What Security Weaknesses Are Likely Present?

The scenario description already hinted at several problems:

1. Manual Secret Rotation

Likely issue:

Passwords/API keys
stored manually

Risk:

Credential leaks
Stale secrets
2. Weak CI/CD Controls

GitHub Actions may lack:

Secret scanning
Code scanning
Approval gates

Risk:

Compromised pipeline
        ↓
Malicious deployment
3. Public Databases

Azure SQL or Cosmos DB may be internet-accessible.

Risk:

Internet
     ↓
Database

Instead of:

Application
     ↓
Private Database
4. Configuration Drift

AKS, EKS, databases, and identities may not consistently follow security policies.

Risk:

Open ports
Excessive permissions
Disabled security controls
Zero Trust Assessment

The architecture already contains some Zero Trust components:

✅ Entra MFA
✅ Conditional Access
✅ Centralized Identity
✅ Defender for Cloud
✅ Sentinel

However, it still appears to need:

❌ Strong secret management
❌ Private database access
❌ Consistent policy enforcement across Azure and AWS
❌ Secure CI/CD controls
❌ Automated compliance monitoring






















