# 🚀 Project: Scalable and Secure 3-Tier Application on AWS
This repository deploys a highly available, scalable, and secure 3-Tier architecture on AWS, featuring a Python Streamlit Frontend, a Spring Boot Backend, and an RDS MySQL database, fronted by CloudFront and protected by WAF.

## 1. 🏗️ Architecture Overview
The application is structured into three distinct tiers, deployed across private and public subnets in multiple Availability Zones for High Availability.
| **Tier** | **Component** | **AWS Service** | **Purpose** |
| :--- | :--- | :--- | :--- |
| Edge/CDN (Public) | Global Entry Point | CloudFront, Route 53, WAF | Global low-latency delivery, SSL/HTTPS termination, DDoS/Exploit protection, Origin Failover. |
| Presentation (FE) | Frontend Layer | ALB, ASG (EC2), Streamlit | Internet-facing application load balancing, horizontal scaling, and user interaction. |
| Application (BE) | Backend Layer | NLB, ASG (EC2), Spring Boot | Internal application logic, API processing, and secure communication with the database. |
| Data (Isolated) | Database Layer | RDS MySQL | Secure, managed, and highly available relational data storage. |


# AWS 3-Tier Architecture

## [Link to my project blog](https://www.notion.so/AWS-3-Tier-Architecture-2bbab8d2629880449a47f1e0c30695f3)

## [Medium article]([Add your Medium post link here](https://medium.com/@maheshgore7888/building-a-production-ready-3-tier-application-on-aws-a-complete-guide-0409207850a5))

---

## 🔗 Connect with Me

[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?&style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/maheshgoredevops/)
[![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)](https://github.com/mahigore)
[![Medium](https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white)](https://medium.com/@maheshgore7888)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=notion&logoColor=white)](https://mahigore.notion.site/Mahesh-Gore-DevOps-Engineer-b306fb1333b7451489b465242783183b)
