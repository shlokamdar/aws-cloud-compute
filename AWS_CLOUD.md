# AWS Cloud – Core Concepts

## Introduction

**Amazon Web Services (AWS)** is the world's leading cloud computing platform, providing on-demand access to computing resources over the internet. With AWS, organizations can build and scale applications without the burden of managing physical infrastructure.

> **Key Benefit**: Pay only for what you use, scale instantly, and focus on your application instead of hardware.

---

## AWS Global Infrastructure

AWS operates a massive global infrastructure designed for **high availability**, **fault tolerance**, and **low latency**.

### Regions

A **Region** is a geographically isolated area containing multiple Availability Zones.

| Region Code | Location |
|-------------|----------|
| `us-east-1` | N. Virginia, USA |
| `eu-west-1` | Ireland |
| `ap-south-1` | Mumbai, India |

> **Tip**: Choose a region close to your users for lower latency and consider data residency requirements.

### Availability Zones (AZs)

- Isolated data centers within a region
- Connected via low-latency links
- Used for fault tolerance and high availability
- Each region has **2-6 Availability Zones**

### Edge Locations

- Used by **AWS CloudFront** (CDN service)
- Cache content closer to end users
- Reduce latency for global applications
- Over **400+ edge locations** worldwide

---

## Core AWS Service Categories

AWS offers 200+ services. Here are the essential categories:

### Compute

| Service | Description |
|---------|-------------|
| **Amazon EC2** | Virtual servers in the cloud (the focus of this guide) |
| **AWS Lambda** | Serverless compute – run code without managing servers |
| **Amazon ECS/EKS** | Container orchestration services |

### Storage

| Service | Description |
|---------|-------------|
| **Amazon S3** | Object storage for any type of data |
| **Amazon EBS** | Block storage volumes for EC2 instances |
| **Amazon EFS** | Scalable file storage for Linux workloads |

### Databases

| Service | Description |
|---------|-------------|
| **Amazon RDS** | Managed relational databases (MySQL, PostgreSQL, etc.) |
| **Amazon DynamoDB** | Fully managed NoSQL database |
| **Amazon Aurora** | MySQL/PostgreSQL-compatible with enhanced performance |

### Networking

| Component | Description |
|-----------|-------------|
| **Amazon VPC** | Isolated virtual network for your AWS resources |
| **Subnets** | Subdivisions within a VPC (public/private) |
| **Route Tables** | Control traffic routing between subnets |
| **Internet Gateway** | Enables internet access for VPC resources |

### Security

| Service | Description |
|---------|-------------|
| **AWS IAM** | Identity and Access Management |
| **Security Groups** | Virtual firewalls for EC2 instances |
| **Network ACLs** | Subnet-level traffic filtering |

---

## Identity and Access Management (IAM)

IAM controls **who** can access **what** in your AWS account.

### Core Components

| Component | Purpose | Example |
|-----------|---------|---------|
| **Users** | Human identities | Developers, admins |
| **Groups** | Collections of users | "Developers" group |
| **Roles** | Service-based access | EC2 role for S3 access |
| **Policies** | Permission definitions (JSON) | Allow/Deny specific actions |

### Principle of Least Privilege

> Always grant **only the minimum permissions** required to perform a task. Never use root account for daily operations.

---

## Amazon EC2 (Elastic Compute Cloud)

EC2 is the **backbone of AWS compute**. It provides resizable virtual servers in the cloud.

### What is EC2?

- **Virtual servers** (called "instances") running in AWS data centers
- Full control over OS, software, and configuration
- Pay by the second (on-demand) or save with reserved instances

### Key EC2 Components

| Component | Description |
|-----------|-------------|
| **AMI** | Amazon Machine Image – the template with OS and software |
| **Instance Type** | Hardware configuration (vCPU, RAM, storage) |
| **Key Pair** | SSH keys for secure access |
| **Security Group** | Firewall rules controlling inbound/outbound traffic |
| **EBS Volume** | Persistent block storage attached to instances |

### Common Instance Types

| Type | Use Case |
|------|----------|
| `t2.micro` | Free tier, testing (1 vCPU, 1GB RAM) |
| `t3.medium` | Small applications (2 vCPU, 4GB RAM) |
| `m5.large` | General purpose workloads |
| `c5.xlarge` | Compute-intensive applications |

---

## Infrastructure Provisioning Models

There are two primary ways to create AWS infrastructure:

### Manual Provisioning (Console-based)

| Aspect | Status |
|--------|--------|
| Speed | Slow for large environments |
| Consistency | Prone to human error and drift |
| Version Control | Not trackable |
| Scalability | Not suitable for production |

> **When to use**: Learning, quick experiments, one-off tasks

### Infrastructure as Code (IaC)

| Aspect | Status |
|--------|--------|
| Speed | Fast, automated deployments |
| Consistency | Exactly reproducible |
| Version Control | Git-based change tracking |
| Scalability | From 1 to 1000s of resources |

> **When to use**: Production environments, team collaboration, CI/CD pipelines

**Popular IaC Tools**:
- **Terraform** (Multi-cloud, open-source) – *Covered in this guide*
- **AWS CloudFormation** (AWS-native)
- **Pulumi** (Programming languages-based)

---

## Conclusion

AWS provides a robust and flexible cloud platform for modern applications. Key takeaways:

1. **Choose the right region** for your users and compliance needs
2. **Understand core services** – EC2, S3, VPC, and IAM are foundational
3. **Use Infrastructure as Code** for consistent, scalable, and auditable deployments
4. **Follow the principle of least privilege** for all IAM configurations

---

## Next Steps

Continue your learning journey:

1. **[EC2 via AWS Console](./compute/aws-console/aws-console-compute.md)** – Launch your first instance manually
2. **[Terraform Introduction](./compute/terraform/TERRAFORM_INTRO.md)** – Learn IaC fundamentals
