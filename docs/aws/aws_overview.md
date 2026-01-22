# AWS Architecture Overview

This document provides a high-level overview of the AWS infrastructure built for the 
Cloud Security (SOC) project.
The focus is on secure access, separation of roles, and log visibility for security 
monitoring.

---

## AWS Components Used

The environment is built using the following AWS components:

- Virtual Private Cloud (VPC)
- Public Subnets
- EC2 Instances
- Security Groups
- Internet Gateway

The architecture is intentionally simple but follows real-world security principles.

---

## High-Level Architecture Design

The AWS environment contains three main EC2 instances:

1. Bastion Host  
2. Splunk Enterprise Server  
3. Web Server (Nginx + Universal Forwarder)

Each instance has a dedicated role and limited exposure.

---

## Design Principles

### 1. Controlled Access
- Direct SSH access is allowed only to the Bastion Host
- Internal servers are accessed through the Bastion
- This reduces the attack surface

### 2. Role Separation
- Web traffic is handled only by the Web Server
- Log collection and analysis are handled only by the Splunk Server
- Administrative access is isolated

### 3. Private Communication
- Internal communication between instances uses private IP addresses
- Logs are forwarded internally from the Web Server to Splunk

---

## Traffic Flow Overview

- User → HTTP → Web Server
- Web Server → Logs (UF) → Splunk Server
- Admin → SSH → Bastion → Internal Servers

This flow mirrors a basic SOC-monitored cloud environment.

---

## Why This Matters for Security

This architecture demonstrates:
- Basic cloud segmentation
- Secure access patterns
- Centralized logging
- SOC-oriented thinking

The setup serves as a foundation for log ingestion, detection, and investigation 
workflows.

