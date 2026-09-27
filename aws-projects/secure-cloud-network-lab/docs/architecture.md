# Secure AWS Cloud Network and Security Validation Lab

## 1. Project Purpose

This project demonstrates the complete lifecycle of designing, securing, deploying, monitoring, testing, and documenting a multi-tier AWS network.

Phase 1 focuses on cloud engineering and infrastructure deployment. Phase 2 uses the same environment for controlled cloud-security validation, investigation, and remediation.

The environment will first be built manually to develop an understanding of each AWS component. It will then be recreated with Terraform and validated through Python/Boto3 automation.

## 2. AWS Environment

- Region: `us-east-1` — US East (N. Virginia)
- Availability Zones: Two
- VPC CIDR: `10.0.0.0/16`
- Monthly budget: `$25`
- Deployment approach: Build in stages and remove billable resources after testing
- Administration approach: Use an IAM administrator identity instead of the root user

## 3. Network Design

| Availability Zone | Subnet type | CIDR block | Intended resources |
|---|---|---:|---|
| AZ A | Public | `10.0.1.0/24` | Application Load Balancer and temporary NAT Gateway |
| AZ B | Public | `10.0.2.0/24` | Application Load Balancer |
| AZ A | Private application | `10.0.11.0/24` | EC2 application server |
| AZ B | Private application | `10.0.12.0/24` | EC2 application server |
| AZ A | Private database | `10.0.21.0/24` | Database |
| AZ B | Private database | `10.0.22.0/24` | Database standby or subnet-group coverage |

The six subnets are distributed across two Availability Zones to demonstrate availability, network segmentation, and multi-tier cloud architecture.

## 4. Architecture Tiers

### Web Tier

The public Application Load Balancer receives approved web traffic from the internet. It is deployed across the two public subnets.

### Application Tier

EC2 instances run the application inside private application subnets. These instances do not receive public IPv4 addresses and accept application traffic only from the load balancer.

### Database Tier

The database is placed inside the private database subnets. It accepts database connections only from the application tier and is not directly accessible from the internet.

## 5. Traffic Flow

1. A user sends a web request from the internet.
2. The Internet Gateway provides an internet path into the VPC.
3. The public Application Load Balancer receives the request.
4. The load balancer forwards the request to an EC2 instance in a private application subnet.
5. The application connects to the database in a private database subnet.
6. Security groups restrict communication between the web, application, and database tiers.
7. Response traffic returns through the established connection.

The intended application flow is:

`Internet → Application Load Balancer → EC2 application → Database`

## 6. Routing Design

### Public Subnets

The public subnets use a public route table containing:

- A local route for communication within the VPC
- A default route of `0.0.0.0/0` to the Internet Gateway

### Private Application Subnets

The private application subnets:

- Do not assign public IPv4 addresses to EC2 instances
- Use local VPC routes for internal communication
- May temporarily use a NAT Gateway for outbound internet access
- Do not route internet traffic directly to the Internet Gateway

If a NAT Gateway is required, the private route table will contain:

`0.0.0.0/0 → NAT Gateway`

The NAT Gateway will be located in a public subnet and use its own Elastic IP address.

### Private Database Subnets

The private database subnets:

- Use local routes for internal VPC communication
- Do not have a direct route to the Internet Gateway
- Do not provide direct public access to the database
- Will be associated with a database subnet group when required

## 7. Security Design

### Network Security

- The Application Load Balancer accepts only required web traffic.
- Application instances accept traffic only from the load balancer security group.
- The database accepts traffic only from the application security group.
- Private EC2 instances will not receive public IPv4 addresses.
- Security groups will provide stateful resource-level traffic control.
- Network ACL behavior may be examined as part of the security-validation phase.

### Administrative Access

Administrative access should use AWS Systems Manager Session Manager when possible instead of public SSH access.

Session Manager provides terminal access without requiring:

- A public IP address on the EC2 instance
- An inbound security-group rule for SSH port `22`
- Direct management of SSH private keys
- A publicly exposed bastion host

The EC2 instance will require:

- SSM Agent
- An appropriate IAM instance role
- Outbound HTTPS connectivity to Systems Manager through a temporary NAT Gateway or VPC endpoints

### Identity and Access Management

- The AWS root user will not be used for routine administration.
- IAM permissions will follow the principle of least privilege.
- EC2 instances will use IAM roles instead of stored access keys.
- Administrative and application permissions will be separated.
- Access decisions and denied actions will be documented.

### Logging and Monitoring

Relevant services may include:

- Amazon CloudWatch
- AWS CloudTrail
- VPC Flow Logs
- Amazon GuardDuty
- AWS Config or Security Hub when appropriate

