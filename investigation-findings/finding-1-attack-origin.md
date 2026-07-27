# Finding 1 - Attack Origin

## Objective

Identify the source of the suspicious network activity and determine the registered geographic origin of the IP address initiating communication with the target web application.

***

### Investigation Steps

1. Opened the provided PCAP file in Wireshark.
2. Reviewed IPv4 conversations to identify active communication pairs.
3. Examined the initial HTTP requests exchanged between the communicating hosts.
4. Identified the source IP address initiating communication with the target web server.
5. Performed a WHOIS lookup and IP geolocation analysis to determine the registered ownership and geographic location of the source IP address.

***

### Evidence

#### Network Traffic Evidence

<figure><img src="../.gitbook/assets/Fig 1.1 Wireshark info on IP.png" alt=""><figcaption></figcaption></figure>

<table data-search="false"><thead><tr><th>Field</th><th>Value</th></tr></thead><tbody><tr><td>Source IP Address</td><td><strong>117.11.88.124</strong></td></tr><tr><td>Destination IP Address</td><td><strong>24.49.63.79</strong></td></tr><tr><td>Destination Port</td><td><strong>80 (HTTP)</strong></td></tr><tr><td>HTTP Method</td><td><strong>GET</strong></td></tr><tr><td>Requested Resource</td><td><strong>/</strong></td></tr><tr><td>Host Header</td><td><strong>shoporoma.com</strong></td></tr><tr><td>Packet Reference</td><td><strong>Frame 4</strong></td></tr></tbody></table>

***

#### WHOIS & IP Geolocation Analysis

<div><figure><img src="../.gitbook/assets/Fig 1.2 WHOis Lookup.png" alt="WHOIS"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Fig 1.3 IP Geolocation.png" alt=""><figcaption></figcaption></figure></div>

<table data-search="false"><thead><tr><th>Attribute</th><th>Value</th></tr></thead><tbody><tr><td>Source IP Address</td><td><strong>117.11.88.124</strong></td></tr><tr><td>City</td><td><strong>Tianjin</strong></td></tr><tr><td>Province</td><td><strong>Tianjin Municipality</strong></td></tr><tr><td>Country</td><td><strong>China (CN)</strong></td></tr><tr><td>Organization</td><td><strong>China Unicom</strong></td></tr><tr><td>Registered Network</td><td><strong>China Unicom Tianjin Province Network (UNICOM-TJ)</strong></td></tr><tr><td>Autonomous System (ASN)</td><td><strong>AS4837</strong></td></tr><tr><td></td><td></td></tr></tbody></table>

***

### Analysis

Analysis of the packet capture identified **117.11.88.124** as the source of the initial HTTP request directed to the target web application hosted at **24.49.63.79**.

Further investigation using WHOIS and IP geolocation identified the source IP address as belonging to the **China Unicom Tianjin Province Network**, with a registered geolocation of **Tianjin, Tianjin Municipality, China**.

The originating IP address represents the initial source of communication observed during the investigation and serves as the starting point for reconstructing the attacker's activities throughout the incident.

It should be noted that IP geolocation identifies the registered location of the IP address and supporting network infrastructure. While it provides valuable investigative context, it should not be interpreted as definitive proof of the attacker's physical location.

***

### Conclusion

The investigation identified **117.11.88.124** as the source IP address responsible for initiating communication with the target web server. IP intelligence indicates that the address is registered to **China Unicom's Tianjin Province Network**, with a registered location in **Tianjin, China**.

***

