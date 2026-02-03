# AWS Cloud Compute Learning Repository

[![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazonaws)](https://aws.amazon.com/)
[![Terraform](https://img.shields.io/badge/Terraform-IaC-purple?logo=terraform)](https://www.terraform.io/)

## Overview

This repository provides a comprehensive guide to **AWS Cloud Compute services**, covering both manual provisioning through the AWS Console and automated Infrastructure as Code (IaC) using Terraform.

Whether you're new to cloud computing or looking to automate your infrastructure, this learning resource walks you through the fundamentals with hands-on examples.

---

## Repository Structure

```
aws-cloud-compute/
├── AWS_CLOUD.md                           # Core AWS concepts and fundamentals
├── README.md                              # This file
└── compute/
    ├── aws-console/
    │   └── aws-console-compute.md         # Manual EC2 provisioning guide
    ├── terraform/
    │   ├── TERRAFORM_INTRO.md             # Introduction to Terraform
    │   └── COMPUTE_TERRAFORM.md           # EC2 provisioning with Terraform
    └── images/                            # Screenshots for step-by-step guides
        ├── step-1.png → step-6.png
```

---

## Learning Path

| Order | Document | Description |
|:---:|---|---|
| 1 | [AWS Core Concepts](./AWS_CLOUD.md) | Understand AWS fundamentals, regions, and core services |
| 2 | [EC2 via Console](./compute/aws-console/aws-console-compute.md) | Launch an EC2 instance manually with step-by-step screenshots |
| 3 | [Terraform Introduction](./compute/terraform/TERRAFORM_INTRO.md) | Learn Terraform basics and project structure |
| 4 | [EC2 via Terraform](./compute/terraform/COMPUTE_TERRAFORM.md) | Automate EC2 provisioning using Infrastructure as Code |

---

## What You'll Learn

- **AWS Fundamentals**: Regions, Availability Zones, core services
- **EC2 Essentials**: AMIs, instance types, key pairs, security groups
- **Manual Provisioning**: Step-by-step console-based instance creation
- **Infrastructure as Code**: Terraform configuration and workflow
- **Best Practices**: Security, scalability, and automation principles

---

## Getting Started

### Prerequisites

- An [AWS Account](https://aws.amazon.com/free/) (Free Tier eligible)
- Basic understanding of cloud computing concepts
- For Terraform sections:
  - [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) installed and configured
  - [Terraform](https://developer.hashicorp.com/terraform/install) installed

### Quick Start

1. Start with **[AWS Core Concepts](./AWS_CLOUD.md)** to understand the fundamentals
2. Follow the **[EC2 Console Guide](./compute/aws-console/aws-console-compute.md)** to manually launch your first instance
3. Move to **[Terraform Introduction](./compute/terraform/TERRAFORM_INTRO.md)** to learn IaC principles
4. Practice with **[Terraform EC2](./compute/terraform/COMPUTE_TERRAFORM.md)** to automate infrastructure

---

## Contributing

Feel free to submit issues or pull requests to improve this learning resource!

---

## License

This project is for educational purposes. Feel free to use and modify for your learning journey.
