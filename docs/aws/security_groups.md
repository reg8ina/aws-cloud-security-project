# Security Groups

This document summarizes the Security Groups used in the AWS Cloud Security Project.
The goal is to reduce exposure, control access, and support log visibility for Splunk 
monitoring.

---

## 1) Bastion Host Security Group (regi-bastion-sg)

Purpose: Secure SSH entry point into the VPC.

Inbound Rules:
- TCP 22 (SSH) → Allowed only from the admin/public IP (your IP)

Outbound Rules:
- Default allow outbound

Notes:
- No other inbound services are exposed on the Bastion host.

---

## 2) Splunk Server Security Group (regi-splunk-sg)

Purpose: Host Splunk Enterprise UI and receive logs from Universal Forwarder.

Inbound Rules:
- TCP 22 (SSH) → Allowed from Bastion host (recommended)
- TCP 8000 → Splunk Web UI (admin access)
- TCP 9997 → Splunk receiving port (UF ingestion)

Outbound Rules:
- Default allow outbound

Notes:
- UF ingestion should be restricted to the Web Server private IP where possible.

---

## 3) Web Server Security Group (regi-web-sg)

Purpose: Host Nginx web service and forward logs to Splunk.

Inbound Rules:
- TCP 22 (SSH) → Allowed from Bastion host
- TCP 80 (HTTP) → Public access for testing web traffic

Outbound Rules:
- TCP 9997 → Splunk Server private IP (10.0.0.149)

Notes:
- The Web Server is the only instance intended to receive public HTTP traffic.
- All other internal communication is performed over private IPs.

