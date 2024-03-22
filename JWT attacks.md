## JWT
JSON web tokens (JWTs) are a standard format for sending cryptographically signed JSON data between systems. They're commonly used in authentication, session management, and access control mechanisms. This means that if an attacker can successfully modify a JWT, they may be able to escalate their own privileges or impersonate other users.
## Exploiting JWT
Use inspector in Burp to work with JSON value.
### Accepting arbitrary signatures
JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js library jsonwebtoken has ```verify()``` and ```decode()```. Occasionally, developers confuse these two methods and only pass incoming tokens to the ```decode()``` method. This effectively means that the application doesn't verify the signature at all. So, can provide your arbitrary value in json data to access some malicious goals, like changing user value to ```admin/administrator``` to achieve administritive priviledge. 
### Accepting tokens with no signature
JWT format ```header.payload.signature```.
    ```
    Steps:
    1. Set alg parameter value to none.
    2. Set/change username, role, id in paylaod.
    3. Remove the signature from the JWT, but remember to leave the trailing dot after the payload.
    ```
