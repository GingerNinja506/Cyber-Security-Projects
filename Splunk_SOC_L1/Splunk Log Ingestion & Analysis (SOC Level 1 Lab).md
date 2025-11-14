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
<img width="1917" height="915" alt="Zrzut ekranu 2025-11-14 161027" src="https://github.com/user-attachments/assets/258ed62e-c8a7-4edc-916f-3c43655aceea" />

---

## **2. How many events are pressent in log file.**

### <img width="1908" height="912" alt="Zrzut ekranu 2025-11-14 161125" src="https://github.com/user-attachments/assets/4411aae6-3462-4b06-8c4d-18dd8e589b79" />

**Answer:** 2862

## **3. How many log events are captured by the user Maleena?**

<img width="871" height="526" alt="Zrzut ekranu 2025-11-14 161244" src="https://github.com/user-attachments/assets/e4bae92a-0d6d-4834-89ed-ec44d6b62aee" />

**Answer:** 60

## **4. What is the username associated with IP 107.14.182.38?**

Method:
To identify the username tied to a specific IP address, I filtered the logs directly in the Splunk Search Head using a simple SPL query that narrows results to that IP only.

SPL Query Used:

Source="VPNlogs.json" host="ip-10-10-40-195" sourcetype="_json" Source_ip="107.14.182.38"


Explanation:
Adding the condition Source_ip="107.14.182.38" restricts the search to only those events coming from the target IP.
Splunk returned 26 matching events, all containing the field:

UserName: Smith

<img width="1907" height="871" alt="Zrzut ekranu 2025-11-14 165059" src="https://github.com/user-attachments/assets/fe6bd5dc-c227-43d3-b2f6-feb4d90412e1" />

**Answer:** Smith

## **5. What is the number of events that originated from all countries except France?**

Method:
To determine how many VPN events originated from all countries except France, I applied a simple SPL filter in the Search Head.
By adding a negation condition on the Source_Country field, Splunk excludes all events where the country equals “France”.

SPL Query Used:

source="VPNlogs.json" host="ip-10-10-40-195" sourcetype="_json" Source_Country!="France"

<img width="1906" height="917" alt="Zrzut ekranu 2025-11-14 165341" src="https://github.com/user-attachments/assets/543f4ca4-3f02-469e-9c66-3d93414c122a" />


Explanation:
The operator != tells Splunk to remove all events where Source_Country is exactly “France”.
Splunk returned 2,814 events, representing all VPN connections from every other country.









