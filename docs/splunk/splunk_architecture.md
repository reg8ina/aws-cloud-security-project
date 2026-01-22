# Splunk Architecture (AWS SOC Lab)

This document describes how Splunk Enterprise and Universal Forwarder were used to build 
a basic cloud SOC monitoring pipeline on AWS.

---

## Components

- Splunk Enterprise Server (Receiver)
  - Hosts Splunk Web UI (TCP 8000)
  - Receives forwarded logs (TCP 9997)

- Universal Forwarder (Sender)
  - Runs on the Web Server
  - Monitors system and application logs
  - Forwards events to Splunk over the private network

- Web Server (Nginx)
  - Generates web traffic and application logs
  - Acts as a realistic log source

---

## Data Flow (High Level)

1. The Web Server generates logs (Nginx + OS logs)
2. Universal Forwarder monitors log files
3. Universal Forwarder forwards events to Splunk Receiver (TCP 9997)
4. Splunk indexes the events and makes them searchable via Splunk Web UI

---

## Private IP Log Forwarding

Forwarding uses private IPs inside the VPC to keep log transport internal:

- Web Server (UF) → Splunk Server (Receiver)

This reduces exposure and matches real-world cloud logging patterns.

---

## Validation Steps (What was checked)

- Splunk receiver listening on TCP 9997
- Universal Forwarder connected to the forward-server
- Monitored paths configured (system logs + Nginx logs)
- Search verification in Splunk Web UI (events visible)

---

## Why this matters in a SOC interview

This architecture demonstrates:
- SIEM deployment fundamentals
- Endpoint log forwarding design
- Verification mindset (connectivity + ingestion checks)
- Practical troubleshooting under real constraints

