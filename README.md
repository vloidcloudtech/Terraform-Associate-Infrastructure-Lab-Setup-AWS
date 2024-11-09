Terraform AWS Infrastructure Project
Project Overview
This project is designed to provision a cloud infrastructure on AWS using Terraform, applying best practices for Infrastructure as Code (IaC). It includes a fully managed Virtual Private Cloud (VPC) with both public and private subnets, EC2 instances with SSH access, secure connections, an S3 bucket, and a NAT Gateway for private network resources. The configuration is suitable for a development or testing environment, showcasing Terraform's modularity and reusability.

Architecture and Components
1. Virtual Private Cloud (VPC)
CIDR Block: 10.0.0.0/16
Subnets:
Public Subnets: Three public subnets, allowing resources within them to have internet access.
Private Subnets: Three private subnets for resources that require isolation from the internet.
2. Subnet Configuration
Public Subnets:
Named dynamically and mapped to specific availability zones.
Enable automatic public IP assignment for internet-facing resources.
Private Subnets:
No public IP assigned, isolated for security.
Routes traffic through the NAT Gateway to allow outbound internet access.
3. Security Groups
SSH Access (ingress-ssh):
Allows SSH connections on port 22 from any IP.
Web Traffic (vpc-web):
Allows HTTP (port 80) and HTTPS (port 443) access for web applications.
Ping/ICMP Access (vpc-ping):
ICMP enabled to allow ping requests.
4. NAT Gateway
Configured in the public subnet, allowing private subnet instances to initiate outbound traffic while remaining inaccessible from the internet.
5. S3 Bucket
Bucket Name: Dynamically generated.
Ownership Controls: Configured for "BucketOwnerPreferred" access, setting ownership at the bucket level for consistent access control.
6. EC2 Instances
AMI: Latest Ubuntu 22.04 LTS (AMI dynamically retrieved).
Instance Type: t3.micro, suitable for testing under the AWS Free Tier.
Provisioning:
Key Pair: Generated using TLS provider; private key saved locally.
Local and Remote Provisioners:
Executes scripts upon instance launch to install necessary software.
Configures a demo web application by cloning a GitHub repository and running setup scripts.
7. Auto Scaling Group
Configuration:
Uses Terraform AWS module for auto-scaling.
Min, max, and desired instance size are configurable.
Tags:
Tags include the instance name and application details for easy identification.
File Descriptions
File	Description
main.tf	Core configuration file, defines VPC, subnets, EC2, security groups, S3, and NAT gateway.
variables.tf	Parameterized variables for customizable configurations, such as region, VPC CIDR, and zones.
outputs.tf	Specifies outputs like public URLs, VPC IDs, and instance details for post-deployment access.
terraform.tf	Specifies provider versions, ensuring compatibility and consistent deployments.
.terraform.lock.hcl	Provider dependency lock file, maintains version control over dependencies.
terraform.tfstate	State file that tracks resources managed by Terraform.
terraform.tfstate.backup	Backup state file for disaster recovery or rollback scenarios.
Requirements
Terraform: Version 1.0.0 or higher.
AWS CLI: For AWS configuration and credentials management.
AWS Account: Permissions to create resources such as VPC, EC2, S3, and NAT gateways.
Setup Instructions
Step 1: Install Terraform
Download and install Terraform based on your OS.

Step 2: Clone the Repository
bash
Copy code
git clone <repository_url>
cd <project_directory>
Step 3: Initialize the Project
Run terraform init to install required providers and set up the Terraform environment.

Step 4: Plan the Infrastructure
bash
Copy code
terraform plan -out=tfplan
This command outputs a summary of the changes Terraform will make, allowing you to review the plan before deployment.

Step 5: Apply the Infrastructure
bash
Copy code
terraform apply tfplan
This command deploys the infrastructure on AWS. Confirm the apply command to create resources.

Step 6: Verify the Outputs
After successful deployment, Terraform will output important information such as:

VPC ID
Public IP of the EC2 Instance
Public URL for web access of the EC2 instance.
Step 7: Access EC2 Instance
SSH into the EC2 instance using the private key generated during provisioning:

bash
Copy code
ssh -i "MyAWSKey.pem" ubuntu@<public_ip>
Variable Definitions
Variable	Type	Description	Default
aws_region	string	AWS region for resource deployment.	us-east-1
vpc_name	string	Name identifier for the VPC.	demo_vpc
vpc_cidr	string	CIDR block for the VPC network.	10.0.0.0/16
private_subnets	map	Map of private subnets and associated identifiers.	{subnet_1, ...}
public_subnets	map	Map of public subnets and associated identifiers.	{subnet_1, ...}
environment	string	Environment label (e.g., dev, prod).	dev
Outputs
Output	Description
hello-world	Prints a simple "Hello World" text output for verification.
vpc_id	Displays the VPC ID for the primary VPC.
public_url	Provides a URL to access the web application on the EC2 instance.
vpc_information	Summary of VPC details, including environment and VPC ID.
public_ip	Public IP address of the main EC2 instance.
public_dns	Public DNS for accessing the EC2 instance.
asg_group_size	Size of the Auto Scaling Group configured for the project.
Provider Details
Provider	Version	Description
hashicorp/aws	~> 3.0	AWS provider, manages all AWS resources (VPC, EC2, S3, NAT Gateway, etc.).
hashicorp/tls	3.1.0	Generates TLS keys for SSH access to the EC2 instance.
hashicorp/http	3.4.5	For HTTP-based data retrieval, if applicable.
hashicorp/random	3.1.0	Generates random resources, such as unique resource names.
hashicorp/local	2.5.2	Manages local operations, such as saving private key files.
Troubleshooting
Error with Terraform Version: Ensure the installed version matches the version specified in terraform.tf.
Permissions Issues: Verify AWS IAM permissions for resource creation.
State File Conflicts: If multiple users are deploying, set up remote state storage (e.g., S3 backend).
License
This project is licensed under the MIT License. You are free to use, modify, and distribute this code under the terms of the MIT License.
