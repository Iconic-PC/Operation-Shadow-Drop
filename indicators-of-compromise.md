# 📁 Indicators of Compromise

### Overview

Indicators of Compromise (IoCs) are forensic artifacts that provide evidence of malicious activity within an environment. During the investigation of **Operation Shadow Drop**, several network, host, and application-based indicators were identified that can assist security teams in detecting similar attacks, conducting threat hunting activities, and preventing future compromise.

The indicators documented in this section were extracted from packet captures, network communications, HTTP requests, and system interactions observed throughout the investigation.

These IoCs should be incorporated into security monitoring platforms such as SIEM, IDS/IPS, EDR, and Web Application Firewalls (WAFs) to improve detection capabilities and support proactive threat hunting.

### Network Indicators

| Indicator      | Value             | Description                                                                              |
| -------------- | ----------------- | ---------------------------------------------------------------------------------------- |
| Source IP      | **117.11.88.124** | Attacker system initiating reconnaissance, exploitation, and web shell access.           |
| Destination IP | **24.49.63.79**   | Target web server.                                                                       |
| HTTP Port      | **80/TCP**        | Used for initial reconnaissance, directory enumeration, and web application interaction. |
| HTTPS Port     | **443/TCP**       | Used during attempted outbound communication and possible data exfiltration.             |
| Service Port   | **8080/TCP**      | Interactive communication channel used after web shell compromise.                       |

***

### Suspicious Directories Accessed

The attacker enumerated multiple directories during reconnaissance, including:

* /
* /products/
* /about/
* /reviews/
* /admin/
* /uploads/
* /admin/uploads/
* /reviews/uploads/

***

### Suspicious Files Accessed

The attacker requested several files while browsing the web server, including:

* /icons/blank.gif
* /icons/back.gif
* /icons/image2.gif
* /products/images/product1.jpg
* /products/images/product2.jpg

***

### Malicious File

The investigation identified an uploaded PHP web shell:

* **/reviews/uploads/image.jpg.php**

This file enabled remote command execution on the compromised server.

***

### Host Indicators

Following successful exploitation, the attacker executed several reconnaissance commands, including:

* `whoami`
* `uname -a`
* `pwd`
* `ls /home`
* `cat /etc/passwd`

These commands were used to identify the current user, operating system, filesystem structure, and local user accounts.

***

### Compromised Account

The attacker obtained command execution as:

* **www-data**

This account represents the Apache web server service account.

***

### Targeted Sensitive File

The attacker attempted to access:

* **/etc/passwd**

This Linux system file contains local account information and is commonly targeted during post-exploitation reconnaissance.

***

### Attack Characteristics

Observed attacker behavior included:

* Web directory enumeration
* File upload abuse
* PHP web shell execution
* Interactive remote command execution
* System reconnaissance
* Attempted data exfiltration
* Persistent communication over TCP port 8080

***

### Detection Recommendations

Security teams should monitor for:

* PHP files executing from upload directories
* Requests targeting administrative or upload folders
* Access to sensitive Linux files such as `/etc/passwd`
* Unexpected outbound connections from web servers
* Repeated HTTP GET requests to hidden or restricted directories
* Long-lived interactive sessions over uncommon application ports such as TCP 8080

These indicators should be incorporated into SIEM correlation rules, IDS/IPS signatures, endpoint detection policies, and threat hunting playbooks to improve early detection of similar attacks.
