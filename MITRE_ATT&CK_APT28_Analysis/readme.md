# MITRE ATT&CK Analysis – APT28
This section documents the analysis of APT28 using the MITRE ATT&CK Navigator.  
Goal: identify the Tactics, Techniques, and Procedures (TTPs) associated with APT28 in order to check for possible intrusion paths within E-Corp’s environment.

---

## 1. Intelligence Summary
APT28 (also known as Fancy Bear / Sofacy) is a well-established threat actor targeting government, defense, aerospace, and industrial sectors.  
The team received a classified report indicating the group may target organizations similar to E-Corp.

Task: Use the MITRE ATT&CK Navigator layer for APT28 to answer the investigation questions.
<img width="1912" height="992" alt="image" src="https://github.com/user-attachments/assets/454a3d2a-816c-4cee-827c-648711efb3b3" />

**1. What is a technique used by the APT to both perform recon and gain initial access?**
Method:

Using the MITRE ATT&CK Navigator layer provided for APT28, I reviewed all techniques marked under both Reconnaissance and Initial Access.
The goal: identify a single technique that appears in both phases.

APT28 commonly uses phishing campaigns with malicious links embedded in emails. This technique was visible in:

Recon → Phishing for Information

Initial Access → Spearphishing Link (T1566.002)

Evidence (screenshots)

<img width="627" height="553" alt="image" src="https://github.com/user-attachments/assets/2ad076a1-cf20-451e-b76d-65877f741728" />


Answer:

Spearphishing Link (T1566.002 / T1598.003)

**2.  Which accounts might the APT compromise while developing resources?**

Method:

Using the MITRE ATT&CK Navigator layer for APT28, I reviewed all techniques mapped under Resource Development.
The objective: determine which type of accounts the APT is known to compromise during preparation for further operations.

APT28 has documented activity involving the compromise of legitimate accounts for use in phishing and command-and-control operations.

The highlighted technique in the Navigator:

Email Accounts (T1586.002)
This technique explicitly states that APT28 has used compromised email accounts to send phishing emails and support their infrastructure.

Evidence (screenshots)

<img width="493" height="214" alt="image" src="https://github.com/user-attachments/assets/063e5446-1b14-4570-8ea8-a01579d294a4" />


Answer:

Email Accounts (T1586.002)
