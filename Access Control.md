### Unprotected functionality

    admin
    administrator
    administrator-panel
    robots.txt

### Parameter-based access control 
Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location. This could be:

A hidden field.
A cookie.
A preset query string parameter.

The application makes access control decisions based on the submitted value. For example:
"https://insecure-website.com/login/home.jsp?admin=true"
"https://insecure-website.com/login/home.jsp?role=1"

This approach is insecure because a user can modify the value and access functionality they're not authorized to, such as administrative functions. 

### Broken access control resulting from platform misconfiguration (Url and Method based)
Some applications enforce access controls at the platform layer. they do this by restricting access to specific URLs and HTTP methods based on the user's role. Some application frameworks support various non-standard HTTP headers that can be used to override the URL in the original request, such as X-Original-URL and X-Rewrite-URL.
```txt
POST / HTTP/1.1
X-Original-URL: /admin/deleteUser
```
An alternative attack relates to the HTTP method used in the request. In this situation, try to change request method and see response.
