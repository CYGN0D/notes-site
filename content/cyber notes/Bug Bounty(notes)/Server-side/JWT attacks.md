## 1. What is JWT?

**JWT (JSON Web Token)** is a **compact, URL-safe token format** used to transfer claims between parties.

✅ Stateless  
✅ Stored client-side  
✅ Common in APIs, OAuth, SSO

📌 **JWT is only a container — security depends on JWS or JWE**

---

## 2. JWT Family Overview

|Component|Full Name|Purpose|
|---|---|---|
|JWT|JSON Web Token|Token format|
|JWS|JSON Web Signature|Signing (Integrity)|
|JWE|JSON Web Encryption|Encryption (Confidentiality)|
|JWK|JSON Web Key|Key representation|
|JWA|JSON Web Algorithms|Crypto standards|

---

## 3. JWT Basic Structure

### JWS JWT (Most Common)

`HEADER.PAYLOAD.SIGNATURE`

### JWE JWT

`HEADER.ENCRYPTED_KEY.IV.CIPHERTEXT.AUTH_TAG`

---

## 4. JWT Claims

### Registered Claims

|Claim|Meaning|
|---|---|
|`iss`|Issuer|
|`sub`|Subject|
|`aud`|Audience|
|`exp`|Expiration|
|`nbf`|Not before|
|`iat`|Issued at|
|`jti`|Token ID|

### Private / Custom Claims

`{   "user": "admin",   "role": "admin",   "scope": "all" }`

---

## 5. JWS – JSON Web Signature

### What is JWS?

JWS provides:

- ✅ Integrity
    
- ✅ Authentication
    
- ❌ Confidentiality
    

⚠️ Payload is **Base64URL encoded, NOT encrypted**

---

### JWS Structure

`BASE64URL(header).BASE64URL(payload).BASE64URL(signature)`

---

### JWS Header Example

`{   "alg": "HS256",   "typ": "JWT" }`

### JWS Payload Example

`{   "user": "admin",   "role": "admin",   "exp": 1735689600 }`

---

### How JWS Works (Flow)

`Client:   Header + Payload      ↓   Sign with secret/private key      ↓   Send token  Server:   Verify signature      ↓   Trust payload`

---

### JWS Algorithms

#### Symmetric (Shared Secret)

- HS256
    
- HS384
    
- HS512
    

#### Asymmetric (Public / Private Key)

- RS256
    
- ES256
    
- PS256
    

---

### JWS Security Properties

|Property|Status|
|---|---|
|Integrity|✅|
|Authentication|✅|
|Confidentiality|❌|
|Payload Visibility|Public|

---

## 6. JWE – JSON Web Encryption

### What is JWE?

JWE encrypts the payload, providing:

- ✅ Confidentiality
    
- ✅ Integrity
    
- ✅ Authentication (indirect)
    

🔒 Payload is **fully encrypted**

---

### JWE Structure (5 Parts)

`ProtectedHeader .EncryptedKey .InitializationVector .Ciphertext .AuthenticationTag`

---

### JWE Header Example

`{   "alg": "RSA-OAEP",   "enc": "A256GCM" }`

|Field|Purpose|
|---|---|
|`alg`|Key encryption|
|`enc`|Content encryption|

---

### How JWE Works (Flow)

`Sender:   Generate random CEK      ↓   Encrypt payload with CEK (AES)      ↓   Encrypt CEK with receiver's public key      ↓   Send JWE token  Receiver:   Decrypt CEK with private key      ↓   Decrypt payload`

---

### JWE Encryption Layers

1 Key Encryption

- RSA-OAEP
    
- ECDH-ES
    

2 Content Encryption

- AES-GCM
    
- AES-CBC + HMAC
    

---

### JWE Security Properties

|Property|Status|
|---|---|
|Confidentiality|✅|
|Integrity|✅|
|Authentication|✅|
|Payload Visibility|Encrypted|

---

## 7. JWS vs JWE (Comparison)

|Feature|JWS|JWE|
|---|---|---|
|Payload readable|✅|❌|
|Token parts|3|5|
|Performance|Fast|Slower|
|Complexity|Low|High|
|Most common|✅|❌|
|CTF usage|Very high|Rare|

---

## 8. Nested JWT (JWS + JWE)

🔥 **Maximum security JWT**

### Process

`Payload   ↓ JWS (sign)   ↓ JWE (encrypt)`

### Verification

`Decrypt JWE → Verify JWS`

✅ Confidential  
✅ Authenticated  
✅ Tamper-proof

# JWT Attacks

## 2. Why JWT is Vulnerable?

JWT is:

- **Stateless**
    
- **Client-side stored**
    
- **Trusted blindly by servers**
    

If **verification logic is weak**, attackers can:

- Become admin
    
- Bypass authentication
    
- Forge tokens
    
- Persist access
## 3. JWT Attack Surface Overview

