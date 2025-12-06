# Project 10 – Network Traffic Analysis (Wireshark)

## Overview
In this project, I used Wireshark inside a Windows VM to capture and analyze different types of network traffic. The goal was to simulate activity that a SOC analyst investigates every day, including beaconing, DNS lookups, HTTP requests, and encrypted TLS traffic. This project helped me practice spotting suspicious patterns and understanding how normal and abnormal traffic behaves on a network.

---

## Tools Used
- Windows 10 Virtual Machine
- Wireshark 4.6.2
- Npcap (packet capture driver)
- Command Prompt

---

## Traffic Types Captured

### 1. ICMP Beaconing (Suspicious)
I ran `ping 8.8.8.8 -t` to create repetitive ICMP traffic.  
In Wireshark, I filtered using `icmp`.

**What I saw:**
- Continuous Echo Request and Echo Reply packets  
- Packet lengths stayed the same (74 bytes)  
- Timing was almost perfectly spaced (about 1 second apart)

**Why this matters:**
This pattern is similar to malware “beaconing” to a command-and-control server.  
Consistent timing and repetition are strong indicators of automated communication.

---

### 2. DNS Traffic
I triggered DNS queries using `nslookup google.com` and filtered with `dns`.

**What I saw:**
- Standard DNS queries and Standard DNS responses  
- A-record lookups requesting IP addresses  
- DNS responses containing the resolved IP

**Why this matters:**
Malware often relies on DNS to locate its command servers.  
Analysts look for high-frequency queries, strange domains, or encoded data.  
This helps detect DNS tunneling or malware contacting suspicious domains.

---

### 3. HTTP Traffic
I created HTTP traffic using:

Filtered with: `http`.

**What I saw:**
- HTTP GET request  
- HTTP 200 OK response  
- Visible HTML content in the packet body

**Why this matters:**
Clear-text traffic is easy to analyze.  
Many beginner-level malware samples still use unencrypted HTTP for simplicity.

---

### 4. TLS Encrypted Traffic
I filtered encrypted traffic using:

and:


**What I saw:**
- Client Hello / Server Hello messages  
- TLSv1.2 and TLSv1.3  
- Encrypted Application Data  
- SYN, ACK, FIN, and RST packets  

**Why this matters:**
Most modern malware uses HTTPS (TLS) to hide its command-and-control traffic.  
Since the payload is encrypted, analysts rely on metadata like:
- Destination IP  
- Packet timing  
- JA3 fingerprints  
- Unusual connections  
- Repetition patterns  

This is exactly how encrypted C2 channels are detected.

---

## Key Findings

- ICMP traffic showed a classic beaconing pattern  
- DNS queries behaved as expected and demonstrated domain resolution  
- HTTP traffic was captured and readable  
- TLS traffic showed how encrypted sessions are established and maintained  
- Packet metadata is extremely important for identifying suspicious behavior when encryption is involved  

---

## What I Learned
- How to capture live network traffic inside a VM  
- How to filter and isolate specific protocols  
- How to recognize repetitive beaconing patterns  
- How encrypted C2 traffic hides inside normal web traffic  
- Why SOC analysts depend heavily on metadata when payloads are encrypted  

---

## Evidence Screenshots
(Here is where you’ll insert the screenshots you took:)
- ICMP beaconing  
- DNS queries  
- HTTP GET/200 OK  
- TLS handshake + encrypted packets  
- TCP port 443 view  

---

## Conclusion
This project helped me practice real-world network analysis skills used in SOC environments. By triggering controlled traffic and analyzing it in Wireshark, I learned how to identify normal behavior and how suspicious patterns can stand out. These skills are important for detecting malware, command-and-control activity, and abnormal outbound connections in enterprise networks.
