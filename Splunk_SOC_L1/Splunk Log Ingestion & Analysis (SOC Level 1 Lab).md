# TryHackMe – SOC Level 1  
## Splunk: The Basics – Practical Lab Documentation

This repository contains my hands-on work from the **“Splunk: The Basics”** room (SOC Level 1 path, TryHackMe).  
The goal of this lab is to demonstrate practical SIEM skills: ingesting VPN logs into Splunk, exploring indexed data, and answering investigation-style questions using SPL (Search Processing Language).

---

## Environment

- Platform: **TryHackMe**
- Room: *Splunk: The Basics*  
- Splunk Version: **8.x – Search & Reporting App**
- Dataset: `VPNlogs.json`
- Source type: `_json`
- Index: `VPN_Logs`

---

## Core Concepts Practiced

- Splunk architecture:
  - **Forwarder** – log collector
  - **Indexer** – parsing, field extraction, indexing
  - **Search Head** – querying and visualization
- Data ingestion workflow (Add Data → Upload → Configure)
- Basic SPL queries:
  - Filtering `field="value"`
  - Excluding values `field!="value"`
  - Aggregations (`stats count`)
  - Display formatting (`table`)
- Field exploration using the Fields Sidebar (Top values, Events with this field)

---

# Tasks & Solutions

Below are the exact questions from the room and my answers, including the SPL queries and screenshots from Splunk.

---

## **1. Upload the VPN logs into Splunk**

**THM Question:**  
*Upload the `VPN_logs` file to the Splunk instance.*

### Steps performed

1. `Add Data` → **Upload**
2. Selected `VPNlogs.json`
3. Configured:
   - **Source type:** `_json`
   - **Index:** `VPN_Logs`
   - **Host:** `ip-10-10-40-195`
<img width="1917" height="861" alt="Zrzut ekranu 2025-11-14 160859" src="https://github.com/user-attachments/assets/1c159fbe-20a4-4fbe-8607-37519f17d239" />
<img width="1916" height="917" alt="Zrzut ekranu 2025-11-14 161004" src="https://github.com/user-attachments/assets/0fdcf851-6efd-42b0-91c3-f424bfe0a145" />

4. Reviewed and finalized the ingestion

**Screenshot: Add Data workflow**  




