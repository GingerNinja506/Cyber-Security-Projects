# TryHackMe – SOC Level 1

## Snort The Basics – Practical Lab Documentation

This repository contains my hands-on work from the **"Snort"** lab in the SOC 1 path on TryHackMe. The goal of this lab is to understand the function of an Intrusion Detection System (IDS), master the different modes of the Snort tool, and develop practical skills in writing custom rules for signature-based detection.

---

## 🛠️ Environment

* **Platform:** TryHackMe
* **Room:** Snort
* **Operating System:** Linux VM (Ubuntu)
* **Tool:** Snort (Intrusion Detection System)

---

## 💡 Core Concepts Practiced

* Understanding the distinction between **Firewall (Prevention)** and **IDS (Detection)**.
* Practical application of Snort's operational modes: **Sniffer**, **Logger**, and **NIDS/IPS**.
* Configuring Snort and working with rule files (`snort.conf`, `local.rules`).
* Mastering the **Snort Rule Syntax** (Action, Protocol, IP, Port, Options).
* Analyzing historical network traffic **PCAP** files using Snort rules (Forensics).

---

## 📝 Tasks & Solutions

### 1. Task 1: Run the Snort instance and check the build number.

#### Method

The Snort version and build information is accessed by running the `snort` command with the **uppercase `-V`** flag. This is a common practice to verify tool installation and compatibility before starting an analysis session. The output provides details about the Snort version, the associated build number, copyright information, and dependencies used (e.g., `libpcap`, `PCRE`, `ZLIB`).

**Command:**

```bash
snort -V
```
<img width="755" height="232" alt="image" src="https://github.com/user-attachments/assets/36e9f7d3-9aeb-4308-9f9e-632bdbfb80c3" />

## Answer
149


