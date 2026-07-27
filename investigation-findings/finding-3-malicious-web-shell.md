# Finding 3 - Malicious Web Shell

## Objective

Identify whether the attacker uploaded a malicious script or web shell to the target web application and determine the method used to introduce the file.

***

### Investigation Steps

1. Analyzed HTTP POST requests within the captured network traffic.
2. Examined file upload-related endpoints and multipart form submissions.
3. Inspected uploaded file metadata contained within HTTP request bodies.
4. Identified a suspicious PHP file upload attempt.

***

### Evidence

#### Upload Request Details

<figure><img src="../.gitbook/assets/fig 3 malicious file name.png" alt=""><figcaption></figcaption></figure>

<table data-search="false"><thead><tr><th>Field</th><th>Value</th></tr></thead><tbody><tr><td>Packet Reference</td><td><strong>Frame 63</strong></td></tr><tr><td>Source IP</td><td><strong>117.11.88.124</strong></td></tr><tr><td>Destination IP</td><td><strong>24.49.63.79</strong></td></tr><tr><td>HTTP Method</td><td><strong>POST</strong></td></tr><tr><td>Upload Endpoint</td><td><strong>/reviews/upload.php</strong></td></tr><tr><td>Host</td><td><strong>shoporoma.com</strong></td></tr><tr><td>Content Type</td><td><strong>multipart/form-data</strong></td></tr></tbody></table>

***

#### Uploaded File Information

| Field      | Value                 |
| ---------- | --------------------- |
| Form Field | uploadedFile          |
| Filename   | **image.jpg.php**     |
| File Type  | **application/x-php** |

***

### Analysis

Analysis of Frame 63 revealed that the attacker submitted a file upload request to:

```
/reviews/upload.php
```

The request contained a file named:

```
image.jpg.php
```

with MIME type:

```
application/x-php
```



