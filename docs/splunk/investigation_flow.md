# Investigation and Verification Flow

This document describes the basic investigation and verification workflow
used to validate log ingestion and perform initial analysis in Splunk.

---

## Purpose

The goal of this workflow is to ensure that:
- Logs are successfully ingested
- Data is searchable and reliable
- Basic investigation steps can be performed

This reflects a Tier 1 SOC analyst mindset.

---

## Step 1: Log Ingestion Verification

Initial verification focuses on confirming that data is arriving in Splunk.

Checks performed:
- Search for recent events
- Validate host and source fields
- Confirm timestamps align with test activity

This step ensures the pipeline is functioning.

---

## Step 2: Source-Based Validation

Each log source is validated independently:

- System logs → authentication activity
- Nginx logs → HTTP requests

This helps isolate ingestion or parsing issues early.

---

## Step 3: Test Activity Generation

Controlled test actions were performed to generate logs:
- HTTP requests to the web server
- Failed SSH login attempts

These actions create known events that can be traced in Splunk.

---

## Step 4: Search and Analysis

Basic searches were used to:
- Filter by host
- Filter by source
- Review event patterns

The focus was on understanding what "normal" activity looks like.

---

## Step 5: Verification Outcome

Successful investigation confirmed:
- End-to-end log flow
- Accurate event visibility
- Readiness for alerting and further detection logic

---

## SOC Relevance

This workflow demonstrates:
- Structured investigation thinking
- Validation before detection
- Understanding of data quality and visibility

These skills are essential for effective SOC operations.

