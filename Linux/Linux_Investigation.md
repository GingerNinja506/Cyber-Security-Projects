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
