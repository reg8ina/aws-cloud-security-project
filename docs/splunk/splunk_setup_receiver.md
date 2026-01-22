# Splunk Enterprise Setup (Receiver)

This document describes the installation and initial configuration of Splunk Enterprise
as a log receiver in the AWS Cloud Security Project.

---

## Splunk Server Role

The Splunk server is responsible for:
- Receiving logs from Universal Forwarder
- Indexing events
- Providing search and investigation capabilities via Splunk Web UI

---

## Installation

Splunk Enterprise was installed on an EC2 instance running Linux.

Example installation steps:

- Upload or download Splunk package
- Install using system package manager
- Start Splunk and accept license

Splunk was configured to start automatically on boot.

---

## Receiver Configuration

Splunk was configured to listen for incoming data from Universal Forwarders:

- Receiving Port: TCP 9997
- Scope: Internal VPC traffic only

This port is used exclusively for log ingestion.

---

## Splunk Web Interface

Splunk Web UI was accessed via:

- Port: TCP 8000
- Used for searches, verification, and basic investigation

---

## Verification Steps

The following checks were performed to validate the setup:

- Confirm Splunk is running
- Confirm port 9997 is listening
- Access Splunk Web UI
- Verify no errors during startup

---

## Security Considerations

- Splunk UI access limited to administrative use
- Log ingestion restricted via Security Groups
- Receiver not exposed unnecessarily to public traffic

This setup reflects a realistic SOC deployment model for a cloud-based SIEM.

