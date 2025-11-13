##  Alert Triage
From the queue of five pending alerts, the correct triage order follows two rules used in SOC L1:

1. **Severity priority** – Critical → High → Medium → Low  
2. **Oldest first** within the same severity category

Based on these rules, the alert selected for this case is:

**"Potential Data Exfiltration" – Severity: Critical – Timestamp: Mar 21st 2025 at 13:30**

It is the oldest alert with the highest severity, making it the correct first choice for investigation.

<img width="1524" height="629" alt="image" src="https://github.com/user-attachments/assets/bf5a604c-2790-416e-ab03-94d27d6cb865" />

## 1. Initial Handling
The alert **"Potential Data Exfiltration" (Critical severity)** was identified as the highest-priority item in the queue.

Actions taken:
- **Assigned the alert to myself (L1)**  
- **Changed status to "In Progress"**  
- Confirmed that no previous triage actions had been performed  
- Began the analysis workflow following standard SOC L1 procedure

<img width="672" height="466" alt="image" src="https://github.com/user-attachments/assets/e299e8dc-86a6-41a3-8a76-65ccbfde8580" />

## 2. Analysis

### 2.1 Alert Context Review
The alert flagged **>5 GB of outgoing data** sent from a single internal device to an external domain within one day.  
Internal source: **192.168.45.66 (UK04/MEETINGROOM)**  
Destination: ** *.zoom.us **  
Sent: **5.8 GB**  
Received: **5.2 GB**

<img width="1459" height="327" alt="image" src="https://github.com/user-attachments/assets/020b5faf-8d0d-4621-a947-eaebec4985a8" />


This volume of traffic matches collaboration/video-conference usage patterns and does not immediately imply malicious activity. The asset is a **meeting-room device**, which further supports a legitimate use case (e.g., Zoom meetings, screen sharing).

### 2.2 Verification of Destination (Reputation Check)
Performed reputation lookup of destination domain on VirusTotal.

- Scanned domain: **zoom.us**  
- VT detections: **0/98**  
- Content type: text/html (normal web page)  
- Community indicators: neutral/clean  
- Interpretation:  
  VirusTotal does not analyze wildcard domains directly, but scanning the root domain validates the trust of all subdomains (`*.zoom.us`).  
  No signs of malicious reputation or compromise.

<img width="1782" height="926" alt="image" src="https://github.com/user-attachments/assets/e1bdc91a-99ab-44ce-9def-557cdbf10119" />


**Conclusion:**  
The destination is **trusted** and belongs to the legitimate Zoom infrastructure.

### 2.3 Data Exfiltration Risk Assessment
- Traffic direction: outbound + inbound (typical for conferencing)  
- Traffic volume: high but consistent with HD video-call usage  
- Asset role: meeting-room system → expected to generate high Zoom traffic  
- No IOC, malicious indicators, or unusual domains involved  
- No additional suspicious activity correlated to this source IP

**Risk Assessment:**  
Low likelihood of data exfiltration.  
Strong indicators that this is **normal business-related Zoom usage**.

### 2.4 Verdict
Based on domain reputation, traffic analysis, and asset role:

**Verdict: False Positive**

### 2.5 Resolution
- Alert closed as **False Positive**  
- No further action required  
- Documented findings in the case file




