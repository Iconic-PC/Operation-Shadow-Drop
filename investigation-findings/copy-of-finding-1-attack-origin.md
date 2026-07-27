# Finding 2 – Attacker User-Agent

### Objective

Identify the User-Agent string used by the attacker during HTTP communication and determine what it reveals about the client software and operating environment.

***

### Investigation Steps

1. Applied the Wireshark filter:

_**http.user\_agent**_

2. Reviewed all HTTP requests containing User-Agent headers.
3. Compared the observed User-Agent values across the captured traffic.
4. Identified the unique User-Agent string used throughout the HTTP communication.

***

### Evidence

#### Observed User-Agent String

<figure><img src="../.gitbook/assets/fig 2 User agent.png" alt=""><figcaption></figcaption></figure>

```
Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
```

***

### Client Information Analysis

| Component        | Details         |
| ---------------- | --------------- |
| Operating System | Linux x86\_64   |
| Platform         | X11             |
| Browser          | Mozilla Firefox |
| Browser Version  | 115.0           |
| Browser Engine   | Gecko           |

***

### Analysis

Analysis of the captured HTTP traffic revealed a single User-Agent string:

```
Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
```

#### What the User-Agent Reveals

The User-Agent string reveals that the client presented itself as:

* **Browser:** Mozilla Firefox 115.0
* **Operating System:** Linux x86\_64
* **Browser Engine:** Gecko
* **Platform:** X11

This suggests that the attacker accessed the target web application using a Firefox browser running on a Linux system. No automated exploitation tool was identified from the User-Agent field during this stage of the investigation.

However, User-Agent values are controlled by the client and can be modified, meaning they should be treated as supporting evidence rather than absolute proof of the attacker's actual environment.
