# VPC and Networking Design

This document describes the networking layer of the AWS Cloud Security Project.
The focus is on visibility, simplicity, and clear traffic flow for SOC monitoring.

---

## Virtual Private Cloud (VPC)

- VPC Name: regi-security-vpc-vpc
- Region: eu-central-1 (Frankfurt)
- CIDR Block: 10.0.0.0/16

The VPC provides an isolated network for all project resources.

---

## Subnet Design

The environment uses public subnets to simplify access and visibility during the 
learning phase.

- Subnets are associated with an Internet Gateway
- Each EC2 instance receives a public IP
- Internal communication still uses private IP addresses

This design allows easy troubleshooting while preserving logical separation.

---

## Internet Gateway

An Internet Gateway is attached to the VPC to allow:
- SSH access to the Bastion Host
- HTTP access to the Web Server
- Access to the Splunk Web UI

---

## Routing Overview

- Default route (0.0.0.0/0) points to the Internet Gateway
- All instances can reach the internet
- Internal traffic stays within the VPC using private IPs

---

## Security Perspective

From a security standpoint:
- Public exposure is limited by Security Groups
- SSH access is restricted to the Bastion Host
- Monitoring and logging compensate for reduced isolation

This reflects a realistic SOC lab environment rather than a production-hardened setup.

