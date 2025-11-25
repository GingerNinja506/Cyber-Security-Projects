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

#### Answer
TryHackMe_Wireshark_Demo 

### 2. What is the total number of packets?

#### Method  
Opened the file `Exercise.pcapng` and checked the status bar at the bottom of Wireshark.  
The **Packets** field displays the total number of captured packets.

<img width="1397" height="862" alt="image" src="https://github.com/user-attachments/assets/3be55a30-1939-4ee8-9fd5-08a8709ebbff" />

#### Answer  
58620
### 3. What is the SHA256 hash value of the extracted image?

#### Method  
The hint inside **Capture File Comments** pointed to packet **39765**, which contains a reassembled JPEG image.

Performed the following steps:

1. Navigated to packet **39765** in `Exercise.pcapng`.  
2. Expanded the packet details and located the **JPEG File Interchange Format** section.  
3. Right-clicked the packet → **Export Packet Bytes…**  
4. Saved the extracted file as `picture` on the Desktop.  
5. Calculated the SHA-256 hash using:
   ```bash
   sha256sum picture
<img width="1435" height="57" alt="image" src="https://github.com/user-attachments/assets/61673135-1e61-4ef9-9d66-bcb793cdf37e" />
<img width="988" height="744" alt="image" src="https://github.com/user-attachments/assets/8f68da11-11df-42e4-ae30-7ce3bdd39101" />
<img width="739" height="483" alt="image" src="https://github.com/user-attachments/assets/7fc5114d-9177-4232-b873-2eb6f397a275" />

#### Answer
99958e145afe69a59b03f53712e6084b788ec09bed4ad9094a941c0c438942a3

### 4. View packet number 38. Which markup language is used under the HTTP protocol?

#### Method  
Navigated to packet **38** in `Exercise.pcapng`.  
Expanded the protocol layers in the packet details pane.  
Under **Hypertext Transfer Protocol**, an additional data structure was present showing the markup used.

The final decoded layer was labeled **extensible Markup Language**, indicating XML content embedded in the HTTP response.

<img width="993" height="770" alt="image" src="https://github.com/user-attachments/assets/8d3313d5-711f-4ab4-93bb-4c5f019adf6d" />

#### Answer
eXtensible Markup Language

### 5. What is the arrival date of the packet? (Answer format: Month/Day/Year)

#### Method  
Opened packet **38** in `Exercise.pcapng`.  
Expanded the **Frame** section and located the field **Arrival Time**, which displays the full timestamp of when the packet was captured.

<img width="1339" height="383" alt="image" src="https://github.com/user-attachments/assets/964b2f63-61ed-4c49-a09f-f762387fca7c" />

#### Answer
05/13/2004

### 6. What is the TTL value?

#### Method  
Opened packet **38** and expanded the **Internet Protocol Version 4** section.  
Inside the IPv4 header fields, located the parameter **Time to live (TTL)**, which indicates the maximum number of router hops allowed before the packet is discarded.

The TTL value was clearly shown in the decoded IPv4 header.

<img width="1043" height="361" alt="image" src="https://github.com/user-attachments/assets/2d5ffc1f-228b-4a0a-81e9-e71d8aa03c52" />

#### Answer
47












