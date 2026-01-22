# Universal Forwarder Configuration and Log Sources

This document describes how the Splunk Universal Forwarder (UF) was configured
to collect and forward logs to the Splunk Enterprise receiver.

---

## Universal Forwarder Role

The Universal Forwarder runs on the Web Server and is responsible for:
- Monitoring local log files
- Forwarding events to the Splunk receiver
- Maintaining a lightweight footprint on the host

---

## Forwarding Configuration

The Universal Forwarder was configured to forward data to the Splunk receiver:

- Destination: Splunk Enterprise Server
- Protocol: TCP
- Port: 9997
- Network: Private VPC communication

This ensures logs are transferred internally and securely.

---

## Log Sources Monitored

The following log sources were configured for monitoring:

### System Logs
- Authentication logs
- General system messages

These logs support detection of:
- Failed login attempts
- Suspicious authentication behavior

### Application Logs (Nginx)
- Access logs
- Error logs

These logs support:
- Visibility into HTTP traffic
- Basic web activity investigation

---

## Validation Steps

The following checks were performed:

- Confirm UF service is running
- Verify connection to the forward-server
- Generate test traffic on the Web Server
- Confirm events appear in Splunk searches

---

## SOC Perspective

From a SOC standpoint, this configuration demonstrates:
- Endpoint log collection
- Separation between log generation and analysis
- Validation of ingestion before investigation

This is a foundational step in any SIEM-based monitoring pipeline.

