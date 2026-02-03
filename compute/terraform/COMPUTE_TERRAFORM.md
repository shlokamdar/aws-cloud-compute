# EC2 Compute Using Terraform

## Objective

Provision an **Amazon EC2 instance** using Terraform to demonstrate Infrastructure as Code (IaC) principles. This guide walks through creating a complete, production-ready Terraform configuration.

> **Prerequisite**: Complete the [Terraform Introduction](./TERRAFORM_INTRO.md) first to understand the basics.

---

## Prerequisites

Before you begin, ensure you have:

- [ ] **AWS CLI** installed and configured (`aws configure`)
- [ ] **Terraform** installed (v1.0+) – [Download here](https://developer.hashicorp.com/terraform/install)
- [ ] **IAM permissions** for EC2 (AmazonEC2FullAccess or equivalent)

**Verify your setup**:
```bash
# Check AWS CLI
aws --version
aws sts get-caller-identity

# Check Terraform
terraform version
```

---

## Directory Structure

Create a new directory for your Terraform project:

```
terraform-ec2/
├── provider.tf          # AWS provider configuration
├── main.tf              # EC2 instance resource
├── variables.tf         # Variable declarations
├── terraform.tfvars     # Variable values
├── outputs.tf           # Output definitions
└── README.md            # Project documentation
```

---

## Step-by-Step Configuration

### Step 1: Create `provider.tf`

```hcl
# Configure the AWS Provider
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region  = var.aws_region
  profile = "default"
}
```

---

### Step 2: Create `variables.tf`

```hcl
variable "aws_region" {
  description = "AWS region to deploy resources"
  type        = string
  default     = "ap-south-1"
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "instance_name" {
  description = "Name tag for the EC2 instance"
  type        = string
  default     = "terraform-demo-instance"
}

variable "ami_id" {
  description = "AMI ID for the EC2 instance"
  type        = string
  # Amazon Linux 2023 in ap-south-1
  default     = "ami-0e35ddab05955cf57"
}
```

---

### Step 3: Create `terraform.tfvars`

```hcl
# Customize these values for your deployment
aws_region    = "ap-south-1"
instance_type = "t2.micro"
instance_name = "my-terraform-ec2"
```

---

### Step 4: Create `main.tf`

```hcl
# Data source to get latest Amazon Linux 2023 AMI (alternative to hardcoded AMI)
data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["al2023-ami-*-x86_64"]
  }
}

# Create a Security Group for SSH access
resource "aws_security_group" "ec2_sg" {
  name        = "${var.instance_name}-sg"
  description = "Security group for EC2 instance"

  # SSH access
  ingress {
    description = "SSH from anywhere"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]  # Restrict in production!
  }

  # Outbound internet access
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "${var.instance_name}-sg"
  }
}

# Create the EC2 Instance
resource "aws_instance" "web_server" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = var.instance_type
  
  vpc_security_group_ids = [aws_security_group.ec2_sg.id]
  
  # Enable public IP
  associate_public_ip_address = true

  tags = {
    Name        = var.instance_name
    Environment = "development"
    ManagedBy   = "terraform"
  }
}
```

---

### Step 5: Create `outputs.tf`

```hcl
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.web_server.id
}

output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.web_server.public_ip
}

output "instance_public_dns" {
  description = "Public DNS name of the EC2 instance"
  value       = aws_instance.web_server.public_dns
}

output "security_group_id" {
  description = "ID of the security group"
  value       = aws_security_group.ec2_sg.id
}

output "ssh_command" {
  description = "SSH command to connect to the instance"
  value       = "ssh -i <your-key.pem> ec2-user@${aws_instance.web_server.public_ip}"
}
```

---

## Terraform Workflow

Execute these commands in order:

### 1. Initialize

```bash
terraform init
```

**Expected Output**:
```
Initializing provider plugins...
- Finding hashicorp/aws versions matching "~> 5.0"...
- Installing hashicorp/aws v5.xx.x...

Terraform has been successfully initialized!
```

---

### 2. Plan

```bash
terraform plan
```

**Expected Output Summary**:
```
Plan: 2 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + instance_id        = (known after apply)
  + instance_public_ip = (known after apply)
  ...
```

> **Tip**: Always review the plan before applying!

---

### 3. Apply

```bash
terraform apply
```

When prompted, type `yes` to confirm:
```
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes
```

**Successful Output**:
```
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

instance_id = "i-0abc123def456789"
instance_public_ip = "52.66.xxx.xxx"
ssh_command = "ssh -i <your-key.pem> ec2-user@52.66.xxx.xxx"
```

---

### 4. Destroy (Cleanup)

When you're done, remove all resources:

```bash
terraform destroy
```

---

## Benefits of Terraform

| Benefit | Description |
|---------|-------------|
| **Repeatable** | Run the same code anytime to get identical infrastructure |
| **Version Controlled** | Store in Git, track changes, review via pull requests |
| **Reduced Human Error** | No manual clicking means fewer mistakes |
| **Scalable** | Deploy hundreds of resources with the same effort |
| **Self-Documenting** | The code IS the documentation |
| **Easy Rollback** | Revert to previous versions via Git |

---

## Common Troubleshooting

| Issue | Solution |
|-------|----------|
| `Error: No valid credential sources` | Run `aws configure` to set up credentials |
| `Error: creating EC2 Instance: UnauthorizedOperation` | Check IAM permissions |
| `Error: Invalid AMI ID` | AMI IDs are region-specific; update for your region |

---

## Conclusion

Using Terraform for EC2 provisioning provides:

- **Consistency** – Same code = same infrastructure
- **Automation** – No manual console work
- **Collaboration** – Teams can review and contribute
- **Scalability** – From 1 to 1000s of instances

This approach is **suitable for production workloads** and is the industry standard for cloud infrastructure management.

---

## What's Next?

Extend your Terraform knowledge:

- Add an **Elastic IP** for a static public IP
- Create an **Auto Scaling Group** for high availability
- Set up a **Load Balancer** for traffic distribution
- Use **Terraform Cloud** for remote state and team collaboration

---

## Additional Resources

- [Terraform AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Terraform Best Practices](https://developer.hashicorp.com/terraform/language)
- [AWS Free Tier](https://aws.amazon.com/free/)