# TryHackMe – Linux Investigation Challenge
## Practical Lab Documentation

This repository contains my hands-on forensic investigation of a compromised Linux host provided in the TryHackMe environment. 
The goal of this lab is to practice foundational Linux DFIR (Digital Forensics and Incident Response) skills: analyzing system logs, identifying unauthorized access, uncovering persistence mechanisms, and reconstructing the attacker's activities.

## Environment
* **Platform:** TryHackMe
* **Room:** Linux Forensics
* **Operating System:** Ubuntu 20.04.6 LTS
* **Kernel Version:** 5.15.0-1077-aws
* **Architecture:** x86_64

## Core Concepts Practiced
* **System & OS Information:** Enumerating release details, user accounts (`/etc/passwd`), and sudoers.
* **System Configuration:** Auditing network interfaces, open ports (`netstat`), and running processes (`ps`).
* **Persistence Mechanisms:** Inspecting Cron jobs, systemd services, and shell startup scripts (`.bashrc`).
* **Evidence of Execution:** Analyzing command history (`.bash_history`) and authentication logs (`/var/log/auth.log`).
* **Log Analysis:** Reviewing syslog and third-party application logs for indicators of compromise (IOCs).

---

## Investigation Reference Tool
During this lab, the following **Linux Forensics Cheatsheet** provided by TryHackMe was used to identify critical file locations and forensic artifacts:

<img width="630" height="892" alt="image" src="https://github.com/user-attachments/assets/cb427c2a-9277-435d-bc7b-81cbebde72cc" />

---

## Tasks & Solutions

### 1. The user installed a package on the machine using elevated privileges. According to the logs, what is the full COMMAND?

#### Method
To identify commands executed with elevated privileges, I analyzed the authentication logs located in `/var/log`. 
1. Initially, I searched the current `auth.log` file using `grep sudo`, but found no relevant installation commands.
2. I then inspected the rotated log file `auth.log.1`, which often contains older session data.
3. By filtering for `sudo`, I identified a log entry where the user `cybert` used `sudo` to install a package via `apt`.

**Commands used:**
`cat /var/log/auth.log.1 | grep sudo`

**Screenshot:**
<img width="1325" height="434" alt="image" src="https://github.com/user-attachments/assets/eb9143ec-1da3-488d-8f01-0f144b405498" />

<img width="1343" height="43" alt="image" src="https://github.com/user-attachments/assets/5ebcb59e-cb88-4573-bf0b-9a05e2476bbe" />


#### Answer
/usr/bin/apt install dokuwiki
