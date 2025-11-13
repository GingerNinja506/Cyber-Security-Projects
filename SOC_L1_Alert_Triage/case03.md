## Initial Handling
The initial triage steps are identical to Case 01:
- Selected alert based on severity and time (highest severity → oldest item)
- Assigned alert to myself (L1)
- Set status to **In Progress**
- Began data review

Further analysis below focuses only on elements specific to this case.

## Analysis

### 1. Alert Context
The alert detects a download from GitHub, which can contain both legitimate open-source tools and malicious scripts.  
Details:

- URL accessed: https://github.com/facebook/react  
- User: G.Chandler  
- Host: LPT-IT-063  
- Network: VPN/DEVELOPERS  
- Severity: Low  

<img width="1455" height="279" alt="image" src="https://github.com/user-attachments/assets/7b5a4443-aba3-4998-a6b5-36ac1ed1de94" />

The rule does not indicate malware — only flags the event because GitHub can host malicious content.

---

### 2. Asset & User Verification
- Source network is **VPN/DEVELOPERS**, suggesting a development or engineering context.  
- User **G.Chandler** appears to belong to a technical team (based on naming conventions and network segment).  
- Hostname **LPT-IT-063** corresponds to an IT workstation, not a standard employee laptop.  

This strongly aligns with legitimate developer activity.

---

### 3. URL Reputation & Context Check
The accessed URL points to:
https://github.com/facebook/react

This is the **official React repository** by Meta (Facebook), one of the most widely used frontend frameworks.

Quick checks:
- Official GitHub organization: *facebook*  
- High-trust, widely adopted open-source project  
- No suspicious path, no unusual forks, no raw script execution  

No indicators of compromise.

### Safety Note (Analyst Procedure)
In a real SOC environment the analyst must not open URLs directly,
even if the domain appears legitimate.  
Instead, reputation checks must be performed using safe tools such as:

- VirusTotal (URL scan)
- urlscan.io
- Threat Intelligence feeds

This procedure prevents accidental exposure to malicious payloads
and maintains analyst operational security.

<img width="1692" height="927" alt="image" src="https://github.com/user-attachments/assets/6d8b7efe-95ee-445f-a414-2a7f69e2a3b2" />



### 4. Threat Assessment
No signs of malicious activity:

- URL belongs to a **top-tier verified open-source vendor**  
- Download aligns with the user's likely job role  
- GitHub repository is well-known, safe, and widely used  
- No associated malware indicators, no suspicious domains, no abnormal behavior  

This matches **normal developer workflow**.

---

### 5. Preliminary Verdict
**False Positive – Legitimate developer activity**

No escalation required.


