# TryHackMe – Windows Forensics Challenge

## Practical Lab Documentation

This repository contains my hands-on forensic investigation of a compromised Windows Server host provided in the TryHackMe room.  
The goal of this lab is to practice foundational SOC and DFIR skills: host triage, artifact examination, privilege auditing, scheduled task analysis, and identifying malicious activity on Windows systems.

---

## Environment

- **Platform:** TryHackMe  
- **Room:** Windows Forensics Challenge  
- **Operating System:** Windows Server 2016 Datacenter  

---

## Core Concepts Practiced

- Enumerating Windows system information and versioning  
- Auditing user logon activity and account privileges  
- Reviewing Windows Event Logs (Security, System)  
- Inspecting Scheduled Tasks for persistence  
- Identifying suspicious network connections and ports  
- Analyzing attacker behaviour and timeline reconstruction  
- Detecting indicators of compromise on a Windows host  

---

## Scenario (Background)

The target VM represents a Windows Server system that was previously compromised.  
The objective is to investigate system artifacts, reconstruct the attacker’s actions, and identify all relevant indicators.

This includes:

- Inspecting Windows OS metadata and configuration  
- Reconstructing user activity (logon events, timestamps)  
- Detecting privilege escalation and admin group membership changes  
- Identifying persistence mechanisms (scheduled tasks, scripts)  
- Reviewing malicious network activity or C2 communication  
- Establishing an incident timeline  

This documentation reflects the exact steps performed during the investigation.

---

## Tasks & Solutions

### **1. What’s the version and year of the Windows machine?**

#### **Method**

To determine the OS version, two verification methods were used:

1. **Command-line inspection**  
   Executed the `winver` command from an elevated Command Prompt to display OS edition and version details.

   Screenshot:  
   <img width="970" height="505" alt="image" src="https://github.com/user-attachments/assets/f6d769cc-0193-4247-8bee-a7bf13d291d5" />


2. **GUI navigation**  
   Navigated to:  
   `Control Panel → System and Security → System`  
   This provides full system metadata including edition, processor type, and activation information.

   Screenshot:  
   <img width="1382" height="802" alt="image" src="https://github.com/user-attachments/assets/b3da7750-5592-4c0a-bd16-d7a613dadd09" />


Both methods confirm the same Windows version.

#### **Answer**  
**Windows Server 2016**

---
### **2. Which user logged in last?**

#### **Method**

To identify the last logged-in user, the **Windows Event Viewer** was examined:

1. Navigated to:  
   `Event Viewer → Windows Logs → Security`

2. Applied a filter on the **Security** log using the **Event ID: 4624**  
   (Successful Logon events).  
   This event contains fields such as:
   - Account Name  
   - Account Domain  
   - Logon Type  
   - Logon Timestamp  

   Screenshot:  
   <img width="539" height="547" alt="image" src="https://github.com/user-attachments/assets/d318006a-d086-4543-ae98-1493fc5a44f3" />


3. Reviewed the most recent **4624** event at the top of the filtered list.  
   The “New Logon” section indicates which account successfully authenticated last.

   Screenshot:  
   <img width="1230" height="645" alt="image" src="https://github.com/user-attachments/assets/ee822170-8325-4971-9431-977a1850f646" />


#### **Answer**  
**Administrator**

---

### **3. When did John log onto the system last?**

#### **Method**

To determine the last logon timestamp for the user **John**, two independent verification methods were used.

---

### **Method 1: Command Line (net user)**

Used the built-in Windows command:

`net user John`

This command displays detailed account attributes, including the **Last logon** field.

Screenshot:  
<img width="979" height="506" alt="image" src="https://github.com/user-attachments/assets/4a89898d-7075-4ec7-9f33-5d2f2c8cc82b" />


The output shows:

**Last logon: 3/2/2019 5:48:32 PM**

---

### **Method 2: Event Viewer (Event ID 4624)**

