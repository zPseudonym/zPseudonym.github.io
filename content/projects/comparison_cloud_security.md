---
date    : 2022-12-14
title   : Comparison of Public Cloud Hyperscalers From a Security Perspective
toc     : true
tech    : ["LaTeX"]
---
## Introduction

This project is about a comparison of the largest cloud service providers in terms of security features their public cloud platforms offer. The evaluation in an updated excerpt of a scientific research project from third semester of university back in late 2021/early 2022 which was graded 1.0/1.0.

## Evaluation
The approach is to get an overview of the latest market trends for public cloud services from a security perspective and to evaluate the portfolio of security features the six biggest public cloud providers (Amazon Web Services, Microsoft Azure, Google Cloud Platform, Alibaba Cloud, IBM Cloud and Oracle Cloud) offer with their product. It should be emphasized that the focus in this research is not to find the vendor with the best security mechanisms, but rather to evaluate which hyperscaler offers the most security features built-in their standard public cloud environment without the use of any third party additions.

For this approach, different security criteria — both technical and less technical — are selected and individually weighted. These criteria are Management, Network Security, Data Security, Identity and Access, Data Protection and Compliance; therefore not only the implementation of technical security features, but also the vendor's standpoint in terms of data privacy and compliance regulations matter for a service provider to reach the best scoring. Each of these categories contains multiple criteria within its group that are also weighted again inside its category.

The following table contains the comparison of all six public cloud vendors in terms of the defined criteria they are measured for:

| **Criteria** | **Weight** | **Amazon Web Services** | **Microsoft Azure** | **Google Cloud Platform** | **Alibaba Cloud** | **IBM Cloud** | **Oracle Cloud** |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Management** | **6 (100%)** | | | | | | |
| Monitoring System | 20% | Yes | Yes | Yes | Yes | Yes | Yes |
| Central Security Management | 15% | Yes | Yes | Yes | Yes | Yes | Yes |
| SIEM | 10% | No | Yes | Yes | No | Yes | No |
| Key Management | 15% | Yes | Yes | Yes | Yes | Yes | Yes |
| HSM service | 10% | Yes | Yes | Yes | Yes | Yes | Yes |
| Certificate management | 15% | Yes | Yes | Yes | Yes | Yes | Yes |
| Secret management | 15% | Yes | Yes | Yes | Yes | Yes | Yes |
| **Network Security** | **5 (100%)** | | | | | |
| Web Application Firewall | 40% | Yes | Yes | Yes | Yes | Yes | Yes
| DDoS Protection | 30% | Yes | Yes | Yes | Yes | Yes | Yes |
| Anomaly Detection | 15% | Yes | Yes | Yes | Yes | No | Yes |
| Anti-malware system | 15% | Yes | Yes | Yes | Yes | No | Yes
| **Data Security** | **3 (100%)** | | | | | |
| Encryption of data at rest | 60% | Yes/Yes | Yes/Yes | Yes/Yes | Yes/Yes | Yes/Yes | Yes/Yes |
| Protection of confidential data | 40% | Yes | Yes | Yes | Yes | Yes | Yes |
| **Identity and Access** | **2 (100%)** | | | | | |
| Identity & Access Management | 70% | Yes | Yes | Yes | Yes | Yes | Yes |
| Role-based Access Control | 30% | Yes | Yes | Yes | Yes | Yes | Yes |
| **Data Protection** | **2 (100%)** | | | | | |
| Standard Contractual Clauses | 75% | Yes | Yes | Yes | Yes | Yes | Yes
| EU Cloud Code of Conduct | 25% | Yes | Yes | Yes | Yes | Yes | Yes
| **Compliance** | **2 (100%)** | | | | | |
| ISO 27001/27017/27018 | 45% (15%/15%/15%) | Yes/Yes/Yes | Yes/Yes/Yes | Yes/Yes/Yes | Yes/Yes/Yes | Yes/Yes/Yes | Yes/Yes/Yes
| SOC 2/3 | 25% | Yes/Yes | Yes/Yes | Yes/Yes | Yes/Yes | Yes/Yes | Yes/Yes |
| PCI DSS/3DS | 10% | Yes/Yes | Yes/Yes | Yes/Yes | Yes/Yes | Yes/No | Yes/No
| CSA | 20% | Yes | Yes | Yes | Yes | Yes | Yes |
|
| **TOTAL** | **20** | **19.4** | **20** | **20** | **19.4** | **18.4** | **19.3** |

