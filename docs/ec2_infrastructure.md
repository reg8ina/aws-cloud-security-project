# AWS EC2 Infrastructure Overview  
Cloud Security Project Architecture

## Overview
This document describes the EC2-based infrastructure used in the AWS Cloud Security Project.  
The environment includes a Bastion host, a Splunk Enterprise server, and a Web Server running Nginx with Universal 
Forwarder.  
All components are deployed inside a custom VPC and communicate using private IP addresses.

---

## EC2 Instances Summary

| Instance Name        | Public IP        | Private IP   | Instance Type | Security Group     | Role / Purpose                                   
|
|---------------------|------------------|--------------|----------------|--------------------|--------------------------------------------------|
| regi-bastion-ec2     | 18.195.148.236   | 10.0.13.23   | t3.micro       | regi-bastion-sg    | Secure SSH entry point to 
VPC                    |
| regi-splunk-server   | 3.120.190.194    | 10.0.0.149   | t3.small       | regi-splunk-sg     | Splunk Enterprise (ports 
8000, 9997)             |
| regi-web-ec2-new     | 3.75.189.97      | 10.0.15.63   | t3.micro       | regi-web-sg        | Web server (Nginx + 
Universal Forwarder)         |

*Legacy instance (regi-web-ec2) removed from architecture.*

---

## Network Architecture

All instances reside inside the same custom VPC:

- VPC Name: `regi-security-vpc-vpc`
- Region: eu-central-1 (Frankfurt)
- Subnets: Public subnets
- Routing: Internet Gateway attached for outbound access

### Private Connectivity Flow
[Bastion 10.0.13.23]
        |
        | (SSH)
        v
[Web Server 10.0.15.63] -----> (UF logs) -----> [Splunk 10.0.0.149]

### Public Access Flow
Your Laptop
     |
     | SSH → Bastion (18.195.148.236)
     |
     | HTTP → Web Server (3.75.189.97)
     |
     | Splunk Web → Splunk (3.120.190.194:8000)

---

## Security Groups Summary

### regi-bastion-sg
Inbound:
- TCP 22 (SSH) — From your IP  
Outbound:
- Allow all  
Purpose: Secure SSH access into the VPC.

### regi-splunk-sg
Inbound:
- TCP 22 — From Bastion  
- TCP 8000 — Splunk Web UI  
- TCP 9997 — From Universal Forwarder  
Outbound:
- Allow all  
Purpose: Accept logs + serve Splunk UI.

### regi-web-sg
Inbound:
- TCP 22 — From Bastion  
- TCP 80 — HTTP (Nginx)  
Outbound:
- TCP 9997 → Splunk private IP (10.0.0.149)  
Purpose: Host Nginx + send logs to Splunk.

---

## Instance Roles

### 1. Bastion Host
- Entry point for SSH  
- Protects internal servers from public exposure  
- Used for administrative access only  

### 2. Splunk Enterprise Server
- Receives logs from Universal Forwarder  
- Hosts Splunk Web UI (port 8000)  
- Listens for log ingestion on port 9997  

### 3. Web Server (Nginx + Universal Forwarder)
- Serves web pages over HTTP  
- Forwards Nginx + system logs to Splunk  
- Used for detection, learning, and attack simulation  

---

## Useful EC2 Commands

Update server:
sudo apt update && sudo apt upgrade -y

Install Nginx:
sudo apt install nginx -y

Check listening ports:
ss -tulnp

Test HTTP:
curl http://3.75.189.97

Check Splunk server connectivity:
ping 10.0.0.149

Restart Nginx:
sudo systemctl restart nginx

---

## Final Architecture Status
✔ All EC2 instances are deployed and reachable  
✔ Internal routing between servers works  
✔ Web Server successfully forwards logs to Splunk  
✔ Bastion host secured  
✔ Environment ready for security monitoring & analysis  

