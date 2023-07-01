---
date    : 2023-07-01
title   : Comparative Security Assessment of Public Cloud Hyperscalers
toc     : true
---
## Introduction

This project delves into a comparative analysis of the largest cloud service providers, primarily focusing on the security features offered by their public cloud platforms. The evaluation serves as an updated snippet from an academic research project completed during the third semester of university in late 2021/early 2022, which was graded a score of 1.0/1.0.

## Evaluation
The study aims to provide a comprehensive overview of the contemporary market trends for public cloud services, specifically from a security standpoint. It assesses the security features portfolio provided by the top six public cloud providers, namely Amazon Web Services, Microsoft Azure, Google Cloud Platform, Alibaba Cloud, IBM Cloud, and Oracle Cloud. The purpose of this research isn't necessarily to identify the provider with superior security mechanisms, but rather to gauge which hyperscaler offers the most robust security features within their standard public cloud environment, excluding third-party additions.

The methodology entails selecting and individually weighing a variety of security criteria, both technical and non-technical. These criteria include Administration, Network Security, Data Security, Identity and Access, Data Protection, and Compliance. Hence, not only the implementation of technical security features but also the vendor's stance on data privacy and compliance regulations contribute to a service provider's overall score. Each category comprises multiple criteria, each further weighted within its respective group.

The subsequent [table 1]( {{< ref "comparison_cloud_security.md#table1" >}} ) presents a comparison of all six public cloud vendors, evaluated against the predefined criteria:

<h6 id="table1">Table 1: Evaluation Matrix</h6>

| **Criteria** | **Weight** | **Amazon Web Services** | **Microsoft Azure** | **Google Cloud Platform** | **Alibaba Cloud** | **IBM Cloud** | **Oracle Cloud** |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Administration** | **6 (100%)** | | | | | | |
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

The [table 2]( {{< ref "comparison_cloud_security.md#table2" >}} ) below, where applicable, provides information on the products that deliver the security functionalities of all six cloud service providers:

<h6 id="table2">Table 2: Security Services of Cloud Providers</h6>

| **Criteria** | **Amazon Web Services** | **Microsoft Azure** | **Google Cloud Platform** | **Alibaba Cloud** | **IBM Cloud** | **Oracle Cloud** |
| --- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Administration** | | | | | | | |
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

As delineated in the initial table under the [Evaluation](#evaluation) section, both Microsoft Azure and Google Cloud Platform have accomplished top marks, scoring a perfect 20 out of 20. Their public cloud platforms successfully meet all criteria examined in this evaluation.

Close on their heels are Amazon Web Services and Alibaba Cloud, both with a score of 19.4 out of 20. They nearly meet all requirements, but fall short by not featuring an integrated SIEM.

Oracle's cloud platform follows suit with a score of 19.3. Similar to AWS and Alibaba Cloud, it lacks an integrated SIEM, but also fails to comply with the PCI 3DS standard.

Ranking last in this comparative analysis is IBM Cloud, primarily due to its absence of built-in anomaly detection and anti-malware functionalities, accruing a total score of 18.4.

The evaluations presented in this study pertain to the security features of the stated cloud hyperscalers as observed at the time of the study, acknowledging that the dynamic nature of cloud security may yield different results in future assessments.