|Area|Risk|
|---|---|
|Algorithm validation|Algorithm confusion|
|Secret management|Brute-force / leakage|
|Claim validation|Privilege escalation|
|Token lifecycle|Replay|
|Key handling|Key confusion / injection|

---

## 4. JWT Algorithm Confusion Attack (CRITICAL)

### Root Cause

Server trusts the `alg` field **from the token itself**.

### Attack Idea

Change algorithm from **RS256 → HS256**  
Use **public key as HMAC secret** 🤯

### Steps

1. Intercept a valid JWT
    
2. Change header:
    

`{   "alg": "HS256" }`

3. Use **RSA public key** as HMAC secret
    
4. Sign token with HS256
    
5. Server verifies successfully
    

### Result

✅ Full authentication bypass

### Tools

- Burp Suite JWT Editor
    
- jwt_tool
    
- PyJWT

## 5. `alg: none` Attack

### Root Cause

Server allows `alg: none`

### Attack Token

`{   "alg": "none" }`

Signature: **empty**

### Example

`HEADER.PAYLOAD.`

### Result

✅ No signature verification  
✅ Instant login bypass

### Mitigation

- Disallow `none`
    
- Hardcode acceptable algorithms
    

---

## 6. Weak Secret Brute-Force Attack

### When Possible

- HS256 used
    
- Weak/shared secret
    
- No rate limiting
    

### Workflow

1. Capture JWT
    
2. Brute-force secret
    
3. Re-sign token with admin payload
    

### Tools

`jwt_tool <JWT> -C -d /path/wordlist.txt`

or

`hashcat -m 16500 jwt.txt wordlist.txt`

### Result

✅ Admin access

---

## 7. JWT Claim Manipulation

### Common Dangerous Claims

|Claim|Abuse|
|---|---|
|`role`|user → admin|
|`isAdmin`|false → true|
|`uid`|privilege hopping|
|`email`|impersonation|
|`scope`|expand permissions|

### Example

`{   "user": "admin",   "role": "admin" }`

### If signature not verified properly:

✅ Privilege escalation

---

## 8. Expiry (`exp`) Bypass Attacks

### Bugs

- `exp` not checked
    
- Uses local time incorrectly
    
- Accepts negative values
    

### Payload

`{   "exp": 9999999999 }`

or

`{   "exp": -1 }`

### Result

✅ Tokens never expire

---

## 9. JWT Replay Attack

### Root Cause

- Tokens are stateless
    
- No revocation list
    
- Long expiry
    

### Attack

1. Steal JWT
    
2. Reuse indefinitely
    

### Impact

- Session hijacking
    
- Persistent access
    

---
# JWT Header Parameter Injections

**1. `jwk` (JSON Web Key)**

- Contains a public key inside the JWT
    
- Server may trust this embedded key
    
- Attacker signs JWT with their own key and includes it in `jwk`
    

**2. `jku` (JSON Web Key Set URL)**

- URL pointing to a set of public keys
    
- Server fetches keys from this URL
    
- Attacker hosts a malicious JWKS and points `jku` to it
    

**3. `kid` (Key ID)**

- Identifies which key to use
    
- Vulnerable to key confusion, path traversal, or injection
    
- Poor validation allows attacker-controlled key selection
## 10. JWK / `kid` Parameter Attacks

### Header with `kid`

`{   "alg": "HS256",   "kid": "key1" }`

### Attack Types

#### a) Path Traversal

`"kid": "../../etc/passwd"`

#### b) SQL Injection

`"kid": "1 OR 1=1"`

#### c) Remote Key Injection

`"jku": "https://attacker.com/jwks.json"`

Server fetches attacker-controlled key 🔥

---

## 11. JWK Set Injection Attack

### Idea

- Server dynamically loads keys
    
- Attacker provides malicious JWK
    

### Result

✅ Token signed by attacker is accepted

---

## 12. Key Confusion Attack

### Scenario

Server supports both:

- HS256
    
- RS256
    

Developer mistake:

- Uses **same secret variable**
    

### Exploit

- Sign HS256 token
    
- Server verifies as RS256
    

✅ Authentication bypass

---

## 13. JWT Header Injection

### Payload

`{   "crit": ["exp"],   "exp": "invalid" }`

Some parsers:

- Ignore invalid claims
    
- Skip checks
    

---

## 14. JWT Compression Attacks (Advanced)

### Attack Type

**CRIME / BREACH-like attacks**

- JWT compressed (`zip: DEF`)
    
- Token used in HTTPS
    
- Sensitive info inside payload
    

### Result

✅ Extract secrets via compression oracle

---

## 15. Real CTF / Pentest Indicators

Look for:

- JWT in cookies / Authorization header
    
- HS256 with short secret
    
- `alg` editable
    
- No `aud`, `iss`, `nbf`
    
- No token revocation
    

---

## 16. Tools for JWT Attacks

### Must-Know Tools

- **jwt.io**
    
- **jwt_tool**
    
- **Burp Suite JWT Editor**
    
- **hashcat**
    
- **PyJWT**
