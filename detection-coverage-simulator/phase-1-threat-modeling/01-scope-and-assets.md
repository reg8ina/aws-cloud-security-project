# Phase 1.1 – Scope & Assets

## Environment Scope
- Environment type: Cloud-based lab environment
- Cloud provider / platform: AWS
- High-level architecture summary:
  - Public-facing EC2 web server
  - Internal Splunk server for log collection and analysis
  - Custom VPC with public and private subnets
  - Internet Gateway and Security Groups controlling access

## Critical Assets
- Identities:
  - AWS IAM users
  - EC2 instance roles
- Compute resources:
  - Web server EC2 instance
  - Splunk Enterprise EC2 instance
- Data stores:
  - Application and authentication logs
  - Security telemetry stored in Splunk indexes
- Network components:
  - VPC
  - Public subnet
  - Security Groups
  - Internet Gateway

## Trust Boundaries
- External → Internal:
  - Internet traffic entering the AWS VPC via the public web server
- User → Service:
  - SSH access to EC2 instances
  - Web access to the public application
- Service → Service:
  - Log forwarding from EC2 instances to Splunk

## Assumptions
- This project focuses on detection and investigation, not prevention
- Denial-of-service attacks are out of scope
- The environment represents a simplified but realistic SOC monitoring 
scenario

