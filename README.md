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
Git hub actrions handles CI/CD with private repositories. Recent incidents included misconfigured admission policies and permissive role assignments requiring manual team coordiantion.
Developers use open osurtce packages/dependecies with ACR base and images and developer driven security and azure container Registry base images---------relying on vulnerability monitoring and periodic rebuilds for security. Monitoring uses Azure Monitor and Microsoft Sentinel with Custom analytics.
Operation team have varying reviewing processes focused on performance and diagnostics. 
My task as a asecurity architect is to analyse Fabrikam Inc., currenmt security posture and design a zero trust architect.
