Security and monitoring services will be enabled selectively to control costs.

## 8. Implementation Stages

### Stage 1 — Network Foundation

- Create the VPC
- Create six subnets across two Availability Zones
- Create and attach the Internet Gateway
- Create public and private route tables
- Associate each subnet with the correct route table
- Apply consistent resource tags
- Verify the network configuration

### Stage 2 — Security and Workloads

- Create tier-specific security groups
- Create the required IAM roles
- Configure Systems Manager access
- Deploy the Application Load Balancer
- Deploy private EC2 application instances
- Deploy the database layer
- Add temporary outbound connectivity when required
- Verify workload communication

### Stage 3 — Monitoring and Operational Testing

- Configure CloudWatch monitoring
- Test application connectivity
- Test permitted and denied network paths
- Validate route-table behavior
- Validate security-group rules
- Troubleshoot intentional or accidental configuration failures
- Capture evidence screenshots
- Document findings in the build log

### Stage 4 — Infrastructure as Code

- Recreate the architecture with Terraform
- Separate resources into understandable Terraform files
- Define variables and outputs
- Run `terraform fmt`
- Run `terraform validate`
- Review the `terraform plan` output
- Deploy the reproduced environment
- Compare the Terraform deployment with the manual deployment
- Store Terraform code in the project repository

### Stage 5 — Python/Boto3 Automation

- Inventory deployed AWS resources
- Validate resource names, tags, and configuration
- Check network and security settings
- Identify unexpected resources
- Record automation results
- Handle errors and create useful logs

### Stage 6 — Cloud Security Validation

- Establish the expected security baseline
- Enable and review relevant logging services
- Review CloudTrail activity
- Analyze VPC Flow Logs
- Review GuardDuty sample findings or controlled events
- Test permitted and denied network paths
- Validate security-group segmentation
- Test IAM least-privilege controls
- Generate safe and controlled security events
- Investigate relevant logs and findings
- Identify the affected resource and cause
- Remediate identified weaknesses
- Verify that remediation worked
- Record results in `docs/security-lab.md`

Security testing will be performed only against resources owned by this AWS account. The project will not intentionally expose vulnerable resources to the public internet.

### Stage 7 — Teardown

- Destroy temporary Terraform-managed resources
- Remove manually created resources
- Release Elastic IP addresses
- Delete temporary NAT Gateways and load balancers
- Confirm that no unexpected EC2 or database resources remain
- Review final AWS costs
- Document lessons learned

## 9. Cost Controls

- Maintain the `$25` monthly AWS budget.
- Use email alerts at the configured budget thresholds.
- Avoid deploying billable components until they are required.
- NAT Gateway, Application Load Balancer, EC2, database, VPC endpoints, and selected security services may create charges.
- Keep billable resources active only while they are being tested.
- Delete temporary resources immediately after testing.
- Check AWS Billing and Cost Management throughout the project.
- Confirm that Elastic IP addresses and other billable resources are released during teardown.

## 10. Documentation Plan

Project documentation will include:

- `README.md` — Project overview and final results
- `docs/architecture.md` — Architecture and implementation plan
- `docs/build-log.md` — Chronological record of work performed
- `docs/security-lab.md` — Security tests, findings, investigation, and remediation
- `docs/lessons-learned.md` — Final technical reflections and improvements

Evidence will be stored in organized screenshot directories:

- `screenshots/infrastructure/`
- `screenshots/security/`

Screenshots will be reviewed for account numbers, personal information, credentials, and other sensitive information before being committed to GitHub.

## 11. Architecture Diagram

The editable architecture diagram and its exported image will be stored as:

- `diagrams/aws-network-architecture.drawio`
- `diagrams/aws-network-architecture.png`

The diagram will show:

- AWS Region and VPC boundaries
- Two Availability Zones
- Six subnets
- Internet Gateway
- Application Load Balancer
- NAT Gateway when used
- Private EC2 application instances
- Private database tier
- Route and traffic direction
- Security boundaries
- Monitoring and logging services

## 12. Expected Learning Outcomes

After completing this project, I should be able to:

- Explain the purpose of each AWS network component
- Design a multi-tier VPC using appropriate CIDR ranges
- Distinguish public and private subnet behavior
- Configure and troubleshoot AWS routing
- Apply security groups using least-privilege principles
- Securely administer private EC2 instances
- Deploy infrastructure manually and with Terraform
- Use Python/Boto3 to validate AWS resources
- Interpret cloud logs and security findings
- Investigate and remediate controlled security issues
- Verify that security changes work as intended
- Safely tear down AWS resources and confirm cost control