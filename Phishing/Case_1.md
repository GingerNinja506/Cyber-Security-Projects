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

### 3. What is the redirection URL to the phishing page for the individual Zoe Duncan? (defanged format)

**Method:**  
Identified Zoe Duncan’s phishing email and extracted the HTML attachment (`Direct Credit Advice.html`).  
Viewed the file contents using:
cat Direct\ Credit\ Advice.html

<img width="1832" height="441" alt="image" src="https://github.com/user-attachments/assets/11d299fc-949e-40ca-9776-0adf564e110c" />


The HTML contained a `<meta http-equiv="refresh">` tag and a fallback hyperlink pointing to the phishing redirect URL.

<img width="1363" height="599" alt="image" src="https://github.com/user-attachments/assets/5263d296-a45e-4d16-8a23-6c572bf9bdfb" />

**Answer (defanged):**  
`hxxp://kennaroads[.]buzz/data/Update365/office365/40e7baa2f826a57fcf04e5202526f8bd/?email=zoe.duncan@swiftspend.finance&error`

### 4. What is the URL to the .zip archive of the phishing kit? (defanged format)

**Method:**  
Followed the redirection link associated with Zoe Duncan in the sandbox VM.  
The malicious HTML attachment forwarded the victim to a phishing landing page hosted on:

hxxp[://]kennaroads[.]buzz/data/Update365/office365/40e7baa2f826a57fcf04e5202526f8bd/enterpassword[.]php?93A39917635544516884ca137f3c3624d1bf46b9e7bbdba86884ca137f3c3624d1bf46b9e7bbdba86884ca137f3c3624d1bf46b9e7bbdba86884ca137f3c3624d1bf46b9e7bbdba86884ca137f3c3624d1bf46b9e7bbdba8&email=zoe[.]duncan@swiftspend[.]finance&error=

The page replicated Microsoft’s login portal and pre-filled the victim’s email (`zoe.duncan@swiftspend.finance`) to increase credibility.  
Password submission would POST credentials directly to the attacker-controlled backend.  
The page was served over **HTTP (Not Secure)**, confirming data exposure.

<img width="1846" height="892" alt="image" src="https://github.com/user-attachments/assets/4d47020e-5276-49dd-bed1-91b0627b5dd5" />


After confirming the credential-harvesting interface, the directory structure was enumerated by removing URL path segments.  

<img width="1257" height="54" alt="image" src="https://github.com/user-attachments/assets/98fa0a61-701a-42e3-9df9-20994bd6c3f2" />
<img width="391" height="69" alt="image" src="https://github.com/user-attachments/assets/e0f8d95a-6613-420c-82c7-7fb24977dfb1" />


Directory indexing was enabled on the server, exposing:
<img width="703" height="414" alt="image" src="https://github.com/user-attachments/assets/7c37d071-55c3-4272-ac6c-e004be7959a2" />

<img width="650" height="404" alt="image" src="https://github.com/user-attachments/assets/536f000e-d94d-4665-82d9-42f006e0e6a1" />

**Answer (defanged):**  
`hxxp://kennaroads[.]buzz/data/Update365.zip`

### 5.What is the SHA256 hash of the phishing kit archive?

Inside the directory, the phishing kit archive `Update365.zip` was publicly accessible.

The ZIP archive was downloaded and its integrity/hash was verified using:
sha256sum Update365.zip

<img width="788" height="98" alt="image" src="https://github.com/user-attachments/assets/4271fc47-6d72-4cbb-8722-be8771eaab7e" />

Result:

ba3c15267393419e0bc27b6252b86bb39b406ef300ae8a18fee4d16b19ac9686 Update365.zip

### 5. When was the phishing kit archive first submitted? (format: YYYY-MM-DD HH:MM:SS UTC)

**Method:**  
Computed the SHA-256 hash of the phishing kit (`Update365.zip`) and queried the hash on VirusTotal.  
Navigated to the **Details** tab and retrieved the timestamp listed under **First Submission**.

<img width="1910" height="660" alt="image" src="https://github.com/user-attachments/assets/38902e5b-7f36-4722-bbcd-9f6a8bdada3c" />

<img width="584" height="173" alt="image" src="https://github.com/user-attachments/assets/5d845288-c20a-46a8-933a-4c7cc28bb2e6" />

**Answer:**  
**2020-04-08 21:55:50 UTC**

### 6. What was the email address of the user who submitted their password twice?

**Method:**  
Opened the attacker’s credential log file at:

`hxxp://kennaroads[.]buzz/data/Update365/log.txt`

Reviewed each captured credential entry.  
Identified the victim whose email appeared twice with duplicate password submissions.

Both entries showed:

- identical email address  
- two separate timestamps  
- two captured password submissions

<img width="1190" height="374" alt="image" src="https://github.com/user-attachments/assets/0f95a749-0707-433b-ba04-d96a39733147" />

<img width="1087" height="762" alt="image" src="https://github.com/user-attachments/assets/0fd6e416-18bf-4b2a-95df-e2cc9df7a5b8" />

**Answer:**  
**michael.ascott@swiftspend.finance**

### 7. What was the email address used by the adversary to collect compromised credentials?

**Method:**  
Extracted the phishing kit (`Update365.zip`) and inspected the Office365 phishing workflow located in:

`/Update365/office365/Validation/`

Searched for hard-coded email receivers in PHP handler files.  
The file `submit.php` contained the variable responsible for sending stolen credentials to the adversary:


$send = "m3npat@yandex.com";.

<img width="1523" height="760" alt="image" src="https://github.com/user-attachments/assets/53977dc8-1f99-4990-beba-54afd5c831b4" />

### 8. The adversary used other email addresses in the obtained phishing kit. What is the email address that ends in "@gmail.com"?

**Method:**  
Examined auxiliary phishing modules included in the kit, specifically the Yahoo-based phishing variant located in:

`/Update365/office365/script.st`

Identified an alternative hard-coded credential receiver used for that phishing flow.  
The variable `$to` in the script pointed to an attacker-controlled Gmail address:

$to = "janestanner2299@gmail.com";

<img width="1567" height="783" alt="image" src="https://github.com/user-attachments/assets/5c6e7a33-6b0b-4370-a45f-c03863c506bb" />
























