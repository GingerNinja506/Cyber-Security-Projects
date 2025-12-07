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
   ![event-filter](./images/event_filter_4624.png)

3. Reviewed the most recent **4624** event at the top of the filtered list.  
   The “New Logon” section indicates which account successfully authenticated last.

   Screenshot:  
   <img width="1230" height="645" alt="image" src="https://github.com/user-attachments/assets/ee822170-8325-4971-9431-977a1850f646" />


#### **Answer**  
**Administrator**

---

