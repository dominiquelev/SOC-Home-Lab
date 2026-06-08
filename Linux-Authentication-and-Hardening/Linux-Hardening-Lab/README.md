# LINUX HARDENING & SSH AUTHENTICATION MONITORING

## 📌 1. Project Objective
The objective of this lab was to learn about linux hardening for beginner build and monitoring authentication log with openssh with Ubuntu and Kali Linux server inside a VirtualBox virtualized lab.

focused on :
- configuring ssh remote access between ubuntu server and kali linux
- understand the communication between client and server
- monitoring SSH authentication log activity
- observing live SSH authentication log 
- building foundational linux hardening knowledge for future SIEM and SOC analysis project
---

## ⚙️ 2. Lab Specifications & Tools

* **Hypervisor / Platform:** Oracle VM VirtualBox 
* **Operating System(s):**
  - Kali Linux (Client Macchine)
  - Ubuntu Server (Server Machine)
* **Security Tools Used:**
  - OpenSSH Server
  - Linux Terminal
  - Journalctl

### Hardware Resource Profiles:


| Component | Allocation | Purpose |
| :--- | :--- | :--- |
| **Memory (RAM)** | 4096 MB | Prevents application lag and data drops during active packet capture processing. |
| **Processors** | 2 vCPUs | Required for smooth real-time multi-threaded packet dissection and interface rendering. |
| **Network Mode** | Bridged Adapter | Allows the virtual machine to bypass NAT restrictions and sniff live local network interfaces. |


---

## ⚠️ 3. Engineering Challenges & Troubleshooting

### Incident / Roadblock: 
Ensuring proper visibility of both HTTP and HTTPS traffic and identifying the correct packets inside Wireshark.

* **The Problem:**
During the initial attempt to capture live HTTP and HTTPS traffic simultaneously, packet analysis became difficult because Wireshark displayed multiple protocols at the same time. This made it challenging to isolate and identify the required HTTP and HTTPS packets clearly.

To resolve this issue, the websites were tested one at a time instead of simultaneously.

Additional display filtering and packet inspection were required to:

Identify HTTP communication using GET requests
Verify successful HTTP traffic capture
Identify HTTPS communication using TLS handshake packets
Verify successful HTTPS communication through Client Hello and Server Hello messages

* **The Resolution Workflow:** 
  1. Open VirtualBox and Start Kali Linux.
  2. update and upgrade application on kali linux with  `sudo apt update` and       `sudo apt upgrade`
     <img src="Images/sudo-apt-update-and-upgrade.png" width="700">
  3. Relaunch Wireshark using:
    ```bash
      wireshark &
     ```
  4. Confirmed that the `eth0` interface appeared correctly and verified that live network traffic could be captured successfully.

     <img src="Images/eth0-interface.png" width="700">
     
  5. Opened `neverssl.com` using command  `firefox http://neverssl.com &`

     <img src="Images/http-website-open.png" width="700"> 
       
     to generate and capture live network traffic using Wireshark
  
  6. Applied the `http.request` display filter in Wireshark to verify that HTTP     packets were captured successfully .
     
    <img src="Images/http-filter.png" width="700">
       
  7. Inspected the HTTP GET request packet:
    GET / HTTP/1.1

    <img src="Images/http-get-request.png" width="700">

    to verify readable unencrypted HTTP communication.
  
  8. Opened github.com using command  `firefox https://github.com &`

    <img src="Images/https-website-open.png" width="700">
       
     to generate and capture live network traffic using Wireshark
     
  9. Applied the `tls` display filter in Wireshark to isolate encrypted HTTPS traffic
      
    <img src="Images/tls-filter.png" width="700">

  10. identified the TLS handshake packet by:
    - `client hello`
    <img src="Images/client-hello-github.com.png" width="700">
  
    - `server hello`
    <img src="Images/server-hello-github.com.png" width="700">

  to verify successful encrypted HTTPS communication between the client and GitHub servers.

  11. Exported the packet capture file generated during tcp-3-way-handshake for future investigation and traffic-review practice

  12. The packet capture file was saved as:
  - http-traffic-analysis.pcapng
  - https-traffic-analysis
  and store inside the `pcaps/` directory
  

---

## 📊 4. Practical Execution & Findings

* **Activity Executed:**
  - captured live network traffic generated from github.com
  - use command `firefox http://neverssl.com &` and `firefox https://github.com` to open examples website of HTTP and HTTPS
  - Applied `http.request` on display filter of Wireshark to isolate HTTP packets 
  - Applied `tls` on display filter of Wireshark to isolate HTTPS packets
  - inspect HTTP GET requests for HTTP packets and TLS handshake for HTTPS pakcets
  -compared traffic visibility between unencrypted HTTP and encrypted HTTPS communication

* **Key Observations:**
  - HTTP traffic generated from `neverssl.com` was fully readable inside Wireshark.
  - The HTTP capture display get requests, host information, and plain-text communication data
  - HTTPS traffic generated from `github.com` was encrypted and not directly readable inside ireshark
  - HTTPS analysis to revealed TLS handshake packet including CLient Hello, and Server Hello.
  - TLS traffic successfully demonstrated how HTTPS protects communication through encryption.
  - The comparison clearly demonstrated the security difference between HTTP and HTTPS communication.
---

## 🚀 5. Key Takeaways & Career Alignment
* **L1 SOC Skill Demonstrated:**
  - Basic packet capture and traffic analysis
  - HTTP and HTTPS Traffic inspection
  - Beginner-level wireshark filtering and packet analysis
  - network troubleshooting and interaface verification 
* **Next Steps:**
  - Continue building beginner SOC and network-analysis projects
## 🛠 Skills Practiced
  - VirtualBox networking
  - Wireshark packet capture
  - HTTP traffic analysis
  - HTTPS/TLS traffic analysis
  - Wireshark display filtering
  - Traffic inspection and protocol comparison
  - Technical documentation and reporting
