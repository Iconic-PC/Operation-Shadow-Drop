# Incident Impact Assessment

### Overview

The investigation confirmed that the web server was successfully compromised after the attacker discovered an exposed upload directory and uploaded a malicious PHP web shell (`image.jpg.php`). The attacker then established an interactive remote shell over TCP port **8080**, executed reconnaissance commands, enumerated the operating system and users, and attempted to exfiltrate sensitive system information.

Although there is no evidence that the attacker obtained root privileges, the compromise demonstrates that the attacker achieved **remote code execution (RCE)** with the privileges of the web server (`www-data`). This level of access is sufficient to steal sensitive web application data, modify website content, upload additional malware, and use the compromised server as a pivot point for further attacks.

***

### Confidentiality Impact

**Impact Level: High**

The attacker gained unauthorized access to sensitive information, including:

* `/etc/passwd`
* System username information
* Operating system details
* Directory structure
* Web application paths
* Upload directory contents

Evidence from the packet capture also shows an attempt to transmit `/etc/passwd` to a remote destination using HTTP.

Compromise of these files provides valuable reconnaissance information that can assist attackers in privilege escalation or future attacks.

***

### Integrity Impact

**Impact Level: High**

The attacker successfully uploaded a malicious PHP web shell into:

```
/var/www/html/reviews/uploads/image.jpg.php
```

This demonstrates that unauthorized files could be written to the web server.

Potential consequences include:

* Website defacement
* Modification of application files
* Installation of backdoors
* Malware deployment
* Persistent remote access

***

### Availability Impact

**Impact Level: Medium**

No evidence suggests the attacker attempted to:

* Encrypt files
* Delete data
* Launch a denial-of-service attack

However, the compromised server could easily have been used to:

* Exhaust server resources
* Deploy ransomware
* Interrupt normal web services

Availability remained largely unaffected during this investigation.

***

### Privilege Assessment

The attacker obtained a shell running as:

```
www-data
```

Evidence:

```
whoamiwww-data
```

Although root privileges were not obtained, web server privileges allowed execution of arbitrary Linux commands.

***

### Persistence Assessment

Evidence indicates the uploaded PHP web shell was intended to maintain persistent access.

Indicators include:

* Upload of `image.jpg.php`
* Repeated access to `/reviews/uploads/`
* Interactive command execution through the uploaded script

This provided the attacker with a reusable remote access mechanism.

***

### Attack Severity

| Category                  | Assessment   |
| ------------------------- | ------------ |
| Initial Access            | Successful   |
| Web Shell Upload          | Successful   |
| Remote Command Execution  | Successful   |
| System Enumeration        | Successful   |
| Sensitive File Access     | Successful   |
| Privilege Escalation      | Not Observed |
| Data Exfiltration Attempt | Observed     |
| Service Disruption        | Not Observed |

***

### Overall Risk Rating

**Critical**

The incident should be classified as **Critical** because the attacker successfully:

* Exploited the vulnerable web application
* Uploaded a malicious PHP web shell
* Achieved remote command execution
* Enumerated the compromised Linux server
* Accessed sensitive system information
* Attempted data exfiltration
* Established persistent remote access through the uploaded shell

While there is no evidence of full system takeover or privilege escalation, compromise of the web application and execution of arbitrary commands represent a severe security incident requiring immediate containment, eradication, and remediation.