1. Opened **Event Viewer** → `Windows Logs → Security`
2. Sorted events by newest first
3. Used **Find** to search for “John”
4. Reviewed the corresponding **Event ID 4624** entry for the timestamp

Screenshot:  
<img width="1189" height="750" alt="image" src="https://github.com/user-attachments/assets/b5612c3d-52da-4f5b-8173-4d6f787d0a99" />


The timestamp corresponds to John’s most recent successful logon.

---

#### **Answer**  
**03/02/2019 5:48:32 PM**

### **4. What IP does the system connect to when it first starts?**

#### **Method**

To identify outbound network connections initiated automatically at startup, the **System Information** tool was examined:

1. Opened:  
   `System Information → Software Environment → Startup Programs`
   
2. Reviewed all startup entries to identify any commands or executables that reference an external IP address.

3. Among the listed startup items, one suspicious entry was found:

- **Program:** UpdateSvc  
- **Command:** `c:\tmp\p.exe -s \\10.34.2.3 "net ..."`  
- **User:** Public  
- **Location:** HKLM\SOFTWARE\...

Screenshot:  
<img width="1146" height="334" alt="image" src="https://github.com/user-attachments/assets/6b72c33e-bd6c-4ce2-b94b-65a7f50b7403" />


This entry clearly shows that the system is attempting to initiate a connection to **10.34.2.3** during startup.  
Other startup entries do not contain any network-related commands or external IPs.

---

#### **Answer**  
**10.34.2.3**

### **5. What two accounts had administrative privileges (other than the Administrator user)?**

#### **Method**

To identify which users belong to the **local Administrators group**, the following command was executed in Command Prompt:

`net localgroup "Administrators"`

This command enumerates all members of the Administrators group, including built-in and manually added accounts.

Screenshot:  
<img width="970" height="504" alt="image" src="https://github.com/user-attachments/assets/25ffa02b-7ef9-4c66-ba18-038294000cfa" />


The output reveals three accounts with administrative privileges:

- Administrator  
- Guest  
- Jenny  

Since the question excludes the built-in **Administrator** account, the remaining two users with elevated privileges are:

- **Guest**  
- **Jenny**

---

#### **Answer**  
**Guest, Jenny**

### **6. What is the name of the scheduled task that is malicious?**

#### **Method**

Opened **Task Scheduler** and reviewed tasks under:  
`Task Scheduler Library`

Several entries appeared suspicious, but one task stood out after inspecting its **Actions** tab.

The task named **Clean file system** executes the following command:

`C:\TMP\nc.ps1 -l 1348`

Screenshot:  
<img width="1362" height="456" alt="image" src="https://github.com/user-attachments/assets/7ee5bb32-128f-4c6f-b77b-12d88c4fbaff" />


The referenced script **nc.ps1** was opened in PowerShell ISE for validation.  
Its content shows it is **PowerCat**, a PowerShell implementation of netcat, often used for establishing listeners or backdoors.

Screenshot:  
<img width="1268" height="427" alt="image" src="https://github.com/user-attachments/assets/740224cd-ed32-4822-ba6b-e285e57e6877" />


This confirms that **Clean file system** is the malicious persistence mechanism.

---

#### **Answer**  
**Clean file system**

### **7. What file was the task trying to run daily?**

#### **Method**

Reviewed all scheduled tasks in:

`Task Scheduler → Task Scheduler Library`

Only two tasks were configured to run **daily**:

- **check logged in** → executes Internet Explorer (benign)
- **Clean file system** → suspicious task

Opening **Clean file system → Actions** shows that it launches the following script:

`C:\TMP\nc.ps1 -l 1348`

Screenshot:  
<img width="1165" height="480" alt="image" src="https://github.com/user-attachments/assets/c0a749f1-5819-4d8d-96a1-50d18b78b062" />

Since this is the only daily task executing a non-standard script, it is the malicious file referenced.

---

#### **Answer**  
**nc.ps1**






