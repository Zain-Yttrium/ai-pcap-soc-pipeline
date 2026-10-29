# ai-pcap-soc-pipeline
Automated SOC incident triage pipeline combining tshark network telemetry extraction with AI-driven threat intelligence.
# 🛡️ AI-Powered PCAP Malware Traffic Analysis & SOC Triage Pipeline

An automated, pure-Python Security Operations Center (SOC) incident triage pipeline that parses raw packet captures (`.pcap`), correlates Active Directory endpoint identities, identifies malicious C2 beaconing patterns, decompresses encapsulated payloads, and generates structured AI-synthesized incident reports using the Gemini API.

---

## 📸 Key Features

* **Zero External Dependencies:** Built strictly using pure Python standard libraries (`subprocess`, `urllib.request`, `zlib`, `json`) to run natively inside Linux/Kali environments without requiring third-party package installations.
* **Identity & Network Correlation:** Extracts Kerberos User Principal Names (UPNs) and NetBIOS hostnames via `tshark` CLI integration to map internal IP addresses directly to compromised user accounts and workstations.
* **C2 Beaconing Detection:** Identifies periodic HTTP request patterns, host domain rotation, session identifiers, and HTTP response status codes (`200 OK` vs `204 No Content`).
* **Multi-Layer Payload Decompression:** Automatically detects GZIP-compressed response payloads (`1f 8b` magic header) embedded in C2 streams and extracts decompressed binary previews.
* **AI Synthesis & SOC Reporting:** Constructs structured JSON telemetry and sends it to the Gemini API with automatic exponential backoff and multi-model fallback (`gemini-3.8-flash`, `gemini-2.5-flash`) to generate executive-ready Markdown Incident Reports.

---

## 🏗️ Architecture Pipeline

```text
[ Raw PCAP File ]
       │
       ▼
┌────────────────────────────────────────────────────────┐
│ 1. Extraction Engine (tshark CLI Wrapper)              │
│    - Kerberos UPN & NetBIOS Host Name Correlation      │
│    - HTTP Request/Response Telemetry Extraction        │
└──────────────────────────┬─────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────┐
│ 2. SOC Triage & Parsing Engine                         │
│    - Identifies Hostname & User Account                │
│    - Filters C2 Beaconing Frequency & Status Codes     │
│    - Decompresses GZIP-Encapsulated Payloads (1f8b)   │
└──────────────────────────┬─────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────┐
│ 3. LLM AI Synthesis Engine                             │
│    - Formats Telemetry into Structured JSON Artifact   │
│    - Exponential Backoff & Model Fallback Handling     │
│    - Generates Comprehensive Markdown Incident Report  │
└────────────────────────────────────────────────────────┘
