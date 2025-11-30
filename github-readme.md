# AWS 3-Tier Architecture

[![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white)](https://www.terraform.io/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

> A production-ready, highly available, and scalable 3-tier application architecture deployed on AWS with comprehensive monitoring and alerting.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Implementation Guide](#implementation-guide)
- [Monitoring & Alerts](#monitoring--alerts)
- [Security](#security)
- [Cost Optimization](#cost-optimization)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## 🎯 Overview

This project demonstrates the implementation of a production-grade 3-tier web application on AWS, following industry best practices for security, scalability, and high availability.

### Application Tiers

- **Tier 1 (Presentation)**: Python Streamlit application serving the frontend
- **Tier 2 (Application)**: Spring Boot REST API handling business logic  
- **Tier 3 (Data)**: MySQL RDS database for persistent storage

## 🏗️ Architecture

![Architecture Diagram](./architecture-diagram.png)

### Key Components

- **Networking**: Custom VPC with public/private subnets across 3 AZs
- **Compute**: EC2 Auto Scaling Groups for both frontend and backend
- **Load Balancing**: ALB for frontend, NLB for backend
- **Database**: RDS MySQL with automated backups
- **CDN**: CloudFront with custom domain and SSL
- **Security**: WAF, Security Groups, private subnets
- **Monitoring**: CloudWatch + SNS + Lambda → Slack notifications

## ✨ Features

- ✅ **High Availability**: Multi-AZ deployment across 3 availability zones
- ✅ **Auto Scaling**: Dynamic scaling based on traffic and schedules
- ✅ **Secure Architecture**: Private subnets, security groups, WAF protection
- ✅ **SSL/TLS Encryption**: Custom domain with ACM certificates
- ✅ **Centralized Logging**: CloudWatch Logs with custom metric filters
- ✅ **Real-time Alerting**: Automated Slack notifications for errors
- ✅ **Cost Optimized**: Scheduled scaling for non-business hours
- ✅ **Infrastructure Monitoring**: CloudWatch dashboards and alarms

## 🛠️ Technologies Used

### AWS Services

| Category | Services |
|----------|----------|
| **Compute** | EC2, Auto Scaling Groups, Launch Templates |
| **Networking** | VPC, Subnets, Internet Gateway, NAT Gateway, Route Tables |
| **Load Balancing** | Application Load Balancer (ALB), Network Load Balancer (NLB) |
| **Database** | RDS MySQL |
| **Storage** | S3 |
| **DNS & CDN** | Route 53, CloudFront, ACM |
| **Security** | Security Groups, IAM, WAF |
| **Monitoring** | CloudWatch, SNS, Lambda |

### Application Stack

- **Frontend**: Python, Streamlit
- **Backend**: Java, Spring Boot
- **Database**: MySQL 8.5.6
- **DevOps**: AWS CLI, CloudWatch Agent

## 📋 Prerequisites

- AWS Account with appropriate permissions
- Domain name (for Route 53 and ACM)
- Slack workspace (for alerting)
- Basic knowledge of:
  - AWS services (VPC, EC2, RDS, etc.)
  - Linux command line
  - Python and Java applications
  - Networking concepts

## 🚀 Getting Started

### Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/mahigore/AWS-3-tier-architecture.git
   cd AWS-3-tier-architecture
   ```

2. **Review the architecture**
   - Check the [architecture diagram](./architecture-diagram.png)
   - Read the [detailed documentation](https://mahigore.notion.site/AWS-3-Tier-Architecture-2baab8d2629880c283deff9eebe2f3a8)

3. **Follow the implementation guide**
   - See [Implementation Guide](#implementation-guide) below
   - Or follow the step-by-step [Notion documentation](https://mahigore.notion.site/AWS-3-Tier-Architecture-2baab8d2629880c283deff9eebe2f3a8)

## 📂 Project Structure

```
AWS-3-tier-architecture/
├── README.md
├── architecture-diagram.png
├── docs/
│   ├── implementation-guide.md
│   ├── troubleshooting.md
│   └── best-practices.md
├── scripts/
│   ├── backend-userdata.sh
│   ├── frontend-userdata.sh
│   └── cloudwatch-config.json
├── lambda/
│   └── slack-notification.py
├── policies/
│   ├── lifecycle-hook-policy.json
│   └── instance-role-policy.json
├── screenshots/
│   ├── vpc-diagram.png
│   ├── target-groups.png
│   └── monitoring-dashboard.png
└── applications/
    ├── backend/
    │   └── datastore-0.0.7.jar
    └── frontend/
        └── app.py
```

## 📖 Implementation Guide

### Step 1: Network Infrastructure

```bash
# Create VPC
VPC Name: cheetah-dev
CIDR: 10.0.0.0/16

# Create 6 Subnets (3 public, 3 private)
# Create Internet Gateway and NAT Gateway
# Configure Route Tables
# Set up Security Groups
```

**Subnet Layout:**

| Type | Name | AZ | CIDR |
|------|------|-----|------|
| Public | cheetah-dev-pub-1a | ap-southeast-1a | 10.0.0.0/24 |
| Public | cheetah-dev-pub-1b | ap-southeast-1b | 10.0.1.0/24 |
| Public | cheetah-dev-pub-1c | ap-southeast-1c | 10.0.2.0/24 |
| Private | cheetah-dev-pri-2a | ap-southeast-1a | 10.0.3.0/24 |
| Private | cheetah-dev-pri-2b | ap-southeast-1b | 10.0.4.0/24 |
| Private | cheetah-dev-pri-2c | ap-southeast-1c | 10.0.5.0/24 |

**Security Groups:**

| Name | Port | Source |
|------|------|--------|
| cheetah-dev-sg-alb | 80 | 0.0.0.0/0 |
| cheetah-dev-sg-fe-asg | 8501 | cheetah-dev-sg-alb |
| cheetah-dev-sg-nlb | 80 | cheetah-dev-sg-fe-asg |
| cheetah-dev-sg-be-asg | 8084 | cheetah-dev-sg-nlb |
| cheetah-dev-sg-db | 3306 | cheetah-dev-sg-be-asg |

### Step 2: Database Layer

```bash
# Create RDS Subnet Group
# Launch RDS MySQL instance in private subnets
# Configure security group for database access
# Enable automated backups and monitoring
```

### Step 3: Backend (Application Tier)

```bash
# Create IAM role with necessary policies
# Create S3 bucket and upload Spring Boot JAR
# Create backend target group
# Create launch template with user data script
# Create Auto Scaling Group
# Set up lifecycle hooks
# Create Network Load Balancer
```

**Backend User Data:**
```bash
#!/bin/bash
# Install Java 11
yum install java-11-amazon-corretto -y

# Download application from S3
aws s3 cp s3://bucket-name/datastore-0.0.7.jar /home/ec2-user/

# Configure and start application
java -jar datastore-0.0.7.jar

# Install and configure CloudWatch agent
# See scripts/backend-userdata.sh for complete script
```

### Step 4: Frontend (Presentation Tier)

```bash
# Create S3 bucket and upload Streamlit app
# Create frontend target group
# Create launch template with user data script
# Create Auto Scaling Group
# Set up lifecycle hooks
# Create Application Load Balancer
```

**Frontend User Data:**
```bash
#!/bin/bash
# Install Python and dependencies
dnf install -y python3-pip
python3 -m venv /root/venv
pip install streamlit requests

# Download application from S3
aws s3 cp s3://bucket-name/app.py /root/

# Create systemd service
# See scripts/frontend-userdata.sh for complete script
```

### Step 5: CDN & Security

```bash
# Create CloudFront distribution
# Configure behaviors for WebSocket support
# Request ACM certificate (in us-east-1)
# Configure Route 53 hosted zone
# Add custom domain to CloudFront
# Set up WAF rules
```

### Step 6: Monitoring & Alerting

```bash
# Create CloudWatch log groups
# Set up metric filters for ERROR patterns
# Create CloudWatch alarms
# Set up SNS topic
# Create Slack webhook
# Deploy Lambda function for Slack notifications
# Subscribe Lambda to SNS topic
```

**Lambda Function:**
```python
import json
import os
import urllib.request

def lambda_handler(event, context):
    slack_webhook_url = os.environ.get("slackHookUrl")
    message = json.loads(event['Records'][0]['Sns']['Message'])
    
    alarm_name = message['AlarmName']
    new_state = message['NewStateValue']
    reason = message['NewStateReason']
    
    slack_message = {
        'text': f"{alarm_name} state is now {new_state}: {reason}"
    }
    
    # Send to Slack
    # See lambda/slack-notification.py for complete code
```

## 📊 Monitoring & Alerts

### CloudWatch Metrics

- EC2 instance health and performance
- Target group health status
- RDS database metrics
- Application error logs

### Custom Alerts

- **ERROR Log Pattern**: Triggers when ERROR appears in logs
- **Target Unhealthy**: Alerts when targets become unhealthy
- **High CPU**: Notifications for sustained high CPU usage
- **Database Connections**: Monitors RDS connection count

### Slack Integration

All critical alerts are forwarded to Slack for real-time team notifications.

## 🔒 Security

### Network Security
- ✅ Private subnets for application and database tiers
- ✅ Security groups with least privilege principle
- ✅ NAT Gateway for outbound internet access
- ✅ No direct public access to backend/database

### Application Security
- ✅ SSL/TLS encryption via CloudFront and ACM
- ✅ WAF rules for geo-blocking and rate limiting
- ✅ IAM roles with specific permissions
- ✅ Encrypted RDS storage

### Best Practices Implemented
- ✅ No hardcoded credentials (use IAM roles)
- ✅ Regular security group audits
- ✅ Automated patching via scheduled maintenance
- ✅ CloudWatch Logs for audit trails

## 💰 Cost Optimization

### Implemented Strategies

1. **Scheduled Scaling**
   ```
   Scale down during non-business hours (7:30 PM - 6:00 AM)
   Cron: 30 19 * * 1-5  # Scale down
   Cron: 00 6 * * 1-5   # Scale up
   ```

2. **Right-sizing Instances**
   - Use appropriate instance types (t2.medium)
   - Monitor and adjust based on actual usage

3. **RDS Optimization**
   - Single-AZ deployment for dev/test
   - Automated backups with retention policies

4. **S3 Lifecycle Policies**
   - Archive old logs to Glacier
   - Delete temporary files automatically

## 🐛 Troubleshooting

### Common Issues

**Issue: Instances not healthy in target group**
```bash
# Check security group rules
aws ec2 describe-security-groups --group-ids sg-xxx

# Verify health check configuration
aws elbv2 describe-target-health --target-group-arn arn:xxx

# Check application logs
aws logs tail /datastore/app --follow
```

**Issue: CloudFront not loading**
- Verify origin points to ALB
- Check CloudFront behavior settings
- Ensure SSL certificate is in us-east-1
- Clear CloudFront cache

**Issue: Database connection timeout**
- Verify security group allows traffic from backend SG
- Check RDS endpoint configuration
- Ensure proper subnet routing

**Issue: Slack notifications not working**
- Check Lambda environment variables
- Verify SNS subscription status
- Review Lambda CloudWatch logs
- Test webhook URL manually

For more troubleshooting tips, see [docs/troubleshooting.md](./docs/troubleshooting.md)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Contact

**Mahesh Gore** - DevOps Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/maheshgoredevops)
[![Medium](https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white)](https://medium.com/@maheshgore7888)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=notion&logoColor=white)](https://mahigore.notion.site)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/mahigore)

📧 Email: maheshgore@zohomail.in

---

## 📚 Additional Resources

- [Complete Project Documentation](https://mahigore.notion.site/AWS-3-Tier-Architecture-2baab8d2629880c283deff9eebe2f3a8)
- [Medium Article - Detailed Implementation](https://medium.com/@maheshgore7888)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Best Practices](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/)

---

## ⭐ Show Your Support

If you found this project helpful, please give it a ⭐️!

---

**Tags:** `aws` `devops` `3-tier-architecture` `cloud-computing` `infrastructure` `auto-scaling` `monitoring` `terraform` `cloudformation` `docker` `kubernetes`