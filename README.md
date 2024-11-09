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
Initialize the Project:

bash
Copy code
terraform init
Plan the Infrastructure:

bash
Copy code
terraform plan -out=tfplan
Review the planned actions before deploying.

Apply the Infrastructure:

bash
Copy code
terraform apply tfplan
This command provisions the infrastructure on AWS. Confirm to proceed with deployment.

Access Outputs:

View details like EC2 instance public IP, VPC ID, and web server URL.
SSH Access:

SSH into the EC2 instance using the generated private key:
bash
Copy code
ssh -i "MyAWSKey.pem" ubuntu@<public_ip>
Variable Definitions
Variable	Type	Description	Default
aws_region	string	AWS region for resource deployment	us-east-1
vpc_name	string	Name identifier for the VPC	demo_vpc
vpc_cidr	string	CIDR block for the VPC	10.0.0.0/16
private_subnets	map	Map of private subnets	{subnet_1, ...}
public_subnets	map	Map of public subnets	{subnet_1, ...}
environment	string	Environment label (e.g., dev)	dev
Outputs
Output	Description
hello-world	Simple "Hello World" text for verification
vpc_id	VPC ID for the primary VPC
public_url	URL for accessing the EC2 web server
vpc_information	Summary of VPC environment details
public_ip	Public IP of the EC2 instance
public_dns	Public DNS for EC2 instance
asg_group_size	Auto Scaling Group size
Provider Details
Provider	Version	Description
hashicorp/aws	~> 3.0	AWS provider for all AWS resources (VPC, EC2, S3, etc.).
hashicorp/tls	3.1.0	Generates TLS keys for SSH access.
hashicorp/http	3.4.5	Handles HTTP-based data retrieval, if used.
hashicorp/random	3.1.0	Creates random values for resource names, etc.
hashicorp/local	2.5.2	Manages local operations, like saving files.
Troubleshooting
Terraform Version Errors: Ensure Terraform version meets the requirements in terraform.tf.
Permission Errors: Verify that AWS IAM permissions allow resource creation.
State File Conflicts: For collaborative projects, consider storing state in a remote backend (e.g., S3).
License
This project is licensed under the MIT License. You are free to use, modify, and distribute this code under the terms of the MIT License.


### Explanation of Adjustments
- **Indented Lists & Bullet Points**: Ensures all list items are properly aligned and indented for clarity.
- **Code Blocks**: Each command block is within triple backticks (\```bash\`) to ensure it displays correctly.
- **Table Formatting**: Tables for `File Descriptions`, `Variable Definitions`, and `Outputs` are carefully aligned to enhance readability.
- **Clear Section Breaks**: Extra blank lines between sections for separation.

Copy this into your `README.md` file, and it should now format correctly on GitHub or any Markdown editor. Let me know if it works!
