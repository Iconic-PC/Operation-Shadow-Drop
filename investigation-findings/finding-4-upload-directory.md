# Finding 4 - Upload Directory

## Finding 4 — Upload Directory

### Objective

Identify the directory on the server where the attacker’s uploaded malicious file was stored and determine whether the uploaded file became accessible after the upload process.

***

## Investigation Steps

1. Reviewed HTTP traffic related to the previously identified malicious file upload activity.
2. Examined the server response after the upload request to determine whether the file upload was successful.
3. Searched the captured traffic for references to the uploaded PHP file.
4. Analyzed the attacker’s request to determine the location where the uploaded file was stored.
5. Identified the upload directory used by the vulnerable web application.

***

## Wireshark Filters Used

### 1. Identify Successful Upload Response

Filter:

```
http.response.code == 200
```

This filter was used to identify successful HTTP responses from the web server after the file upload attempt.

The filter helped locate **Frame 65**, which contained the server response confirming that the file upload was successful.

***

### 2. Locate Uploaded PHP File Access

Filter:

```
http contains "image.jpg.php"
```

This filter was used to search for references to the uploaded malicious PHP file within the captured HTTP traffic.

The search identified **Frame 138**, where the attacker attempted to access the uploaded file.

***

## Evidence

### 1. Malicious File Upload Request

The attacker submitted a file upload request to the application's upload functionality.

<table data-search="false"><thead><tr><th>Field</th><th>Value</th></tr></thead><tbody><tr><td>Packet Reference</td><td><strong>Frame 63</strong></td></tr><tr><td>Source IP</td><td><strong>117.11.88.124</strong></td></tr><tr><td>Destination IP</td><td><strong>24.49.63.79</strong></td></tr><tr><td>HTTP Method</td><td><strong>POST</strong></td></tr><tr><td>Upload Endpoint</td><td><strong>/reviews/upload.php</strong></td></tr><tr><td>Host</td><td><strong>shoporoma.com</strong></td></tr><tr><td>Uploaded Filename</td><td><strong>image.jpg.php</strong></td></tr><tr><td>File Type</td><td><strong>application/x-php</strong></td></tr></tbody></table>

The upload request contained:

```
Content-Disposition: form-data; name="uploadedFile"; filename="image.jpg.php"

Content-Type: application/x-php
```

This indicates that the attacker attempted to upload a PHP script disguised as an image file.

***

## 2. Server Upload Confirmation

After the upload request, the server returned a successful response.

| Field            | Value                          |
| ---------------- | ------------------------------ |
| Packet Reference | **Frame 65**                   |
| Response Status  | **HTTP/1.1 200 OK**            |
| Server           | **Apache/2.4.52 (Ubuntu)**     |
| Content Type     | **text/html**                  |
| Response Message | **File uploaded successfully** |

The server response body contained:

<figure><img src="../.gitbook/assets/fig 4.1 File upload successful.png" alt=""><figcaption></figcaption></figure>

```
File uploaded successfully
```

This confirms that the application accepted and processed the uploaded PHP file.

***

## 3. Uploaded File Access and Directory Discovery

After the successful upload, the attacker attempted to access the uploaded file.

<table data-search="false"><thead><tr><th>Field</th><th>Value</th></tr></thead><tbody><tr><td>Packet Reference</td><td><strong>Frame 138</strong></td></tr><tr><td>Source IP</td><td><strong>117.11.88.124</strong></td></tr><tr><td>Destination IP</td><td><strong>24.49.63.79</strong></td></tr><tr><td>HTTP Method</td><td><strong>GET</strong></td></tr><tr><td>Requested File</td><td><strong>image.jpg.php</strong></td></tr><tr><td>Full Request URI</td><td><strong>/reviews/uploads/image.jpg.php</strong></td></tr><tr><td>Host</td><td><strong>shoporoma.com</strong></td></tr></tbody></table>

<figure><img src="../.gitbook/assets/FIG 4.2 File Storage.png" alt=""><figcaption></figcaption></figure>

The attacker request:

```http
GET /reviews/uploads/image.jpg.php HTTP/1.1
```

revealed the directory where uploaded files were stored:

```
/reviews/uploads/
```

***

## Analysis

The investigation revealed a complete malicious file upload sequence.

The attacker first uploaded a PHP file through the vulnerable upload endpoint:

```
/reviews/upload.php
```

The uploaded file was:

```
image.jpg.php
```

Although the filename appeared to resemble an image file, the `.php` extension and content type:

```
application/x-php
```

indicated that the uploaded file was a server-side PHP script.

The server response captured in **Frame 65** confirmed that the upload operation was successful:

```
HTTP/1.1 200 OK

File uploaded successfully
```

Further analysis using the filter:

```
http contains "image.jpg.php"
```

identified **Frame 138**, where the attacker accessed:

```
/reviews/uploads/image.jpg.php
```

This revealed that the application stored uploaded files inside the publicly accessible directory:

```
/reviews/uploads/
```

Because the uploaded PHP file could be directly requested through the browser, the application potentially allowed attackers to deploy executable scripts within the web server environment.

***

## Conclusion

The investigation identified the file upload directory as:

```
/reviews/uploads/
```

The attack sequence was confirmed through the following evidence:

#### Frame 63 — Malicious File Upload

The attacker uploaded:

```
image.jpg.php
```

through:

```
POST /reviews/upload.php
```

***

#### Frame 65 — Upload Success Confirmation

The server confirmed the upload:

```
HTTP/1.1 200 OK

File uploaded successfully
```

***

#### Frame 138 — Uploaded File Access

The attacker accessed the uploaded file:

```
GET /reviews/uploads/image.jpg.php
```

The evidence demonstrates that the application allowed PHP files to be uploaded and stored in a web-accessible directory, creating a potential path for unauthorized server-side code execution.

***

