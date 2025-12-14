# Two-Tier Application Deployment on AWS

📌 Overview

This repository demonstrates a **two-tier application architecture** deployed on **Amazon Web Services (AWS)** following best practices for **security, scalability, and high availability**.

The architecture separates the **presentation / application layer** and the **data layer**, deploying them in **public and private subnets** across **multiple Availability Zones** in the **Mumbai region (ap-south-1)**.

---

🏗️ Architecture Overview

Two Tiers

1. **Web / Application Tier**

   * EC2 instances
   * Hosted in **private subnets**
   * Accessible through an **Application Load Balancer (ALB)**

2. **Database Tier**

   * Amazon RDS (MySQL/PostgreSQL)
   * Hosted in **private subnets**
   * No direct internet access

---

🌍 AWS Infrastructure Components

Region & Availability Zones

* **Region:** ap-south-1 (Mumbai)
* **Availability Zones:**

  * ap-south-1a
  * ap-south-1b

Networking

* **VPC:** Custom VPC (10.0.0.0/16)
* **Subnets:**

  * Public Subnet (AZ-1)
  * Public Subnet (AZ-2)
  * Private Subnet (AZ-1)
  * Private Subnet (AZ-2)

Gateways & Routing

* **Internet Gateway (IGW)**

  * Attached to VPC
  * Used by public subnets

* **NAT Gateway**

  * Deployed in public subnet
  * Enables outbound internet access for private subnets

* **Route Tables**

  * Public Route Table → `0.0.0.0/0 → IGW`
  * Private Route Table → `0.0.0.0/0 → NAT Gateway`

---

⚖️ Load Balancer

* **Type:** Application Load Balancer (ALB)
* **Placement:** Public subnets (multi-AZ)
* **Function:**

  * Receives internet traffic
  * Forwards requests to EC2 instances in private subnets

---

🖥️ Compute Layer

* **EC2 Instances**

  * Deployed in private subnets
  * Part of a Target Group
  * Auto Scaling Group (optional)

* **Web Server:** Nginx / Apache

* **Application:** Sample HTML / backend service

---

🗄️ Database Layer

* **Service:** Amazon RDS
* **Subnet Group:** Private subnets only
* **Public Access:** Disabled
* **Security:** Accessible only from application tier

---
🔐 Security

Security Groups

* **ALB Security Group**

  * Allow HTTP/HTTPS from `0.0.0.0/0`

* **Application Security Group**

  * Allow traffic only from ALB Security Group

* **Database Security Group**

  * Allow traffic only from Application Security Group

Network ACLs

* Stateless protection at subnet level
* Allows only required ports

---

🔁 Traffic Flow

```
User
  ↓
Internet
  ↓
Application Load Balancer (Public Subnet)
  ↓
EC2 Application Servers (Private Subnet)
  ↓
RDS Database (Private Subnet)
```

---

🚀 Deployment Steps (High Level)

1. Create VPC and subnets
2. Attach Internet Gateway
3. Configure route tables
4. Create NAT Gateway
5. Launch Application Load Balancer
6. Launch EC2 instances in private subnets
7. Configure target group and health checks
8. Deploy application code
9. Create RDS database
10. Verify end-to-end connectivity

🧪 Testing

* Access application via ALB DNS name
* Verify EC2 target health in target group
* Validate database connectivity from application tier

---
📈 Future Enhancements

* HTTPS using ACM
* Auto Scaling Group
* CI/CD using GitHub Actions
* Infrastructure as Code (Terraform / CloudFormation)
* Containerization using Docker & EKS

---
🎯 Learning Outcomes

* AWS VPC design
* Public vs Private subnets
* ALB and NAT Gateway usage
* Secure two-tier architecture
* Production-ready AWS deployment

---
👤 Author
Uppara chaithanya
