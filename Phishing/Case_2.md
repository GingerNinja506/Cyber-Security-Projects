# TryHackMe – SOC Level 1

## Phishing Investigation – Practical Lab Documentation

This repository contains my hands-on work from the **“Email Header Analysis”** case in the SOC Level 1 path on TryHackMe.  
The purpose of this lab is to demonstrate practical analysis of a suspicious email reported by an employee.  
The investigation focuses on header forensics, sender validation, attachment inspection, and identification of malicious indicators.

The analysis included:

- Inspecting the raw email headers to identify sender infrastructure
- Extracting and validating Return-Path, From, Reply-To, and Originating IP fields
- Performing OSINT and WHOIS lookups on the sending IP address
- Verifying SPF and DMARC records for the Return-Path domain
- Examining the suspicious file attachment (CAB/RAR archive)
- Computing cryptographic hashes and cross-checking them on VirusTotal
- Identifying Indicators of Compromise (IoCs)

This case replicates typical SOC L1 triage workflow for suspicious inbound emails, demonstrating skills required for real-world phishing investigations.

## Environment

- **Platform:** TryHackMe  
- **Room:** Email Header Analysis (SOC Level 1 Path)  
- **Operating System:** Linux VM (Thunderbird + CLI Tools)


### 1. What is the Transfer Reference Number listed in the email's Subject?

**Method:**  
Opened the phishing email in Thunderbird and inspected the **Subject** line as well as the reference value shown inside the email body. Identified the numeric transfer reference included by the sender.

<img width="652" height="27" alt="image" src="https://github.com/user-attachments/assets/a7c1dbc0-569c-4004-b86e-5a4b9b7ecf60" />

**Answer:**  
09674321

### 2. Who is the email from?

**Method:**  
Reviewed the **From** field in Thunderbird to identify the display name associated with the sender.

<img width="469" height="54" alt="image" src="https://github.com/user-attachments/assets/ce5298ab-03c5-4d65-a137-59af3d8f5320" />

**Answer:**  
Mr. James Jackson

### 3. What is his email address?

**Method:**  
Extracted the sender’s address from the **From** header field displayed in Thunderbird.

**Answer:**  
info@mutawamarine.com

### 4. What email address will receive a reply to this email?

**Method:**  
Checked the **Reply-To** header field, which defines the address that will receive responses instead of the address shown in the “From” field.

<img width="449" height="25" alt="image" src="https://github.com/user-attachments/assets/e6595ee6-42e4-43e2-8db7-52990c15eeaf" />

**Answer:**  
info.mutawamarine@mail.com

### 5. What is the Originating IP?

**Method:**  
Opened the full email headers in Thunderbird (**More → View Source**) and parsed the earliest valid `Received:` line, which indicates the true Originating IP used by the sender.  
Identified the IPv4 address present in the SMTP handshake line.

<img width="1010" height="383" alt="image" src="https://github.com/user-attachments/assets/6e21ff64-9b21-4846-9b29-164569bc2869" />

<img width="823" height="650" alt="image" src="https://github.com/user-attachments/assets/77dfc77e-8cc3-410a-9fe4-a4efa0a6bd4b" />

**Answer:**  
192.119.71.157

### 6. Who is the owner of the Originating IP? (Do not include the "." in your answer.)

**Method:**  
Performed a WHOIS lookup on the originating IP **192.119.71.157** using VirusTotal’s *Details → Network → Whois Lookup*.  
Reviewed the allocation information for the IP block to identify the organization that owns the network range.

<img width="1758" height="422" alt="image" src="https://github.com/user-attachments/assets/e9a9b90a-c482-45e4-b69c-6d8dbc3257f3" />

<img width="1006" height="458" alt="image" src="https://github.com/user-attachments/assets/09a2d10e-d38f-4da0-850b-028a9403d218" />

The whois lookup:

- **OrgName:** Hostwinds LLC  
- **NetType:** Direct Allocation  
- **CIDR:** 192.119.64.0/18  
- **Country:** US  

This confirms that the IP belongs to **Hostwinds LLC**, a US hosting provider often abused for malicious or low-reputation activity.

**Answer:**  
Hostwinds LLC

### 7. What is the SPF record for the Return-Path domain?

**Method:**  
Extracted the **Return-Path** domain from the email headers (from `info@mutawamarine.com`).  
Performed an SPF (TXT) lookup for the domain using both CLI and a DNS analysis tool.

Command used:

dig TXT mutawamarine.com

<img width="1083" height="604" alt="image" src="https://github.com/user-attachments/assets/932e9881-7711-4da5-94fa-8687dab002be" />

**Answer**
v=spf1 include:spf.protection.outlook.com -all

### 8. What is the DMARC record for the Return-Path domain?

**Method:**  
Here we will usse tool mxtoolbox we could also do it as in previous task in terminal jusst by adding _dmarc. in front of domain name but for the purpousse of this portfolio we will also use the other way 

<img width="1588" height="804" alt="image" src="https://github.com/user-attachments/assets/3fc312de-8766-4f0a-b494-122200040cdf" />

**Answer**
v=DMARC1; p=quarantine; fo=1

### 9. What is the name of the attachment?

**Method:**  
Opened the email in **Thunderbird** and inspected the attachment pane displayed below the message body.  
Thunderbird shows the attachment exactly as it appears in the MIME structure, allowing direct extraction of the filename.

<img width="475" height="84" alt="image" src="https://github.com/user-attachments/assets/3e9aaff0-b26b-4af3-8f11-b2f42988d539" />


**Answer:**  
SWT_#09674321___PDF__.CAB

### 10. What is the SHA256 hash of the file attachment?

**Method:**  
Downloaded the attachment from Thunderbird to the analysis VM.  
Used the Linux `sha256sum` utility to compute the cryptographic hash of the file.

Command executed:

bash
sha256sum SWT_#09674321___PDF__.CAB

<img width="734" height="170" alt="image" src="https://github.com/user-attachments/assets/41f05ee4-82e8-47a3-bf6b-7945c48f20d1" />

### 11. What is the attachment’s file size? (NUM KB)

**Method:**  
Uploaded the attachment to **VirusTotal** to verify its metadata.  
Located the file size under **Details → Basic Properties → File size**.

<img width="1759" height="842" alt="image" src="https://github.com/user-attachments/assets/56477efd-8061-41bd-bd69-cab6523996f1" />

**Answer:**  
400.26 KB

### 12. What is the actual file extension of the attachment?

**Method:**  
Although Thunderbird displays the attachment as a **.CAB** file, a deeper inspection was required.  
Uploaded the file to **VirusTotal** and reviewed the **Details → Basic Properties** section.

Multiple fields (`File type`, `Magic`, `TrID`, `Magika`) all identify the file as a **RAR archive**.

This confirms the **real extension** is not `.CAB` but **`.RAR`** — a common obfuscation trick used in phishing/malware delivery.

<img width="874" height="552" alt="image" src="https://github.com/user-attachments/assets/c023ace4-ba27-4bdf-b838-bca89f4592e0" />

**Answer:**  
RAR















