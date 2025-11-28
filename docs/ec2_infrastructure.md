
Overview

This document describes the EC2-based infrastructure used in the AWS Cloud Security Project.
The environment includes a Bastion host, a Splunk Enterprise server, and a Web Server running Nginx with Universal 
Forwarder.
All components are deployed inside a custom VPC and communicate using private IP addresses.

EC2 Instances Summary
Instance Name	Public IP	Private IP	Instance Type	Security Group	Role / Purpose
regi-bastion-ec2	18.195.148.236	10.0.13.23	t3.micro	regi-bastion-sg	Secure SSH entry point to VPC
regi-splunk-server	3.120.190.194	10.0.0.149	t3.small	regi-splunk-sg	Splunk Enterprise (ports 8000, 9997)
regi-web-ec2-new	3.75.189.97	10.0.15.63	t3.micro	regi-web-sg	Web server (Nginx + Universal 
Forwarder)

Legacy instance (regi-web-ec2) removed from architecture.

Network Architecture

All instances reside inside the same custom VPC:

VPC Name: regi-security-vpc-vpc

Region: eu-central-1 (Frankfurt)

Subnets: Public subnets

Routing: Internet Gateway attached for outbound access

Private Connectivity Flow (Internal)

[Bastion 10.0.13.23] → SSH → [Web Server 10.0.15.63] → Logs → [Splunk 10.0.0.149]

Public Access Flow (External)

Laptop → SSH → Bastion (18.195.148.236)
Laptop → HTTP → Web (3.75.189.97)
Laptop → Splunk Web (3.120.190.194:8000)

Security Groups Summary
regi-bastion-sg

Inbound:

TCP 22 (SSH) — From your IP
Outbound: Allow all
Purpose: Entry point for SSH to the VPC.

regi-splunk-sg

Inbound:

TCP 22 — From Bastion

TCP 8000 — Splunk Web UI

TCP 9997 — Universal Forwarder
Outbound: Allow all
Purpose: Splunk Enterprise server.

regi-web-sg

Inbound:

TCP 22 — From Bastion

TCP 80 — HTTP
Outbound:

TCP 9997 → Splunk private IP (10.0.0.149)
Purpose: Web server + log forwarder.

Instance Roles
1. Bastion Host

Secure SSH access gateway

Protects EC2 instances from public exposure

Used for admin access

2. Splunk Enterprise Server

Receives logs from Universal Forwarder

Hosts Splunk Web UI on port 8000

Accepts logs on port 9997

3. Web Server (Nginx + Universal Forwarder)

Serves HTTP traffic

Forwards logs to Splunk

Used for detections & attack simulations

Useful EC2 Commands

Update server:
sudo apt update && sudo apt upgrade -y

Install Nginx:
sudo apt install nginx -y

Check listening ports:
ss -tulnp

Test HTTP:
curl http://3.75.189.97

Ping Splunk:
ping 10.0.0.149

Restart Nginx:
sudo systemctl restart nginx

Final Architecture Status

✔ Infrastructure deployed
✔ Logs flow from Web → Splunk
✔ Bastion protects SSH access
✔ System ready for SOC analysis and monitoring
