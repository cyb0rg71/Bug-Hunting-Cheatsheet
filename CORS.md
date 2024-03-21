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
2. Change Origin Header to null.
```
GET /sensitive-victim-data HTTP/1.1
Host: vulnerable-website.com
Origin: null
```
3. Change the origin header to one that begins with the origin of the site.
```
GET / HTTP/1.1
Host: vulnerable-website.com
Origin: https://vulnerable-website.com.malicious.com
```
4. Change the origin header to one that ends with the origin of the site.
```
GET / HTTP/1.1
Host: vulnerable-website.com
Origin: https://malicious.vulnerable-website.com
```
Note: If a website trusts an origin that is vulnerable to cross-site scripting (XSS), then an attacker could exploit the XSS to inject some JavaScript that uses CORS to retrieve sensitive information from the site that trusts the vulnerable application. So, if you find a CORS vulnerability, try to bind it with XSS attack.
