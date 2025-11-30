# 🚀 Project: Scalable and Secure 3-Tier Application on AWS
This repository deploys a highly available, scalable, and secure 3-Tier architecture on AWS, featuring a Python Streamlit Frontend, a Spring Boot Backend, and an RDS MySQL database, fronted by CloudFront and protected by WAF.



## Screenshots

![App Screenshot](https://github.com/mahigore/AWS-3-tier-achitecture/blob/main/Screenshots/aws.jpg)


## 1. 🏗️ Architecture Overview
The application is structured into three distinct tiers, deployed across private and public subnets in multiple Availability Zones for High Availability.
| **Tier** | **Component** | **AWS Service** | **Purpose** |
| :--- | :--- | :--- | :--- |
| Edge/CDN (Public) | Global Entry Point | CloudFront, Route 53, WAF | Global low-latency delivery, SSL/HTTPS termination, DDoS/Exploit protection, Origin Failover. |
| Presentation (FE) | Frontend Layer | ALB, ASG (EC2), Streamlit | Internet-facing application load balancing, horizontal scaling, and user interaction. |
| Application (BE) | Backend Layer | NLB, ASG (EC2), Spring Boot | Internal application logic, API processing, and secure communication with the database. |
| Data (Isolated) | Database Layer | RDS MySQL | Secure, managed, and highly available relational data storage. |


### **Create VPC**

 1. name = `cheetah-dev`
 2. **IPv4 CIDR = 10.0.0.0/16**

---

### **Create Subnets**

- **CIDRs**
- **VPC ID = cheetah-dev**

| Subnet Type     | Subnet Name            | AZ  | **IPv4 subnet CIDR block** |
|-----------------|------------------------|-----|-----------------------------|
| public subnet   | cheetah-dev-pub-1a     | 1a  | 10.0.0.0/24                 |
| public subnet   | cheetah-dev-pub-1b     | 1b  | 10.0.1.0/24                 |
| public subnet   | cheetah-dev-pub-1c     | 1c  | 10.0.2.0/24                 |
| private subnet  | cheetah-dev-pri-2a     | 1a  | 10.0.3.0/24                 |
| private subnet  | cheetah-dev-pri-2b     | 1b  | 10.0.4.0/24                 |
| private subnet  | cheetah-dev-pri-2c     | 1c  | 10.0.5.0/24                 |

---

### **Internet Gateway**

1. name = `cheetah-dev-igw`
2. attach to VPC → `cheetah-dev`

---

### **NAT Gateway**

1. name = `cheetah-dev-nat-igw`
2. **Subnet =** `cheetah-dev-pub-1a`
3. **Connectivity type = Public**
4. allocate Elastic IP → **Elastic IP allocation ID**

---

### **Route Tables**

---

#### **Public Route Table**

1. name = `cheetah-dev-pub-rt`
2. VPC = `cheetah-dev`
3. subnet association = 3 public subnets  
4. add routes:
   - **Destination = `0.0.0.0/0`**  
   - **Target = Internet Gateway → `cheetah-dev-igw`**

---

#### **Private Route Table**

1. name = `cheetah-dev-pri-rt`
2. VPC = `cheetah-dev`
3. subnet association = 3 private subnets  
4. add routes:
   - **Destination = `0.0.0.0/0`**
   - **Target = NAT Gateway → `cheetah-dev-nat-igw`**


## Key AWS Features Utilized

- VPC: Custom VPC with Public, Private, and Isolated Subnets across multiple AZs.

- Networking: NAT Gateway for private subnet internet access.

- High Availability: All ASGs and RDS are deployed across multiple AZs.

- Scaling: Auto Scaling Groups (ASG) with Scheduling for cost optimization.

- Security: WAF Geo-blocking, Security Groups (SG), and IAM Instance Profiles (Least Privilege).

- Observability: CloudWatch Metric Filters, SNS, and Lambda for Slack Alerting.
## 2. ⚙️ Deployment and Scaling Details
### 2.1 Backend (Application Tier) Configuration

| **Component** | **Detail** | **DevOps Concept** |
| :--- | :--- | :--- |
| **Application** | Spring Boot (Java). Logs exported to CloudWatch via CloudWatch Agent. | **Observability / Logging** |
| **Load Balancer** | NLB (Network Load Balancer). Used for high performance and minimal latency to the BE API. | **Performance / Load Balancing** |
| **Deployment** | ASG Launch Template. Uses user-data to install dependencies, fetch code from S3, and run the app as a `systemd` service. | **Infrastructure as Code (IaC) / Automation** |
| **Graceful Scaling** | Lifecycle Hooks. Ensures instances complete application startup before receiving traffic (`pending:wait`). | **High Availability (HA) / Zero-Downtime Deployment** |

## 2.2 Frontend (Presentation Tier) Configuration
| **Component** | **Detail** | **DevOps Concept** |
| :--- | :--- | :--- |
| **Application** | Streamlit (Python). Relies on a **Python Virtual Environment** (`venv`) for isolated dependencies. | **Dependency Isolation** |
| **Load Balancer** | **ALB (Application Load Balancer)**. Handles HTTP/S traffic (Port 443/80) and redirects to the FE application port (8501). | **Layer 7 Load Balancing** |
| **Connectivity** | **Environment Variable** `API_URL` set in `user-data` to point to the NLB DNS endpoint. | **Decoupling / Configuration Management** |

## 4. 🚨 Monitoring and Alerting Pipeline

The system is configured for instantaneous error detection and notification to the #alerting Slack channe

1. CloudWatch Log Group: Application logs are aggregated here.
2. Metric Filter: Detects the pattern ERROR in the logs.Metric Name: Datastore-Error-App-Metric
3. CloudWatch Alarm: Triggers when Datastore-Error-App-Metric $\ge 1$.SNS Topic: Receives the alarm notification.Topic: cheetah-dev-notify-topic
4. Lambda Function: Subscribed to the SNS topic. It parses the JSON message and formats the notification.
5. Slack Integration: The Lambda uses a secured Incoming Webhook URL (passed via environment variable) to post the alert to Slack.
