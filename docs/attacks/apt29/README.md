# APT29 (Cozy Bear) — Attack Story & Investigation (Splunk)

This section documents an APT29-style attack simulation as part of the AWS Cloud 
Security SOC Lab.
The goal is to demonstrate an end-to-end SOC workflow: log ingestion → detection 
hypothesis → investigation → evidence.

---

## 1) What is APT29 (high level)

APT29 (often referred to as “Cozy Bear”) is a well-known advanced threat actor 
associated with stealthy, persistent operations.
Typical tradecraft patterns include:
- Initial access through exposed services or phishing
- Quiet command-and-control (C2) over application-layer protocols
- Credential access attempts and lateral movement
- Long-lived, low-noise behavior

This project focuses on the SOC side: what we can observe in logs and how we reason 
about it.

---

## 2) How APT29 appears in *this* project

In this lab, APT29 indicators are represented by:
- Unusual inbound activity consistent with initial access attempts (e.g., web requests / 
scanning patterns)
- Suspicious outbound connections consistent with C2-like behavior
- Authentication anomalies consistent with credential access attempts
- Short, focused “attack window” where multiple suspicious events cluster in time

Data sources used:
- Web server logs (e.g., Nginx access/error logs)
- OS authentication logs (SSH failures / auth activity)
- Any additional simulated APT logs (if present in the dataset)

---

## 3) Investigation approach (SOC workflow)

### Step A — Confirm data availability
We start by validating that the relevant host(s) and sources exist in Splunk:
- Identify the host generating events
- Validate timestamps and volume
- Confirm that the suspected time window exists

### Step B — Build a timeline of events
We create a time-based view to see clustering patterns:
- What happened first?
- What followed?
- Do we see pivot points (initial access → C2 → credential access)?

### Step C — Pivot & enrich
We pivot by:
- Source / sourcetype
- Host
- src_ip / dest_ip
- user
- URI / status codes

### Step D — Summarize evidence
We collect the strongest “proof points”:
- a small set of searches that consistently reproduce findings
- screenshots of Splunk panels or search results
- extracted IOCs (IPs, URIs, usernames) if available

---

## 4) Practical SPL queries used in this project

> Note: You may need to adjust `index`, `sourcetype`, and `host` to match your 
environment.
> Store final working queries under: `docs/attacks/apt29/queries/`

### 4.1 Find suspicious time window (broad)

index=* host=* earliest=-24h
| timechart span=1m count


### 4.2 Web traffic anomalies (Nginx access)

index=* (source="nginxaccess*" OR sourcetype=nginx)
| stats count as hits values(status) as statuses values(uri_path) as paths by src_ip, 
host
| sort - hits


### 4.3 Authentication anomalies (SSH / auth logs)

index=* (source="auth" OR source="secure" OR sourcetype=auth)
| stats count as attempts values(user) as users values(src_ip) as srcs by host
| sort - attempts


### 4.4 Possible C2-like outbound patterns (network/proxy logs if exist)


index=* (dest_ip=* OR url=* OR uri=*)
| stats count values(dest_ip) values(url) by host, src_ip
| sort - count


### 4.5 Build an investigation timeline (generic)

index=* host=<YOUR_HOST> earliest=<START> latest=<END>
| eval marker=case(match(_raw,"Failed password"),"AuthFail",
match(_raw,"nginx"),"Web",
1=1,"Other")
| timechart span=1m count by marker



---

## 5) Evidence checklist (what to include for interview)

Store proof under: `docs/attacks/apt29/evidence/`

Include:
- A screenshot of the timeline showing the attack window
- A screenshot of top source IPs / suspicious URIs
- A screenshot of auth anomalies (failed logins / user anomalies)
- A short written conclusion (3–6 lines): what happened, how you know, impact, next 
steps

Store screenshots under:
- `docs/attacks/apt29/screenshots/`

---

## 6) Conclusion template (fill after you lock the final searches)

**Attack window:** <start> → <end>  
**Primary affected host(s):** <hostnames>  
**Initial access evidence:** <web anomalies / exploit attempts>  
**Credential access evidence:** <auth anomalies>  
**Suspected C2 evidence:** <outbound patterns if present>  
**SOC response (recommended):** block IOC(s), reset creds, patch exposed service, hunt 
for persistence

---

## Next step
Create a “real evidence” version of these queries by confirming your actual:
- index name(s)
- sourcetype/source values
- the host that contains the APT29 events

Then store final working SPL under:
`docs/attacks/apt29/queries/apt29_hunt.spl`

