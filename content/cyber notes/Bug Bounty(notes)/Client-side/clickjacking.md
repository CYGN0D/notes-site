## 📚 Clickjacking Notes

---

### 🧠 1. What is Clickjacking? _(Beginner)_

> **Clickjacking** is a UI redress attack where an attacker tricks a user into clicking on something different from what the user perceives, potentially performing malicious actions.

#### 🛠️ Simple Scenario:

- A user clicks a “Play” button.
    
- Behind it is a transparent iframe that submits a sensitive form (e.g., "Change email").
    

#### 🎯 Real-World Targets:

- Profile change pages
    
- “Like” buttons
    
- Purchase confirmations
    
- OAuth/authorization prompts
    

---

### 🧪 2. Basic Clickjacking PoC
`<!DOCTYPE html> <html> <head>   <title>Clickjacking PoC</title>   <style>     iframe {       position: absolute;       top: 0;       left: 0;       width: 1000px;       height: 1000px;       opacity: 0.01; /* Invisible */       z-index: 2;     }     .bait {       z-index: 1;       position: absolute;       top: 100px;       left: 100px;     }   </style> </head> <body>   *"<button class="bait">Claim your prize!</button>"*   ```html
```html
```html
<iframe src="https://target.com/sensitive-action"></iframe>
```
```
``` </body> </html>`

---

### 🔐 3. Frame Busting Techniques _(Defensive)_

Websites try to protect against clickjacking with **frame busting**:
#### ✅ Headers:

- `X-Frame-Options: DENY` → Not loadable in any frame
    
- `X-Frame-Options: SAMEORIGIN` → Only loadable on the same origin
    

#### ✅ CSP Frame-Ancestors:

`Content-Security-Policy: frame-ancestors 'none';`

- More flexible than `X-Frame-Options`
    
- Recommended modern protection
    

---

### ⚔️ 4. Frame Busting via JavaScript _(Legacy)_

Some sites use JS-based frame busting:

`if (top !== self) {   top.location = self.location; }`

But this is **not secure** — can be **bypassed** easily.

---

### 🚨 5. Frame Busting Bypasses _(Advanced)_

#### 1. **sandboxed iframes**:

````html
```html
```html
<iframe sandbox="allow-scripts allow-forms" src="https://target.com"></iframe>
```
```
````

- JS is blocked → frame busting code fails.
    

#### 2. **CSP Bypass** via misconfigured wildcard:

`Content-Security-Policy: frame-ancestors https://*.attacker.com`

- Can be abused with subdomain control.
    

#### 3. **Embedding via Blob:**

- If a site loads via JavaScript dynamically, you might spoof frame control using blobs or shadow DOM.
    

#### 4. **Window.name tricks**:

- Abuse `window.name` persistence across iframes to trick redirects.
    

#### 5. **Click Delay Tricks**:

- Use visual misdirection (loading indicators, fake buttons) while iframe loads behind.
    

---

### 🧩 6. Advanced Testing Techniques

#### 🔍 Tools:

- Burp Suite Clickbandit
    
- OWASP Clickjacking Defense Cheat Sheet
    
- Manual iframe PoCs
    
- Puppeteer/Playwright for automation
    

#### 🧪 What to Test:

- Authenticated pages
    
- Settings/config pages
    
- Pages without `X-Frame-Options` or `CSP`
    

#### 📌 Check Headers:
`curl -I https://target.com`

Look for:

- `X-Frame-Options`
    
- `Content-Security-Policy`
    
- Frame-ancestors directive
    

---

### 🧠 7. Summary Table

| Layer         | Defense                             | Bypass Possible?      |
| ------------- | ----------------------------------- | --------------------- |
| HTTP Header   | `X-Frame-Options` (DENY/SAMEORIGIN) | Limited               |
| HTTP Header   | CSP `frame-ancestors`               | Yes (with misconfig)  |
| JavaScript    | `top !== self`                      | Yes                   |
| Browser-level | sandboxed iframe                    | Yes (with trade-offs) |

---
