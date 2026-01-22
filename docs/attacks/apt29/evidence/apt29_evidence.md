# APT29 Attack Investigation – Splunk Analysis

## Overview
This document describes the detection and investigation of an APT29-style attack 
observed in the AWS Cloud Security Project.
The analysis is based on simulated Nginx access logs ingested into Splunk and tagged 
with a dedicated sourcetype.

The objective of this investigation is to demonstrate practical SOC-level analysis: 
detection, scoping, timeline reconstruction, and validation of suspicious activity.

---

## Data Source
- **Index:** default (searched as `index=*`)
- **Sourcetype:** `nginx:access:apt29`
- **Log Origin:** Web Server (Nginx) with Splunk Universal Forwarder
- **Environment:** AWS EC2 (private VPC)

---

## Detection – Raw Events Search

### SPL Query – Raw Events
```spl
index=* sourcetype="nginx:access:apt29"
```

### Detection Summary
- Total events detected: **15**
- Events originated from a **single internal host**
- Activity observed within a short, continuous time window

---

## Timeline and Duration Analysis

### SPL Query – Time Window per Host
```spl
index=* sourcetype="nginx:access:apt29"
| stats min(_time) as start max(_time) as end count as events by host
| eval duration_minutes=round((end-start)/60,2)
| convert ctime(start) ctime(end)
| table host events start end duration_minutes
```

### Observed Results
| Host          | Events | Start Time            | End Time              | Duration 
(minutes) |
|---------------|--------|-----------------------|-----------------------|--------------------|
| ip-10-0-0-149 | 15     | 12/31/2025 07:25:43   | 12/31/2025 07:35:24   | 9.68               
|

---

## Analysis and Interpretation
- The activity is concentrated on a single internal host.
- The short execution window (~10 minutes) suggests a focused, automated action.
- The behavior is consistent with reconnaissance or exploitation attempts commonly 
associated with APT-style tradecraft.
- No lateral movement was observed during this phase.

---

## MITRE ATT&CK Mapping (Behavior-Based)
- **T1190** – Exploit Public-Facing Application
- **T1071** – Application Layer Protocol

The mapping is based on observed behavior patterns rather than static indicators.

---

## Conclusion
This investigation demonstrates the ability to:
- Identify suspicious activity in Splunk
- Scope the incident using SPL
- Build a clear attack timeline
- Correlate behavior with MITRE ATT&CK techniques

This workflow reflects real-world SOC investigation practices and provides a clear, 
interview-ready example of hands-on detection and analysis.

