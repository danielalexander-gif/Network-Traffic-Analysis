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
> *[<img width="1920" height="1080" alt="Screenshot (50)" src="https://github.com/user-attachments/assets/aed5665b-9baa-43f7-8ed6-3098d1ae26ab" />
]*

**Remediation & Escalation:**
1. **Credential Reset:** Force an immediate password reset for the user operating at the Source IP.
2. **Server Configuration:** Escalate to Network Engineering to disable Port 21 on the Destination server and implement secure alternatives like SFTP (Port 22).
3. **Firewall Policy:** Implement a blanket block on Port 21 across the corporate firewall to physically prevent future cleartext protocol violations.

---
