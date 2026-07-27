# Finding 2 – Attacker User-Agent

## Finding 2 — Attacker User-Agent

### Objective

Identify the User-Agent string used by the attacker during HTTP communication and determine what it reveals about the client software and operating environment.

***

### Investigation Steps

1. Applied the Wireshark filter:

```
http.user_agent
```

2. Reviewed all HTTP requests containing User-Agent headers.
3. Compared the observed User-Agent values across the captured traffic.
4. Identified the unique User-Agent string used throughout the HTTP communication.

***

### Evidence

#### Observed User-Agent String

```
Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
```

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

<figure><img src="../.gitbook/assets/fig 2 User agent.png" alt=""><figcaption></figcaption></figure>

```
Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
```

This identifies the client as a **Mozilla Firefox 115.0 browser running on a 64-bit Linux operating system using the Gecko browser engine**.

The presence of a standard browser User-Agent suggests that the attacker interacted with the target web application using a conventional web browser during the observed activity.

However, User-Agent values are supplied by the client and can be modified or spoofed. Therefore, this information provides insight into the software identity presented during communication but does not independently prove the attacker's actual system configuration.

***

### Conclusion

The investigation identified the following User-Agent as the only value observed within the captured HTTP traffic:

**Mozilla/5.0 (X11; Linux x86\_64; rv:109.0) Gecko/20100101 Firefox/115.0**

This indicates that the client presented itself as a Firefox 115.0 browser operating on a Linux x86\_64 system.
