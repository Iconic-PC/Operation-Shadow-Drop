# 📁 Attack Analysis

### Overview

The Attack Analysis section provides a comprehensive examination of the malicious activity identified during the investigation of the compromised web server. Packet captures, HTTP requests, TCP streams, and reverse shell traffic were analyzed to reconstruct the attacker's actions from initial reconnaissance through post-exploitation and attempted data exfiltration.

The investigation revealed that the attacker successfully exploited a vulnerable web application, uploaded a malicious PHP web shell, established an interactive reverse shell, executed reconnaissance commands on the compromised host, and attempted to transfer sensitive information from the server to an external system. Network analysis confirmed communication over both standard web services (HTTP/HTTPS) and a non-standard port used for command-and-control (TCP 8080).

This section correlates network evidence with attacker behavior to reconstruct the complete attack lifecycle.

***

### Objectives of the Analysis

The primary objectives of this analysis were to:

* Reconstruct the complete attack sequence from network traffic.
* Identify the attacker's reconnaissance techniques.
* Determine how initial access was obtained.
* Analyze the web shell upload and execution process.
* Investigate reverse shell communication.
* Identify commands executed after system compromise.
* Detect evidence of data collection and attempted exfiltration.
* Map attacker behavior to the MITRE ATT\&CK framework.
* Assess the overall impact of the incident.

***

### Summary of Findings

The investigation identified several distinct stages of the intrusion:

| Attack Phase        | Observation                                                                                                     |
| ------------------- | --------------------------------------------------------------------------------------------------------------- |
| Reconnaissance      | Enumeration of web directories including `/admin/`, `/uploads/`, and `/reviews/uploads/`.                       |
| Initial Access      | Exploitation of a vulnerable upload functionality allowing execution of a malicious PHP file (`image.jpg.php`). |
| Execution           | Successful execution of the uploaded PHP web shell.                                                             |
| Command and Control | Reverse shell established over TCP port 8080.                                                                   |
| Discovery           | System reconnaissance using commands such as `whoami`, `uname -a`, `pwd`, `ls`, and `cat /etc/passwd`.          |
| Collection          | Enumeration of user accounts and server information.                                                            |
| Exfiltration        | Attempted transmission of system data using an HTTP POST request containing `/etc/passwd`.                      |

***

### Evidence Used

The conclusions presented in this section are supported by evidence collected from:

* Network packet capture (PCAP)
* HTTP request and response analysis
* TCP Stream reconstruction
* Endpoint command execution captured through the reverse shell
* File upload activity
* Directory enumeration attempts
* Command-and-control communication over TCP port 8080

***

### Structure of this Section

The remaining pages of this section provide a detailed breakdown of the incident:

* **Packet Analysis** – Detailed examination of the captured network traffic, HTTP requests, TCP sessions, and attacker communications.
* **Attack Timeline** – A chronological reconstruction of the attack from initial reconnaissance to attempted data exfiltration.
* **MITRE ATT\&CK Mapping** – Classification of attacker techniques using the MITRE ATT\&CK framework.
* **Incident Impact Assessment** – Evaluation of the attack's effect on system confidentiality, integrity, availability, and overall organizational risk.

***

#### Conclusion

The evidence demonstrates a complete web application compromise involving reconnaissance, exploitation of an upload vulnerability, deployment of a PHP web shell, establishment of an interactive reverse shell, internal system discovery, and attempted exfiltration of sensitive data. The following sections provide detailed technical analysis of each phase of the intrusion, supported by packet-level evidence and reconstructed attacker activity.
