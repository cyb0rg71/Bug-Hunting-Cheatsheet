### Testing for CORS vulnerability
1. Change Origin Header to arbitrary domain.
```
GET /sensitive-victim-data HTTP/1.1
Host: vulnerable-website.com
Origin: http://example.com to Origin: https://malicious.com
```
If it response with this, that means the website is vulnerable to CORS.
```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://malicious.com
Access-Control-Allow-Credentials: true
```
