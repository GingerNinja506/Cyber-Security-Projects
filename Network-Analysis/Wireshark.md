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

### 7. What is the TCP payload size?

#### Method  
Opened packet **38** and expanded the **Transmission Control Protocol** section.  
Located the field labeled **TCP payload**, which shows the size of the application data carried inside the TCP segment.

The payload size is displayed directly in parentheses next to the field name.

<img width="1048" height="229" alt="image" src="https://github.com/user-attachments/assets/47f0c939-d51d-481c-811a-86a9ff4cc499" />

#### Answer
424

### 8. What is the E-Tag value?

#### Method  
Opened packet **38** and expanded the **Hypertext Transfer Protocol** section.  
Scrolled through the HTTP response headers to locate the field **ETag**, which uniquely identifies the specific version of the returned resource.

The value is shown directly next to the `ETag:` header.

<img width="1051" height="231" alt="image" src="https://github.com/user-attachments/assets/d5a1fd84-78fd-4a5f-b3d8-df273b177983" />

#### Answer
9a01a-4696-7e354b00

### 9. Search the "r4w" string in packet details. What is the name of artist 1?

#### Method  
Used Wireshark’s **Find Packet** feature:

1. Opened `Exercise.pcapng`.
2. Pressed `Ctrl + F` and selected search type **String**, search in **Packet bytes / Packet details**.
3. Searched for the string `r4w`.  
4. The match appeared inside an HTTP response containing HTML:
   ```html
   painted by: <a href='artists.php?artist=1'>r4w8173</a>

<img width="1038" height="288" alt="image" src="https://github.com/user-attachments/assets/e7f973fe-da4a-4916-add9-853c87b6b662" />

#### Answer
r4w8173

### 10. Go to packet 12 and read the packet comments. What is the answer?

#### Method  
Opened packet **12** in `Exercise.pcapng` and reviewed the **Packet Comments** section.  
The comment instructed to extract the embedded JPEG file and compute its **MD5** hash.

Since this was the same image previously exported from packet **39765**, reused the already extracted file (`picture`) and calculated the MD5 hash using:

``bash
md5sum picture

<img width="738" height="524" alt="image" src="https://github.com/user-attachments/assets/f5ae06a5-5b98-46fe-86e8-43e4227dcba2" />

#### Answer
911cd574a42865a956ccde2d04495ebf

### 11. There is a ".txt" file inside the capture file. Find the file and read it; what is the alien's name?

#### Method  
Opened `Exercise.pcapng` and navigated to:  
`File → Export Objects → HTTP`.

In the HTTP object list:

- Applied a filter for `.txt`
- Identified the file **note.txt**
- Saved it to the Desktop
- Used the terminal to read its contents:

``bash
cat note.txt

<img width="561" height="563" alt="image" src="https://github.com/user-attachments/assets/c2cfb3a5-9dc5-4155-a50e-dacac5a43d8f" />
<img width="747" height="546" alt="image" src="https://github.com/user-attachments/assets/106aea23-4fbf-43e7-8973-fe8fdedaba75" />
<img width="1254" height="835" alt="image" src="https://github.com/user-attachments/assets/344e3a4c-0984-44e8-8baf-d8b7422cca49" />

#### Answer
PACKETMASTER

### 12. Look at the Expert Info section. What is the number of warnings?

#### Method  
Opened `Exercise.pcapng` and navigated to:  
`Analyze → Expert Information`.

Inside the **Expert Info Composite** window, reviewed the summary of detected issues.  
Under the **Severity: Warning** category, Wireshark displayed the total number of warnings generated by the malformed or unusual HTTP header fields.

<img width="565" height="488" alt="image" src="https://github.com/user-attachments/assets/65c6d905-17bb-43f2-9a39-bdd170cc0a55" />
<img width="1915" height="97" alt="image" src="https://github.com/user-attachments/assets/72178529-aed3-4a29-87d5-dd74f56a79e5" />


#### Answer
1636

### 13. Go to packet 33790, follow the HTTP stream, and review the responses.  
### What is the total number of artists?

#### Method  
Navigated to packet **33790** and selected:  
`Follow → HTTP Stream`.

The displayed HTML content represented a page listing all available artists.  
Inside the response body, the relevant section appeared as:

<img width="1911" height="917" alt="image" src="https://github.com/user-attachments/assets/c5c32c48-a77e-476c-acff-36f0fed2edc4" />

We can also extract this packet and download on local machine in this casse linux VM and we can see it like that:

<img width="1155" height="735" alt="image" src="https://github.com/user-attachments/assets/2781054d-579d-48b8-a5b0-b376f4471152" />

### Answer
3

























