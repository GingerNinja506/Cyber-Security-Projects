# TryHackMe – SOC Level 1

## Phishing Investigation – Practical Lab Documentation

This repository contains my hands-on work from the **“Snapped Phishing Line”** room (SOC Level 1 path, TryHackMe).  
The goal of this lab is to demonstrate practical phishing analysis skills: inspecting email headers, analysing malicious URLs, retrieving the phishing kit, performing OSINT, and extracting Indicators of Compromise (IoCs).

---

## Environment

- **Platform:** TryHackMe  
- **Room:** *Snapped Phishing Line*  
- **Operating System:** Linux VM  
- **Tools:** Firefox, email header viewer, VirusTotal, URLScan, WHOIS, passive DNS, CTI tooling

---

## Core Concepts Practiced

- Email structure analysis (headers + body)  
- Received-chain inspection  
- Spoofing detection  
- SPF / DKIM / DMARC validation  
- URL and redirect chain analysis  
- Credential harvesting detection  
- Phishing kit retrieval  
- File and code inspection (PHP-based kits)  
- OSINT on domains, IP addresses, hosting providers  
- Extraction of Indicators of Compromise

---

## Scenario (Background)

SwiftSpend Financial employees reported a suspicious email.  
Several users submitted their credentials and lost access to their accounts.

The investigation included:

- Analysing provided phishing email samples  
- Inspecting associated URLs  
- Retrieving the phishing kit from the adversary’s server  
- Performing CTI lookups on the domain and IP  
- Analysing the kit contents to identify attacker behaviour and extract IoCs

---

## Tasks & Solutions

### 1. Identify the sender email address  
**Method:** Inspect the `From:` header  
**Answer:** …


