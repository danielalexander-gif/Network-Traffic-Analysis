# Network-Traffic-Analysis

# Network Protocol Auditing & Threat Analysis

## Phase 1: Legacy Protocol Vulnerability Audit (FTP)

**Scenario:**
Utilized a public packet capture as a simulated environment to demonstrate the auditing of corporate network segments for legacy protocol vulnerabilities. The objective was to identify unencrypted data transmissions and assess network policy compliance.

**Execution:**
* Analyzed network traffic using Wireshark to isolate unencrypted Application Layer communication.
* Applied the `ftp` display filter to strip background noise and specifically target Port 21 traffic.
* Reconstructed the packet sequence using the "Follow TCP Stream" function to inspect the raw data exchange between the client and server.

**Findings:**
* **Cleartext Credentials Intercepted:** Successfully extracted the username (`anonymous`) and password (`IEUser@`) transmitted in plain text, proving the risk of unencrypted data transfer.
* **Reconnaissance Data Leaked:** Identified the server banner as `NetBSD-ftpd 20041119`. This exposes the exact operating system and software version, demonstrating how misconfigured servers assist attackers in OS fingerprinting and vulnerability mapping.
* **Incident Scope:**
  * **Source IP (Client):** `2002:5183:4383::5183:4383`
  * **Destination IP (Server):** `2001:638:902:1:201:2ff:fee2:7596`

> **Evidence:**  
> <img width="1920" height="1080" alt="Screenshot (51)" src="https://github.com/user-attachments/assets/1c6a7f92-3d74-4a42-b72c-ee8731c7220a" />



**Remediation & Escalation:**
1. **Credential Reset:** Force an immediate password reset for the user operating at the Source IP.
2. **Server Configuration:** Escalate to Network Engineering to disable Port 21 on the Destination server and implement secure alternatives like SFTP (Port 22).
3. **Firewall Policy:** Implement a blanket block on Port 21 across the corporate firewall to physically prevent future cleartext protocol violations.

---
## Phase 2: Cleartext Payload Delivery & IOC Extraction
Objective: Analyze unencrypted HTTP traffic to demonstrate the foundational mechanics of malware delivery, exploit kit redirects, and payload extraction.

Victim Host Profiling
Before analyzing the payload delivery, the compromised machine was identified on the network using DHCP and NBNS traffic analysis.

IP Address: 172.16.165.165

MAC Address: f0:19:af:02:9b:f1

Hostname: K34EN6W3N-PC

1. Filtering for Drive-By Downloads
To isolate the infection vector from standard background network noise, the capture was filtered to display only outbound file requests initiated by the victim's host.

Query: http.request.method == "GET"

Finding: The victim's browser was subjected to an Exploit Kit redirect chain, seamlessly routing traffic from a legitimate domain (www.ciniholland.nl) through intermediate infrastructure (24corp-shop.com) to a hostile delivery server (stand.trustandprobaterealty.com).

2. Payload Identification
Using Wireshark's HTTP Object extraction capabilities, the hostile server's deliveries were cataloged. The attacker initially deployed Flash and Java files to exploit browser vulnerabilities before successfully pushing a malicious executable payload to the victim's machine.

Extracted IOC (Domain): stand.trustandprobaterealty.com

Extracted IOC (IP): 37.200.69.143

Payload Signature: Disguised as a randomized PHP session string (index.php?req=m&num...), the file's actual Content-Type was revealed as application/x-msdownload (Microsoft Executable).

[<img width="1920" height="1080" alt="Screenshot (54)" src="https://github.com/user-attachments/assets/99779f29-d801-4718-bacb-01f3f003e5b3" />
]

## Phase 3: OSINT Verification (Threat Intelligence)
Objective: Cross-reference extracted network indicators with global threat intelligence databases to confirm the malicious nature of the server.

1. Indicator Lifecycle Analysis
The extracted domain (stand.trustandprobaterealty.com) and associated IP address (37.200.69.143) were queried against VirusTotal.

IP Reputation Decay: The IP address returned a clean reputation, effectively demonstrating indicator aging. As malicious infrastructure is routinely burned and IP addresses are reassigned to legitimate hosts, IP-based IOCs have a highly limited shelf-life.

Domain Verification: The domain name retained its historical threat data, with multiple security vendors successfully flagging it as a malicious malware distributor, confirming the initial Wireshark triage.

[<img width="1920" height="1080" alt="Screenshot (56)" src="https://github.com/user-attachments/assets/b8778aec-606d-4d55-aa44-78bb9f26fba6" />
]
