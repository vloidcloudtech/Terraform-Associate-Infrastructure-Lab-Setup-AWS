# Terraform AWS Infrastructure Project

## Project Overview

This project provisions a cloud infrastructure on AWS using Terraform, applying best practices for Infrastructure as Code (IaC). It includes a Virtual Private Cloud (VPC) with public and private subnets, EC2 instances with SSH access, secure connections, an S3 bucket, and a NAT Gateway. This setup is ideal for development or testing environments and showcases Terraform's modularity and reusability.

## Architecture and Components

### 1. Virtual Private Cloud (VPC)
   - **CIDR Block**: `10.0.0.0/16`
   - **Subnets**:
     - **Public Subnets**: Three public subnets that allow internet access.
     - **Private Subnets**: Three private subnets for isolated resources.

### 2. Subnet Configuration
   - **Public Subnets**: 
     - Automatically assigns public IPs, enabling internet access.
   - **Private Subnets**:
     - No public IPs assigned, uses a NAT Gateway for secure internet access.

### 3. Security Groups
   - **SSH Access (ingress-ssh)**: 
     - Allows SSH on port `22` from any IP.
   - **Web Traffic (vpc-web)**:
     - Allows HTTP (port `80`) and HTTPS (port `443`) access.
   - **Ping/ICMP Access (vpc-ping)**:
     - Allows ICMP traffic for ping requests.

### 4. NAT Gateway
   - Configured in the public subnet, enabling private subnet instances to initiate outbound traffic while remaining inaccessible from the internet.

### 5. S3 Bucket
   - **Ownership Control**: Configured for "BucketOwnerPreferred" access for consistent access control.

### 6. EC2 Instances
   - **AMI**: Latest Ubuntu 22.04 LTS.
   - **Instance Type**: `t3.micro` (AWS Free Tier eligible).
   - **Provisioning**:
     - Uses a TLS-generated SSH key, with the private key saved locally.
     - Automatically installs necessary software and configures a demo web application.

### 7. Auto Scaling Group
   - **Configuration**:
     - Uses Terraform AWS module for auto-scaling with configurable instance limits.
   - **Tags**:
     - Tags include instance name and application details for easy identification.

## File Descriptions

| File                   | Description                                                                                |
|------------------------|--------------------------------------------------------------------------------------------|
| `main.tf`              | Core configuration: VPC, subnets, EC2, security groups, S3, and NAT Gateway.               |
| `variables.tf`         | Customizable parameters (e.g., region, VPC CIDR, availability zones).                      |
| `outputs.tf`           | Specifies outputs like VPC ID, EC2 public IP, and public URL.                              |
| `terraform.tf`         | Provider version configurations for consistent deployments.                                |
| `.terraform.lock.hcl`  | Lock file for provider dependencies.                                                       |
| `terraform.tfstate`    | Tracks resources managed by Terraform.                                                     |
| `terraform.tfstate.backup` | Backup state file for disaster recovery.                                              |

## Requirements

- **Terraform**: Version >= 1.0.0
- **AWS CLI**: For configuration and credentials management
- **AWS Account**: Permissions for VPC, EC2, S3, and NAT Gateway creation

## Setup Instructions

1. **Install Terraform**:
   - Download [Terraform](https://www.terraform.io/downloads.html) and install it.

2. **Clone the Repository**:
   ```bash
   git clone <repository_url>
   cd <project_directory>
