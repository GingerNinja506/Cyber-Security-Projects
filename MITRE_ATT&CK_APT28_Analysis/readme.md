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


Answer:
Powershell and Windows Command shell

**5. Persistence via Registry Keys**

Question
While looking at the scripting interpreters identified in Q4, Sunny found some obfuscated scripts that changed the registry. Assuming these changes are for maintaining persistence, which registry keys should Sunny observe to track these changes?

Techniques Identified
MITRE ATT&CK shows that APT28 uses persistence mechanisms involving Windows Registry modifications:

Registry Run Keys / Startup Folder (T1547.001)
APT28 deploys malware that writes itself into Run keys or the Startup folder, ensuring it executes automatically upon system boot or user logon.

<img width="405" height="264" alt="image" src="https://github.com/user-attachments/assets/1dc33ed9-97b0-4be9-acf9-cf3585f84db0" />

Answer:
Registry Run Keys / Startup Folder

**6. System Binary Proxy Execution**

Question
Sunny identified that the APT executes system binaries to evade defenses. Which system binary's execution should Sunny scrutinize for proxy execution?

Technique Identified
APT28 is known to abuse System Binary Proxy Execution (T1218) to run malicious payloads using trusted Windows binaries.
The MITRE ATT&CK layer highlights:

Rundll32 (T1218.011)
APT28 frequently uses rundll32.exe to execute malicious DLL payloads, leveraging it as a trusted system binary to bypass security controls.

Examples from MITRE notes:

Executing malicious DLLs via:
rundll32.exe C:\Windows\twain_64.dll

Using rundll32 in initial loader stages

Using rundll32 to execute payloads delivered through phishing

<img width="548" height="490" alt="image" src="https://github.com/user-attachments/assets/263d72c3-c649-4067-8a7d-04a95c1a1e45" />

Answer:
Rundll32

**7. Discovery Technique Used by APT28**

Question
Sunny identified tcpdump on one of the compromised hosts. Assuming this was placed there by the threat actor, which technique might the APT be using here for discovery?

Technique Identified
APT28 is known to deploy packet capture utilities to intercept traffic, capture credentials, and perform internal network reconnaissance.

Highlighted MITRE technique:

Network Sniffing (T1040)
APT28 uses tools such as Respondr or Wi-Fi interception equipment, and may drop tools like tcpdump to capture network packets and extract usernames, hashes, or sensitive data.

<img width="440" height="528" alt="image" src="https://github.com/user-attachments/assets/f2f15cfb-1c9c-4b0f-92e5-abfe8bdabdeb" />


Answer
Network Sniffing

**8.Lateral Movement via Remote Service Exploitation**

Question
It looks like the APT achieved lateral movement by exploiting remote services. Which remote services should Sunny observe to identify APT activity traces?


Technique Identified
APT28 is documented using SMB/Windows Admin Shares (T1021.002) for lateral movement.
This technique involves mapping network drives using valid credentials and abusing administrative shares (such as C$, ADMIN$, or custom SMB shares) to move laterally and execute attacker-controlled payloads across the network.

<img width="606" height="250" alt="image" src="https://github.com/user-attachments/assets/ca21dddf-5183-4fbc-886e-9819bdd2cda0" />


Answer
SMB/Windows Admin Shares

**9. Likely Targeted Information Repository**

Question

It looked like the primary goal of the APT was to steal intellectual property from E-corp's information repositories. Which information repository can be the likely target of the APT?

Technique Identified

APT28 is documented collecting sensitive data from SharePoint (T1213.002).
They have accessed Microsoft SharePoint services inside target environments to extract documents, internal files, and intellectual property.

This aligns with E-corp’s concern about compromised internal repositories.

<img width="308" height="235" alt="image" src="https://github.com/user-attachments/assets/ce5e79da-4681-4802-b567-38cc9f1f17e0" />


Answer

SharePoint
