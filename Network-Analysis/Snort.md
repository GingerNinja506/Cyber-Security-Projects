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

###  Task 1: Run the Snort instance and check the build number.

#### Method

The Snort version and build information is accessed by running the `snort` command with the **uppercase `-V`** flag. This is a common practice to verify tool installation and compatibility before starting an analysis session. The output provides details about the Snort version, the associated build number, copyright information, and dependencies used (e.g., `libpcap`, `PCRE`, `ZLIB`).

**Command:**

```bash
snort -V
```
<img width="755" height="232" alt="image" src="https://github.com/user-attachments/assets/36e9f7d3-9aeb-4308-9f9e-632bdbfb80c3" />

**Answer**
149

### Task 2: Test the current instance with "/etc/snort/snort.conf" file and check how many rules are loaded with the current build.

#### Method

To test the current Snort configuration and verify the number of rules loaded, we use the `snort` command with the **`-c`** flag to specify the configuration file path (`/etc/snort/snort.conf`) and the **`-T`** flag to enable **test mode**. This mode validates the configuration syntax and loads the rules without starting the sniffing engine. The output summary confirms the number of rules successfully read.

**Command:**

```bash
snort -c /etc/snort/snort.conf -T
```

<img width="745" height="329" alt="image" src="https://github.com/user-attachments/assets/ae828cba-013c-4d3c-a78a-2efaddb668a0" />

**Answer**
4151

### Task3: Test the current instance with "/etc/snort/snortv2.conf" file and check how many rules are loaded with the current build.

#### Method
To determine how many rules are loaded with the specific configuration file `snortv2.conf`, we execute Snort in test mode pointing to that file.

**Command:**

```bash
snort -T -c /etc/snort/snortv2.conf
```
<img width="705" height="293" alt="image" src="https://github.com/user-attachments/assets/9e0f165c-7a9a-408e-b4e5-acc59b425be3" />
**Answer**
1





