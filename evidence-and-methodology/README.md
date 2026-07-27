---
description: >-
  This section explains what evidence was analyzed, what tools were used, and
  how the investigation was performed.
---

# 📁 Evidence & Methodology

### Overview

The investigation was conducted through the analysis of captured network traffic and digital evidence collected from the simulated incident environment.

The primary source of evidence was a packet capture (PCAP) file containing network communications recorded during the attack. The captured traffic was analyzed to identify malicious activity, trace attacker behavior, and reconstruct the sequence of events.

***

## Tools Used

The following tools were used during the investigation:

### Wireshark

**Purpose:**\
Network protocol analysis and packet inspection.

**Usage:**

* Analyzed captured network traffic.
* Filtered suspicious communications.
* Examined HTTP requests and responses.
* Identified attacker activities and indicators of compromise.

***

### Network Analysis Tools

**Purpose:**\
Supporting investigation and evidence analysis.

**Usage:**

* Examined network connections.
* Investigated suspicious IP addresses.
* Assisted in understanding attacker communication patterns.

***

### GitBook

**Purpose:**\
Documentation and presentation of investigation findings.

**Usage:**

* Structured the incident report.
* Documented investigation procedures.
* Presented findings, evidence, and recommendations.

***

## Evidence Collection

### Primary Evidence

**Evidence Source:**

* Network Packet Capture (PCAP) file

**Evidence Type:**

* Network traffic logs
* HTTP communications
* Connection metadata
* Attacker activity traces

***

### Evidence Handling Process

The following steps were followed during evidence analysis:

1. The PCAP file was obtained from the investigation scenario.
2. The evidence file was opened using Wireshark for analysis.
3. Network traffic was examined to identify suspicious patterns.
4. Relevant packets and communications were documented.
5. Screenshots were captured to support investigation findings.
6. Findings were organized into a structured SOC incident report.

***

## Investigation Process

The investigation followed a structured incident response approach:

### Phase 1 — Evidence Review

* Reviewed available investigation materials.
* Loaded the PCAP file into the analysis environment.
* Confirmed the available network traffic data.

***

### Phase 2 — Network Traffic Analysis

* Applied Wireshark filters to identify suspicious activity.
* Examined communication between hosts.
* Investigated abnormal requests and connections.

***

### Phase 3 — Attack Reconstruction

* Identified attacker actions.
* Analyzed malicious activity patterns.
* Reconstructed the sequence of events.

***

### Phase 4 — Findings & Reporting

* Documented confirmed findings.
* Created an attack timeline.
* Identified indicators of compromise.
* Developed security recommendations.

***

**Note:**\
Screenshots and packet-level evidence collected during the investigation are included in the relevant analysis and findings sections.
