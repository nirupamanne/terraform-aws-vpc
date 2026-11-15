# Terraform AWS VPC Module

## Introduction

This repository contains Terraform code to create AWS VPC networking infrastructure.

I created this project to understand how to automate AWS networking resources using Terraform.

Instead of creating each resource manually from the AWS Console, Terraform helps us create and manage the infrastructure using code.

## Architecture Overview

This Terraform module creates the following resources:

1. **VPC** – Creates a Virtual Private Cloud in AWS.
2. **Internet Gateway (IGW)** – Allows internet connectivity for resources in the public subnet.
3. **Public Subnets** – Creates two public subnets across two Availability Zones.
4. **Private Subnets** – Creates two private subnets across two Availability Zones.
5. **Database Subnets** – Creates two database subnets across two Availability Zones.
6. **DB Subnet Group** – Groups the database subnets.
7. **Route Tables** – Creates separate route tables for public, private, and database subnets.
8. **Elastic IP** – Allocates a public IP address for the NAT Gateway.
9. **NAT Gateway** – Allows resources in private and database subnets to access the internet.

## Project Structure

```text
terraform-aws-vpc/
│
├── data.tf
├── locals.tf
├── outputs.tf
├── variable.tf
├── vpc.tf
└── README.md
```

## Network Configuration

| Resource | CIDR / Configuration |
|---|---|
| VPC | 10.0.0.0/16 |
| Public Subnet 1 | 10.0.1.0/24 |
| Public Subnet 2 | 10.0.2.0/24 |
| Private Subnet 1 | 10.0.11.0/24 |
| Private Subnet 2 | 10.0.12.0/24 |
| Database Subnet 1 | 10.0.21.0/24 |
| Database Subnet 2 | 10.0.22.0/24 |

The subnets are distributed across the first two available Availability Zones in the selected AWS region.

## Terraform Files Explanation

| File | Purpose |
|---|---|
| `data.tf` | Fetches available AWS Availability Zones. |
| `locals.tf` | Defines common names, tags, and Availability Zone information. |
| `variable.tf` | Defines mandatory and optional input variables. |
| `vpc.tf` | Creates the VPC and other AWS networking resources. |
| `outputs.tf` | Displays the VPC ID and Availability Zone information. |

## Prerequisites

Before running this project, make sure you have:

- An AWS account.
- AWS CLI installed and configured.
- Terraform installed.
- AWS credentials configured with the required permissions.
- Git installed (optional, for cloning the repository).

## How to Use This Module

### Step 1: Clone the Repository

```bash
git clone https://github.com/nirupamanne/terraform-aws-vpc.git
```

### Step 2: Create a Root Terraform Configuration

Create a separate working directory, for example, `vpc-test`.

Inside `main.tf`, call this module:

```hcl
provider "aws" {
  region = "us-east-1"
}

module "vpc" {
  source = "../terraform-aws-vpc"

  project     = "roboshop"
  environment = "dev"
}
```

Update the module source path if your folder structure is different.

### Step 3: Initialize Terraform

```bash
terraform init
```

This downloads the required Terraform providers and prepares the working directory.

### Step 4: Validate the Configuration

```bash
terraform validate
```

This checks whether the Terraform configuration is valid.

### Step 5: Review the Execution Plan

```bash
terraform plan
```

This shows which AWS resources Terraform is planning to create.

### Step 6: Create the Infrastructure

```bash
terraform apply
```

Type `yes` when Terraform asks for confirmation.

You can also use:

```bash
terraform apply -auto-approve
```

This applies the changes without asking for confirmation.

### Step 7: View Terraform Outputs

```bash
terraform output
```

To display only the VPC ID:

```bash
terraform output vpc_id
```

## Input Variables

### Mandatory Variables

| Variable | Description |
|---|---|
| `project` | Project name used for resource naming and tagging. |
| `environment` | Environment name, such as dev or prod. |

### Optional Variables

| Variable | Default Value |
|---|---|
| `vpc_cidr` | `10.0.0.0/16` |
| `public_subnet_cidrs` | `10.0.1.0/24`, `10.0.2.0/24` |
| `private_subnet_cidrs` | `10.0.11.0/24`, `10.0.12.0/24` |
| `database_subnet_cidrs` | `10.0.21.0/24`, `10.0.22.0/24` |

Resource-specific tag variables are also available in `variable.tf`.

## Outputs

| Output | Description |
|---|---|
| `vpc_id` | Displays the ID of the created VPC. |
| `azs_info` | Displays the available AWS Availability Zones. |

## Cleanup – Delete AWS Resources

After completing the practice, remove the resources to avoid unnecessary AWS charges.

Run the following command from the root Terraform working directory:

```bash
terraform destroy -auto-approve
---
---
## Outputs:

azs_info = tolist([
  "us-east-1a",
  "us-east-1b",
])
vpc_id = "vpc-01c1e84241be455aa"

nirup@LAPTOP-3HA2P95B UCRT64 /c/Nirupa/JoinDevOps/repos/vpc-test (main)

*Reference Output*
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4bcb531d-402d-44c6-89ee-5ed61275618a" />
