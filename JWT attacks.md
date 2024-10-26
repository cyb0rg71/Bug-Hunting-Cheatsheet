# Table of Contents
1. [Important Paths That Reveal Server's Public/Private Key](#important-paths-that-reveal-servers-publicprivate-key)
2. [JWT](#jwt)
3. [Accepting Arbitrary Signatures](#accepting-arbitrary-signatures)
4. [Accepting Tokens with No Signature](#accepting-tokens-with-no-signature)
5. [JWT Authentication Bypass via Weak Signing Key (Bruteforcing)](#jwt-authentication-bypass-via-weak-signing-key-bruteforcing)
6. [JWT Header Parameter Injections](#jwt-header-parameter-injections)
    - [Injecting Self-Signed JWTs via the `jwk` Parameter](#injecting-self-signed-jwts-via-the-jwk-parameter)
    - [Injecting Self-Signed JWTs via the `jku` Parameter](#injecting-self-signed-jwts-via-the-jku-parameter)
    - [Injecting Self-Signed JWTs via the `kid` Parameter](#injecting-self-signed-jwts-via-the-kid-parameter)
7. [Algorithm Confusion](#algorithm-confusion)
    - [Performing an Algorithm Confusion Attack via Exposed Key](#performing-an-algorithm-confusion-attack-via-exposed-key)
    - [Deriving Public Keys from Existing Tokens (Without Exposed Keys)](#deriving-public-keys-from-existing-tokens-without-exposed-keys)

---

## Important Paths That Reveal Server's Public/Private Key
```
/jwks.json
/.well-known/jwks.json
```

## JWT
JSON Web Tokens (JWTs) are a standard format for sending cryptographically signed JSON data between systems. They're commonly used in authentication, session management, and access control mechanisms. If an attacker can successfully modify a JWT, they may be able to escalate their privileges or impersonate other users. 

**JWT Format**: `header.payload.signature`

## Accepting Arbitrary Signatures
JWT libraries typically provide two methods: one for verifying tokens and another that just decodes them. For example, the Node.js library `jsonwebtoken` has `verify()` and `decode()`. Sometimes, developers mistakenly use `decode()` without verifying the signature, effectively allowing arbitrary values in the JSON data, enabling an attacker to modify user roles and escalate privileges.

## Accepting Tokens with No Signature
1. Set the `alg` parameter value to `none`.
2. Set or change `username`, `role`, `id` in the payload.
3. Remove the signature from the JWT, but leave the trailing dot after the payload.

## JWT Authentication Bypass via Weak Signing Key (Bruteforcing)
Developers sometimes forget to change default secrets or hardcoded example secrets in JWT applications. This makes it easy for attackers to brute-force the server's secret using a wordlist.

Example command using `hashcat`:
```
hashcat -a 0 -m 16500 eyJraWQiOiIyZTI4MDIyYy02NWQ3LTRjNTEtYjU3Ni1mYWY3Mjg4MzVhMTUiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxMTIyMDc5MSwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.gteqm9hkHA6PdSv0pqbXBeUCON_8kwelR7Be1NI6WMs jwt.secrets.list
```

[Wordlist of common secret keys](https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list)

## JWT Header Parameter Injections
JWT headers, also known as JOSE headers, often contain several parameters of interest to attackers:

- **`jwk` (JSON Web Key)**: Provides an embedded JSON object representing the key.
- **`jku` (JSON Web Key Set URL)**: Provides a URL from which servers can fetch a set of keys containing the correct key.
- **`kid` (Key ID)**: Provides an ID that servers use to identify the correct key.

### Injecting Self-Signed JWTs via the `jwk` Parameter
Servers should ideally use a limited whitelist of public keys to verify JWT signatures. However, misconfigured servers may accept any key embedded in the `jwk` parameter. This can be exploited by signing a modified JWT using your own RSA private key and embedding the matching public key in the `jwk` header.

**Steps**:
1. Send the request to Repeater.
2. Go to the JSON Web Token tab to change the payload info.
3. Generate a new RSA key in the JWT Editor.
4. In the JSON Web Token tab, select Attack -> Embedded JWK and add the generated `kid`.
5. Send the HTTP request.

### Injecting Self-Signed JWTs via the `jku` Parameter
Instead of embedding public keys using the `jwk` parameter, some servers accept the `jku` parameter, allowing attackers to reference a JWK Set URL. If the server fetches the key from this URL, you can bypass authentication using the `jku` header.

**Steps**:
1. Send the request to Repeater.
2. Change payload info in the JSON Web Token tab.
3. Generate a new RSA key.
4. Host the RSA key in a JSON format on your exploit server at `/.well-known/jwks.json`.
   ```json
   {
       "keys": [
           {
               "kty": "RSA",
               "kid": "your-key-id",
               "n": "your-key-value",
               "e": "AQAB"
           }
       ]
   }
   ```
5. Change `kid` to match your RSA key's `kid`.
6. Add the `jku` parameter, pointing to your exploit server.
   ```
   "jku": "https://exploit.server/.well-known/jwks.json"
   ```
7. Sign the JWT and send the request.

### Injecting Self-Signed JWTs via the `kid` Parameter
If the server allows directory traversal in the `kid` parameter, an attacker can force the server to use an arbitrary file, like `/dev/null`, as the verification key. If the server supports symmetric algorithms, the JWT can be signed using an empty string.

**Steps**:
1. In the JWT header, set `kid` to `../../../../../../../dev/null`.
2. Change the payload `sub` claim to the desired user.
3. Sign the token with an empty string as the secret key.
4. Send the request.

**Alternative Approach**:
1. Add `"typ": "JWT"` and `"K": "AA=="` in the header.
2. Use a null byte (`AA==`) for the symmetric key.

## Algorithm Confusion
Algorithm confusion vulnerabilities occur when JWT libraries rely on the `alg` parameter to determine the verification method. An attacker can trick the server into using a different algorithm, such as HS256, treating the public key as an HMAC secret.

### Performing an Algorithm Confusion Attack via Exposed Key
1. Obtain the server's public key from `jwks.json`.
2. In Burp, go to the JWT Editor Keys tab and create a new RSA key.
3. Base64-encode the PEM key and create a new symmetric key in the JWT Editor.
4. Replace the `"k"` parameter with the Base64-encoded PEM key.
5. Set the `alg` header to `HS256` and sign the key.
6. Modify the payload and send the request.

### Deriving Public Keys from Existing Tokens (Without Exposed Keys)
1. Copy session cookie values from different login attempts.
2. Run `sudo docker run --rm -it portswigger/sig2n <cookie1> <cookie2>`.
3. Identify the valid tempered JWT.
4. Create a new symmetric key with the identified JWT.
5. Set the `alg` header to `HS256` and sign the key.
6. Modify the payload and send the request.

---
