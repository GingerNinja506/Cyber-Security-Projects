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

### 1. Who is the individual who received an email attachment containing a PDF?

**Method:**  
Searched all email samples for references to PDF attachments by matching the `.pdf` pattern inside MIME headers.  

Command used: grep "\.pdf" ./phish-emails/*

<img width="932" height="501" alt="image" src="https://github.com/user-attachments/assets/f86a37b0-e0b6-4c7d-9efb-21384a065c64" />

<img width="928" height="608" alt="image" src="https://github.com/user-attachments/assets/44d88f90-358e-4ea8-b5cf-87d9f7138851" />

<img width="819" height="267" alt="image" src="https://github.com/user-attachments/assets/8ad6c38a-d537-40f4-aa6a-d74f858a9d96" />

**Answer:**  
**William McClean**

### 2. What email address was used by the adversary to send the phishing emails?

**Method:**  
Opened the identified phishing email and inspected the `From:` header to extract the sender's email address used by the adversary.

<img width="948" height="172" alt="image" src="https://github.com/user-attachments/assets/bae08aad-15f4-4fe7-bfd9-7bb6eceb0629" />

**Answer:**  
**Accounts[.]Payable@groupmarketingonline[.]icu**








