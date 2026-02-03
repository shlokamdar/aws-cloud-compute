# Introduction to Terraform

## What is Terraform?

**Terraform** is an open-source *Infrastructure as Code (IaC)* tool developed by **HashiCorp**. It allows you to **define, provision, and manage infrastructure** across multiple cloud providers using a simple, declarative configuration language called **HCL** (HashiCorp Configuration Language).

> **Key Concept**: Instead of clicking through web consoles, you write code that describes your desired infrastructure. Terraform then creates it for you.

---

## Why Use Terraform?

| Benefit | Description |
|---------|-------------|
| **Reproducible** | Same code = same infrastructure, every time |
| **Version Controlled** | Track changes with Git, review, and roll back |
| **Multi-Cloud** | Works with AWS, Azure, GCP, and 1000+ providers |
| **Collaborative** | Teams can work together on infrastructure |
| **Automated** | No manual clicking, reduces human error |

---

## Terraform vs Manual Provisioning

| Aspect | Manual (Console) | Terraform |
|--------|-----------------|-----------|
| Speed | Minutes per resource | Seconds per resource |
| Consistency | Varies each time | Identical every time |
| Documentation | Separate docs needed | Code IS the documentation |
| Scalability | Doesn't scale | 1 to 1000s of resources |
| Error Recovery | Manual fixes | terraform apply again |

---

## Typical Terraform Project Structure

A Terraform project consists of multiple `.tf` files organized in a directory:

```
my-ec2-project/
├── provider.tf        # Cloud provider configuration
├── main.tf            # Core resource definitions
├── variables.tf       # Input variable declarations
├── terraform.tfvars   # Actual values for variables
├── outputs.tf         # Output value definitions
├── backend.tf         # Remote state configuration (optional)
└── .terraform/        # Generated: provider plugins (do not edit)
```

Each file serves a specific purpose, making your code **organized and maintainable**.

---

## Understanding Each File

### 1. provider.tf – Cloud Provider Configuration

Defines **which cloud** Terraform should connect to and **how to authenticate**.

```hcl
# AWS Provider Configuration
provider "aws" {
  region  = "ap-south-1"    # Mumbai region
  profile = "default"        # AWS CLI profile to use
}
```

**What this does**:
- Tells Terraform to use the AWS provider
- Sets the default region for resources
- Uses credentials from AWS CLI configuration

---

### 2. main.tf – Resource Definitions

Contains the **actual infrastructure** you want to create. All cloud services are defined as "resources".

```hcl
# Create an EC2 instance
resource "aws_instance" "web_server" {
  ami           = "ami-0abcd1234efgh5678"
  instance_type = "t2.micro"
  
  tags = {
    Name = "MyWebServer"
  }
}
```

**Resource Block Structure**:
| Part | Example | Description |
|------|---------|-------------|
| Resource Type | `aws_instance` | What to create (EC2 instance) |
| Resource Name | `web_server` | Your local identifier |
| Arguments | `ami`, `instance_type` | Configuration options |

---

### 3. variables.tf – Input Variable Declarations

Makes your code **reusable and flexible** by defining input parameters.

```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "environment" {
  description = "Deployment environment"
  type        = string
  # No default = must be provided
}
```

**Variable Properties**:
| Property | Required | Description |
|----------|:--------:|-------------|
| `description` | Recommended | Explains what the variable is for |
| `type` | Recommended | Data type (string, number, bool, list, map) |
| `default` | Optional | Value used if none is provided |

---

### 4. terraform.tfvars – Variable Values

Contains **actual values** for your variables. You can have different files for different environments.

```hcl
# terraform.tfvars - Default values
instance_type = "t3.micro"
environment   = "development"
```

```hcl
# prod.tfvars - Production values
instance_type = "m5.large"
environment   = "production"
```

**Usage with different environments**:
```bash
# For development (uses terraform.tfvars automatically)
terraform apply

# For production
terraform apply -var-file="prod.tfvars"
```

---

### 5. outputs.tf – Output Values

Displays important information **after** Terraform creates your infrastructure.

```hcl
output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.web_server.public_ip
}

output "instance_id" {
  description = "ID of the created EC2 instance"
  value       = aws_instance.web_server.id
}
```

**After running `terraform apply`, you'll see**:
```
Outputs:

instance_public_ip = "52.66.123.45"
instance_id = "i-0abc123def456789"
```

---

### 6. backend.tf – Remote State Storage (Optional)

Stores Terraform's **state file** remotely for team collaboration and security.

```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state-bucket"
    key    = "ec2/terraform.tfstate"
    region = "ap-south-1"
  }
}
```

> **What is State?** Terraform maintains a "state file" that maps your configuration to real infrastructure. This file is critical for tracking what exists.

**Benefits of Remote Backend**:
- Team members share the same state
- State is stored securely (not on local machine)
- Supports state locking to prevent conflicts

---

## Terraform Core Workflow

The four essential commands you'll use:

```bash
# 1. Initialize - Download providers and set up backend
terraform init

# 2. Plan - Preview what will be created/changed/destroyed
terraform plan

# 3. Apply - Create the infrastructure
terraform apply

# 4. Destroy - Remove all resources
terraform destroy
```

**Workflow Visualization**:

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   init      │ → │    plan     │ → │   apply     │ → │  destroy    │
│ (one time)  │    │ (preview)   │    │ (create)    │   │ (cleanup)   │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

---

## Best Practices

| Practice | Description |
|----------|-------------|
| Use meaningful names | `web_server` not `instance1` |
| One environment per directory | Separate `dev/`, `prod/` |
| Never commit secrets | Use variables or AWS Secrets Manager |
| Always run `plan` first | Review changes before applying |
| Tag all resources | For cost tracking and organization |

---

## Next Steps

Ready to create infrastructure with Terraform?

**Continue to: [EC2 Provisioning with Terraform](./COMPUTE_TERRAFORM.md)**