Namely, the products that offer the security functionalities of all six cloud service providers can be found in the following table, if applicable.

| **Criteria** | **Amazon Web Services** | **Microsoft Azure** | **Google Cloud Platform** | **Alibaba Cloud** | **IBM Cloud** | **Oracle Cloud** |
| --- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Management** | | | | | | | |
| Monitoring System | CloudWatch | Monitor | Cloud Monitoring | CloudMonitor | Cloud Monitoring | Cloud Monitoring |
| Central Security Management | Security Hub | Security Center | Security Command Center | Security Center | Cloud Security Advisor | Risk Management |
| SIEM | *N/A* | Sentinel | Chronicle SIEM | *N/A* | QRadar | *N/A* |
| Key Management | Key Management Service | Key Vault | Cloud KMS | Key Management Service | Key Protect | Vault |
| HSM service | CloudHSM | Dedicated HSM | Cloud HSM | Data Encryption Service | Cloud HSM | Vault |
| Certificate management | Certificate Manager | App Service Certificate | Certificate Authority Service | SSL Certificates Service | Certificate Manager | Certificates |
| Secret management | Secrets Manager | Key Vault | Secret Manager | Secrets Management | Secrets Manager | Vault |
| **Network Security** | | | | | |
| Web Application Firewall | WAF – Web Application Firewall | Web Application Firewall | Cloud Armor | Web Application Firewall (WAF) | Internet Services | Web Application Firewall (WAF) |
| DDoS Protection | Shield | DDoS Protection | Cloud Armor | Anti-DDoS | Internet Services | Layer 7 DDoS Mitigation |
| Anomaly Detection | CloudWatch | Anomaly Detector | BigQuery ML | Database Autonomy Service (DAS) | *N/A* | Anomaly Detection |
| Anti-malware system | GuardDuty | Antimalware | Chronicle Security | Security Center | *N/A* | Automated SaaS Cloud Security Services (ASCSS)|
| **Data Security** | | | | | |
| Protection of confidential data | Macie | Information Protection | Cloud Data Loss Prevention | Sensitive Data Discovery and Protection | Data Shield | Data Discovery |
| **Identity and Access** | | | | | |
| Identity & Access Management | Identity and Access Management | Identity and Access Management | Identity and Access Management (IAM) | Identity as a Service (IDaaS) | Identity and Access Management | Identity and Access Management |
| Role-based Access Control | Cognito | Role-Based Access Control | Identity and Access Management (IAM) | Role-based SSO | Role-based access control | Role-Based Access Control |



## Conclusion

As presented in the first table in the [Evaluation](#evaluation), both Microsoft Azure and Google Cloud Platform achieved the highest possible score — 20 out of 20 maximum points; their public cloud platforms tick every box that was checked in this evaluation.  
Amazon Web Services and Alibaba Cloud came in afterwards with a score of 19.4/20, fulfilling almost all criteria, but missing the feature of an integrated SIEM.  
Oracle's cloud platform performed similarly achieving a score of 19.3, not offering an integrated SIEM but also not being compliant with the PCI 3DS standard.  
Last place in this comparison went to IBM Cloud, which especially lacks the functionalities of built-in anomaly detection and anti-malware, with a total score of 18.4.   

## What I Have Learned From This Project
This project allowed me to research the different aspects of security when it comes to cloud computing. As I needed to get a thorough understanding of the relevant security aspects to look for when making the decision to host company data in a vendor's infrastructure, I learned about the latest market trends and what functionalities really matter for securely managing a public cloud offered by the biggest vendors. On the non-technical side, I got the chance to familiarize myself with GDPR and other common compliance standards and their relevance when it comes to data privacy and security in cloud environments. As this scientific essay was originally written in LaTeX, I also had the chance to strengthen my skills there, too.