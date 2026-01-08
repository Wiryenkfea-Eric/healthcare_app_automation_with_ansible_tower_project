# Ansible-Tower-Automation-Project
This Project Highlights How FreeRange Took Advantage of Ansible Tower's Enterprise Functionalities, To Automate and Orchestrate The Creation of Different Client Development Environments.

## [SDD] FreeRange Client Dev Environment Automation Architecture
![VenturaSDD!](https://lucid.app/publicSegments/view/f1470d75-725f-4074-a3a6-17f51cedc346/image.png)

## Project Overview

This Ansible automation project provisions AWS EC2 instances and deploys a healthcare web application (MedLife Health Care) using infrastructure as code principles. The project leverages Ansible Tower's enterprise features for secure, scalable, and repeatable deployments across different client environments.

## Architecture

The automation creates a complete AWS infrastructure stack including:
- EC2 instances with customized configurations
- Encrypted EBS volumes
- VPC networking with proper subnet placement
- Security groups for access control
- IAM instance roles for AWS service integration
- Apache web server with healthcare application deployment

## Features

- **Enterprise Automation**: Utilizes Ansible Tower for centralized automation management
- **Secure Configuration**: Integrates with AWS Systems Manager Parameter Store for sensitive data
- **Dynamic Infrastructure**: Parameterized playbooks for multi-environment deployments
- **Encrypted Storage**: Automatic EBS volume encryption for data security
- **Automated Application Deployment**: Complete web server setup and application installation
- **Resource Tagging**: Comprehensive tagging strategy for cost tracking and resource management

## Prerequisites

- Ansible Tower installed and configured
- AWS Account with appropriate permissions
- AWS CLI configured with credentials
- Python 3.x with boto3 library
- Required Ansible collections:
  - `amazon.aws`
  - `community.aws`

## AWS Systems Manager Parameters

The following parameters must be configured in AWS SSM Parameter Store under the `/JJTech/<team>/` namespace:

- `keyname` - EC2 key pair name
- `security_group` - Security group ID
- `image_id` - AMI ID for the instance
- `vpc_subnet_id` - Subnet ID for instance placement
- `instance_role` - IAM role for the instance

## Variables

Configure the following variables in Ansible Tower or your inventory:

| Variable | Description | Example |
|----------|-------------|---------|
| `deployment_region` | AWS region for deployment | `us-east-1` |
| `team` | Team identifier for SSM parameters | `dev-team` |
| `instance_type` | EC2 instance type | `t2.micro` |
| `resource_state` | Instance state (present/absent) | `present` |
| `root_volume_size` | Root EBS volume size in GB | `20` |
| `instance_name` | Name tag for the instance | `healthcare-web-01` |
| `created_by` | Creator identifier | `ansible-tower` |
| `Owner` | Resource owner | `DevOps Team` |
| `App_Name` | Application name | `MedLife Healthcare` |
| `Cost_Center` | Cost center code | `CC-1001` |
| `Business_Unit` | Business unit name | `Healthcare` |

## Project Structure
```
.
├── README.md
├── ec2-automation-playbook.yaml       # Main EC2 automation playbook
├── ec2-automation-project.yaml        # Project-specific EC2 configuration
├── ec2-automation-test.yaml           # Testing playbook
├── dynamic-version.yaml               # Dynamic versioning configuration
├── rds-automation-project.yaml        # RDS database automation
├── rds-automation-with-sm.yaml        # RDS with Secrets Manager
├── s3-create-bucket.yml               # S3 bucket creation
└── tower-installation-configs/        # Ansible Tower installation files
    └── ansible-tower-install.sh
```

## Usage

### 1. Configure AWS SSM Parameters
```bash
aws ssm put-parameter \
  --name "/JJTech/<team>/keyname" \
  --value "your-key-pair" \
  --type "String" \
  --region us-east-1
```

### 2. Run the Playbook

**From Ansible Tower:**
- Create a new Project pointing to this repository
- Create a Job Template using `ec2-automation-playbook.yaml`
- Configure the required variables
- Launch the job

**From Command Line:**
```bash
ansible-playbook ec2-automation-playbook.yaml \
  -e "deployment_region=us-east-1" \
  -e "team=dev-team" \
  -e "instance_type=t2.micro" \
  -e "resource_state=present"
```

### 3. Access the Application

After successful deployment, the healthcare application will be available at:
```
http://<instance-public-ip>/
```

## Application Details

The playbook automatically deploys the MedLife Healthcare web application, which includes:
- Apache HTTP server configuration
- Application files from GitHub repository
- Automatic service startup and enable on boot
- Health check endpoints

## Security Considerations

- All EBS volumes are encrypted by default
- Sensitive data stored in AWS Systems Manager Parameter Store
- Security groups control network access
- IAM roles follow principle of least privilege
- Regular security updates applied via `yum update -y`

## Troubleshooting

**Issue**: Parameter not found in SSM
```
Solution: Verify parameter exists in correct region with proper path format
```

**Issue**: EC2 instance fails to launch
```
Solution: Check subnet availability, security group rules, and AMI availability in the target region
```

**Issue**: Application not accessible
```
Solution: Verify security group allows HTTP traffic (port 80) and httpd service is running
```

## Cost Management

Resources are tagged for cost tracking purposes. Monitor costs using AWS Cost Explorer with the following tag filters:
- `Cost_Center`
- `Business_Unit`
- `App_Name`
- `Owner`

## Contributing

Contributions are welcome! Please follow these steps:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## Support

For issues and questions:
- Open an issue in the GitHub repository
- Contact the DevOps team
- Refer to Ansible Tower documentation


---

**Note**: This is an enterprise automation project. Ensure proper testing in non-production environments before deploying to production systems.
