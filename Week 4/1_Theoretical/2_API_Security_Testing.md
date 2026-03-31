
---

# 2_API_Security_Testing.md


# API Security Testing

## Overview

API security testing focuses on identifying vulnerabilities in Application Programming Interfaces (APIs), particularly those defined in the OWASP API Security Top 10 (2023). APIs are a primary attack surface due to excessive data exposure, weak authentication, and improper authorization.

Key focus areas:
- Broken Object Level Authorization (BOLA)
- Authentication bypass
- GraphQL injection
- Rate limit evasion
- API enumeration and fuzzing

---

## API Reconnaissance (Endpoint Discovery)

### Common API Endpoints

```http

/api
/api/v1
/api/v2
/v1
/v2
/swagger
/swagger-ui
/api-docs
/graphql

```

### Wordlist Sources
- SecLists API wordlists
- FuzzDB common API endpoints

---

## API Enumeration Techniques

### Burp Suite Intruder

```http

GET /api/FUZZ/

````

Payload:
- Use SecLists API wordlist

---

### ffuf Enumeration

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/api \
-u http://target/api/FUZZ
````

---

### Hidden Parameter Discovery (Arjun)

```bash
python arjun.py -u http://target/api/users --post --stable
```

---

### Swagger / OpenAPI Discovery

Check:

```http
/swagger
/api-docs
/v1/swagger.json
```

---

## OWASP API Top 10 Testing Matrix

| API Risk             | Test Method      | Burp Workflow             | Expected Exploit         |
| -------------------- | ---------------- | ------------------------- | ------------------------ |
| API1: BOLA           | ID Enumeration   | Intruder Sniper (1-10000) | Access other user data   |
| API2: Auth           | JWT None Attack  | Repeater header edit      | Token bypass             |
| API3: Property Auth  | JSON Tampering   | Repeater                  | Modify restricted fields |
| API4: Resource Abuse | GraphQL batching | Intruder                  | Server slowdown          |
| API5: Function Auth  | Role escalation  | Repeater                  | Admin function access    |

---

## API1: Broken Object Level Authorization (BOLA)

### Description

Occurs when APIs fail to properly validate object ownership.

### Testing

#### ID Enumeration

```http
/api/user/1
/api/user/2
/api/user/3
```

#### UUID Prediction

* Weak UUID generation patterns

#### Mass Assignment

```json
{
  "username": "user",
  "role": "admin"
}
```

#### IDOR (Insecure Direct Object Reference)

* Modify object ID to access other users' data

---

## API2: Broken Authentication

### Common Issues

* JWT "none" algorithm
* Token leakage
* API key exposure
* Session fixation

### JWT Manipulation

```http
Authorization: Bearer none.eyJ...signature
```

### Token Testing

* Modify payload
* Remove signature
* Replay tokens

---

## API3: Broken Object Property Level Authorization

### Example

```json
{
  "user": "normal",
  "isAdmin": true
}
```

* Modify hidden or restricted fields

---

## API4: Unrestricted Resource Consumption

### DoS Techniques

* Large payloads
* Recursive queries
* GraphQL batching

---

## API5: Broken Function Level Authorization

### Example

* Access admin endpoints as normal user

```http
/api/admin/deleteUser
```

---

## GraphQL Injection

### Introspection Query

```json
{__schema{types{name fields{name type{name}}}}}
```

---

### Batching Attack

```json
[
  {"query":"query1"},
  {"query":"query2"}
]
```

---

### Alias Overuse

```json
{
  a1:user1{id}
  a2:user2{id}
  a3:user3{id}
}
```

---

### Direct Field Access (Bypass Introspection Disabled)

```json
{users{id name email}}
```

---

## Rate Limiting Bypass

### Techniques

* IP rotation
* Parameter pollution

```http
?api_key=1&api_key=2
```

* Slow requests (Slowloris style)
* Distributed requests

---

## Burp Suite API Workflow

```
1. Proxy → Intercept API requests
2. Target → Scope → Identify endpoints
3. Repeater → Modify requests
4. Intruder → Fuzz parameters and IDs
5. Logger++ → Capture API structure
```

---

## Advanced Burp Testing

### Intruder Modes

* Sniper: Single parameter fuzzing
* Cluster Bomb: Multiple parameter combinations

```http
/api/users/FUZZ?role=FUZZ2
```

---

## Postman API Fuzzing

### Collection Runner

* Use CSV/JSON payloads

### Example Data

```json
[
  {"id": 1},
  {"id": 2},
  {"id": 3}
]
```

---

### Pre-request Script (Example Concept)

```javascript
// Generate dynamic token or headers
```

---

### Test Scripts

```javascript
pm.test("Status Code Check", function () {
    pm.response.to.have.status(200);
});
```

---

## GraphQL Attack Cheat Sheet

| Attack          | Description                     |
| --------------- | ------------------------------- |
| Introspection   | Schema extraction               |
| Batching        | Multiple queries in one request |
| Alias Flood     | Overload server                 |
| Field Abuse     | Access hidden fields            |
| Deep Nesting    | Resource exhaustion             |
| Recursive Query | Infinite processing             |

---

## API Pentesting Checklist

### Recon

* Discover endpoints
* Identify API structure

### Enumeration

* Fuzz endpoints
* Discover parameters

### Authentication Testing

* Test JWT flaws
* Check token reuse

### Authorization Testing

* Perform BOLA
* Test role escalation

### Injection Testing

* SQLi
* GraphQL injection

### Rate Limiting

* Bypass limits
* Stress test API

---

## Attack Chain Diagram

```mermaid
graph TD
A[API Recon] --> B[Endpoint Discovery]
B --> C[Parameter Enumeration]
C --> D[BOLA Exploit]
D --> E[Privilege Escalation]
E --> F[Data Exfiltration]
```

---

## Lab Exercise (Juice Shop / DVWA)

### Steps

1. Identify API endpoints
2. Perform BOLA on:

```http
/rest/user/profile
```

3. Test GraphQL:

```http
/graphql
```

4. Perform rate limit bypass
5. Document findings

---

## Key Takeaways

* APIs are highly vulnerable due to weak authorization
* BOLA is the most common API vulnerability
* GraphQL introduces unique attack vectors
* Burp Suite and Postman are essential tools
* Proper testing requires chaining multiple techniques

---

