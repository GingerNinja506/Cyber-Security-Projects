# TryHackMe – SOC Level 1

## Wireshark The Basics – Practical Lab Documentation

This repository contains my hands-on work from the “Wireshark: Traffic Analysis” lab (SOC Level 1 path, TryHackMe).  
The goal of this lab is to develop practical packet-analysis skills: navigating PCAP files, extracting artefacts, decoding protocols, identifying suspicious traffic patterns, reviewing protocol statistics, and retrieving hidden data embedded in network captures.

---

## Environment
- **Platform:** TryHackMe  
- **Room:** Wireshark  
- **Operating System:** Linux VM  

---

## Core Concepts Practiced
- Packet capture structure and metadata analysis  
- Reading capture comments and file-level annotations  
- Navigating packets and protocols (Ethernet / IP / TCP / UDP / DNS / HTTP)  
- Applying display filters for targeted investigation  
- Extracting files from PCAP (Export Packet Bytes / Export Objects)  
- Identifying indicators in network traffic  
- Using Wireshark statistics to summarise traffic scope  

---

## Scenario (Background)

The lab provides multiple PCAP files designed to practice foundational SOC tasks.  
The objective is to familiarise with real-world packet analysis workflows:

- Inspecting capture metadata  
- Reviewing packet sequences and protocol details  
- Extracting embedded objects from traffic  
- Computing cryptographic hashes for validation  
- Identifying anomalies and flagged data inside packet comments  

This documentation reflects the exact steps performed during the analysis.

---

## Tasks & Solutions

### **1. Read the capture file comments. What is the flag?**

#### Method  
Opened the PCAP file `Exercise.pcapng` and navigated to:  
`Statistics → Capture File Properties → Comments`.  
Reviewed the **Capture File Comments** section containing author-embedded notes.

<img width="1833" height="949" alt="image" src="https://github.com/user-attachments/assets/b521f81c-ad15-4950-9491-af3bc99ca0f9" />

#### Result  


