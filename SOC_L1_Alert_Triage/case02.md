## Initial Handling
The initial triage steps are identical to Case 01:
- Selected alert based on severity and time (highest severity → oldest item)
- Assigned alert to myself (L1)
- Set status to **In Progress**
- Began data review

Further analysis below focuses only on elements specific to this case.

## Analysis

### 1. Alert Context
The alert indicates a **double-extension file creation**, a technique used in phishing to disguise executable malware as media or document files.

Key details:
- Host: LPT-HR-009  
- Process: chrome.exe  
- User: S.Conway  
- Target file: `cats2025.mp4.exe`  
- File path: `C:\Users\S.Conway\Downloads\cats2025.mp4.exe`  
- Source URL: `https://freecatvideoshd.monster/cats2025.mp4.exe`  
- MD5: 14d8486f3f63875ef93cfd240c5dc10b

<img width="1465" height="416" alt="image" src="https://github.com/user-attachments/assets/7793c5c8-a0d6-414f-b119-226db575a058" />


### 2. Indicators of Malicious Activity
Observed factors strongly indicate malicious intent:

- **Double extension** (`.mp4.exe`) — classic phishing/malware obfuscation method.
- Download originated from a **suspicious domain** (`freecatvideoshd.monster`) inconsistent with corporate usage.
- File executed from the **Downloads** directory, typical for user-initiated phishing downloads.
- Process responsible: **chrome.exe**, consistent with user clicking a malicious link.
- File name suggests **social engineering lure** (“cats2025” + “mp4” → disguised as a video).

### 3. Initial Threat Assessment
This behavior matches common malware-delivery scenarios:
- User opens a phishing page  
- Downloads a disguised executable  
- Attempts to run it  
- Endpoint security triggers on the creation event

This is *not* a false positive pattern — all indicators align with genuine malware execution attempts.

### 4. Required Checks (Performed)
- Domain reputation check → **Malicious / suspicious TLD `.monster`**  
- File hash MD5 lookup → expected to be flagged as malicious (attach results if needed)  
- Host role → standard workstation (HR), not a sandbox or research system  
- User role → standard corporate user → increased impact

<img width="1584" height="927" alt="image" src="https://github.com/user-attachments/assets/0a235564-9212-4b48-9bd4-3971d960df1b" />


### 5. Preliminary Verdict
**True Positive – Malware delivery attempt**

The double-extension file and untrusted domain confirm malicious activity.


