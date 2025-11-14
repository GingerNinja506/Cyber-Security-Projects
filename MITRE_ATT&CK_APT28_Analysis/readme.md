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

**3. User Execution Techniques Used by APT28**
Question:

E-corp has found that the APT might have gained initial access using social engineering to make the user execute code for the threat actor. Sunny wants to identify if the APT was also successful in execution. What two techniques of user execution should Sunny look out for?

Techniques Identified

Based on the MITRE ATT&CK Navigator layer for APT28, two user-execution techniques are highlighted:

Malicious File (T1204.002)
APT28 delivers Microsoft Office documents containing malicious macro scripts, prompting users to open and run the payload.

Malicious Link (T1204.001)
APT28 sends phishing emails containing malicious hyperlinks, tricking users into executing attacker-controlled code.
<img width="520" height="314" alt="image" src="https://github.com/user-attachments/assets/3ea8d77b-dca6-411b-a3e8-662289d69c17" />
<img width="406" height="262" alt="image" src="https://github.com/user-attachments/assets/70c00479-57ae-4184-84e8-df66ac0da2d9" />


Answer:

Malicious File and Malicious Link

**4. Scripting Interpreters Used by APT28**

Question
If the above technique was successful, which scripting interpreters should Sunny search for to identify successful execution?

Techniques Identified
According to the MITRE ATT&CK Navigator layer for APT28, the group frequently relies on scripting interpreters to execute payloads once initial user execution succeeds:

PowerShell (T1059.001)
APT28 downloads and executes PowerShell scripts to perform reconnaissance, run commands, and deploy additional payloads.

Windows Command Shell (T1059.003)
APT28 uses cmd.exe and batch scripts for macro execution, payload staging, and running embedded commands.

<img width="454" height="335" alt="image" src="https://github.com/user-attachments/assets/859410e5-67c0-4bd8-97f7-0b306f5ef38a" />
<img width="605" height="313" alt="image" src="https://github.com/user-attachments/assets/64cb6b63-a9c3-43e3-bdc7-6b925057d466" />


Answer
Powershell and Windows Command shell
