## JWT
JSON web tokens (JWTs) are a standard format for sending cryptographically signed JSON data between systems. They're commonly used in authentication, session management, and access control mechanisms. This means that if an attacker can successfully modify a JWT, they may be able to escalate their own privileges or impersonate other users.
## Exploiting JWT
Use inspector in Burp to work with JSON value.
### Accepting arbitrary signatures
JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js library jsonwebtoken has ```verify()``` and ```decode()```. Occasionally, developers confuse these two methods and only pass incoming tokens to the ```decode()``` method. This effectively means that the application doesn't verify the signature at all. So, can provide your arbitrary value in json data to access some malicious goals, like changing user value to ```admin/administrator``` to achieve administritive priviledge. 
### Accepting tokens with no signature
JWT format ```header.payload.signature```.
```
1. Set alg parameter value to none.
2. Set/change username, role, id in paylaod.
3. Remove the signature from the JWT, but remember to leave the trailing dot after the payload.
```
### JWT authentication bypass via weak signing key (Bruteforcing)
 When implementing JWT applications, developers sometimes make mistakes like forgetting to change default or placeholder secrets. They may even copy and paste code snippets they find online, then forget to change a hardcoded secret that's provided as an example. In this case, it can be trivial for an attacker to brute-force a server's secret with a wordlist using hashcat.
 ```
hashcat -a 0 -m 16500 eyJraWQiOiIyZTI4MDIyYy02NWQ3LTRjNTEtYjU3Ni1mYWY3Mjg4MzVhMTUiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxMTIyMDc5MSwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.gteqm9hkHA6PdSv0pqbXBeUCON_8kwelR7Be1NI6WMs jwt.secrets.list
```
### JWT header parameter injections
 According to the JWS specification, only the alg header parameter is mandatory. In practice, however, JWT headers (also known as JOSE headers) often contain several other parameters. The following ones are of particular interest to attackers.
```
jwk (JSON Web Key) - Provides an embedded JSON object representing the key.

jku (JSON Web Key Set URL) - Provides a URL from which servers can fetch a set of keys containing the correct key.

kid (Key ID) - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choose from. Depending on the format of the key, this may have a matching kid parameter.
```
