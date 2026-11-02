# 4. Web Security

## The one story

A bank branch does not rely on one lock. It has a door, a guard who checks identity, a rule about who may enter the strong room, a camera, and a procedure for a lost key.

A website needs the same layers.

- **HTTPS and TLS** are the protected passage between the customer's phone and the bank. Outsiders on the path cannot read or quietly alter the conversation.
- **Secure sessions** are the visitor pass given after login. The pass must be hard to steal, hard to forge, and quick to cancel.
- **OWASP** is a respected list of the ways web applications usually fail, plus guidance on how to prevent those failures.

Security is not a feature added at the end. It is a set of habits in design, coding, configuration, and operations.

**Say it in one line:** Web security means the right person sees the right data, the conversation cannot be spied on or changed, and common web attacks do not succeed.

---

## Goals, in words a control officer already knows

| Goal | Question | Banking meaning |
| --- | --- | --- |
| Confidentiality | Can an unauthorised person read it? | Account balances and OTPs stay private |
| Integrity | Can someone change it unnoticed? | A transfer of ₹ 1,000 must not become ₹ 10,000 on the way |
| Availability | Can legitimate users get the service? | The app works on salary day and during an attack that tries to flood it |
| Authenticity | Is the other party genuine? | The customer is really that customer. The site is really the bank |
| Non-repudiation | Can someone later deny the act? | Audit evidence of a payment instruction |
| Accountability | Can we trace actions to a person or process? | Staff access to customer data is logged |

CIA, meaning confidentiality, integrity, and availability, is the short form. The extra rows matter in a bank too.

---

## OWASP

### What the letters mean

**OWASP** is the Open Worldwide Application Security Project. Older material says Open Web Application Security Project. Both names refer to the same community.

It publishes free guidance. The famous piece is the **OWASP Top 10**, a list of the most important web application security risks, refreshed every few years. It is an awareness and priority list, not a complete security standard and not a government law.

For a promotion exam, know:

- What OWASP is.
- That the Top 10 lists the most critical web application risks.
- The idea of each current category, in simple language.
- A few classic attack names that sit inside those categories: SQL injection, cross-site scripting, and cross-site request forgery.

The 2021 Top 10 is the list most textbooks still teach. A 2025 release candidate was in progress in the community, so if an exam says "as per latest OWASP Top 10", read the options carefully and prefer the widely taught 2021 names unless the paper clearly uses newer ones.

### OWASP Top 10 (2021), one by one

**1. Broken Access Control**

The application does not enforce what a user may do.

Story: a customer changes the account number in a web address from their own account to someone else's, and the server returns the other person's statement. Login succeeded. Authorisation failed.

This category is ranked first because it is common and serious.

Prevention in plain language:

- Check authorisation on the server for every request.
- Deny by default. Start with no permission, then grant what is needed.
- Do not rely on hiding a button. A hidden "approve" button can still be called.
- Use indirect references carefully, and always check ownership.
- Log access failures and alert on repeated ones.

**2. Cryptographic Failures**

Older name: sensitive data exposure. The problem is weak or missing protection of data.

Examples: passwords stored as plain text, an account page served over plain HTTP, old encryption, or card data kept without need.

Prevention:

- Use HTTPS everywhere.
- Store passwords with a slow, salted hash made for passwords, such as bcrypt, scrypt, or Argon2. A hash is a one-way transformation. The server checks the hash. It does not need to store the password itself.
- Do not invent your own encryption.
- Do not collect data you do not need. Data you do not hold cannot leak.

**3. Injection**

The application treats an attacker's input as a command.

**SQL injection** is the classic case. SQL is the database language. If the program glues a customer's typing directly into a SQL sentence, the customer can change the sentence.

Imagine the intended question is "find the user with this account number". A crafted input changes it into "find the user and also ignore the password check". The database obeys the altered sentence.

Other injection forms: command injection into the operating system, and LDAP injection into a directory search.

Prevention:

- Use **parameterised queries**, also called prepared statements. The SQL shape is fixed. The user value is data, never part of the sentence.
- Validate input on the server.
- Give the database account the least privilege it needs. The internet-banking reader should not be able to drop tables.

**4. Insecure Design**

The weakness is in the design, before coding. A transfer flow with no step-up authentication for a new beneficiary, no limit, and no cooling period is a design flaw. A later firewall cannot fully repair a missing business control.

Prevention: threat modelling, which means asking "how could this journey be abused?" during design, plus security requirements and abuse-case tests.

**5. Security Misconfiguration**

The parts are good and the settings are bad.

Examples: default passwords left unchanged, a debug page left on in production, directory listing turned on, unnecessary services running, cloud storage left open to the world, or error messages showing stack traces to customers.

Prevention: hardened standard builds, remove unused features, automated configuration checks, and different settings for development and production.

**6. Vulnerable and Outdated Components**

Applications are built from libraries. A forgotten old library can carry a known hole. The famous risk is: the bank's own code is careful, and a component underneath is not.

Prevention: keep an inventory of components, patch supported versions, remove unused libraries, and scan dependencies. This is supply-chain hygiene.

**7. Identification and Authentication Failures**

Older name: broken authentication. The system is weak at proving identity or at managing the login session.

Examples: unlimited password guesses, session ids that never expire, default credentials, weak OTP handling, or a session id written into the web address where it leaks via logs and history.

Prevention:

- Multi-factor authentication for sensitive journeys.
- Rate-limit and lock out credential stuffing, which is trying stolen password lists.
- Use strong, unpredictable session ids.
- Rotate the session id after login.
- Do not put session ids in URLs.

**8. Software and Data Integrity Failures**

The application trusts data or software that it should have verified.

Examples: an update downloaded without checking its signature, or a deserialisation gadget where untrusted data becomes a live object and runs attacker-controlled behaviour. **Deserialisation** means rebuilding a live object from stored bytes. Doing that with untrusted input is dangerous.

A related idea is a supply-chain attack: the build pipeline or a plugin is compromised, so bad code is shipped as if it were yours.

Prevention: verify signatures, use trusted repositories, protect the build pipeline, and deserialise only trusted data with safe formats.

**9. Security Logging and Monitoring Failures**

The attack happens and nobody notices. There is no useful log, the log is never reviewed, or alerts do not reach anyone.

In a bank this also fails audit. You need evidence of login failures, high-value transfers, privilege changes, and access to sensitive data.

Prevention: log important security events, keep logs tamper-resistant, synchronise clocks, alert on meaningful patterns, and rehearse incident response.

Do not log secrets. A log full of passwords is itself a breach.

**10. Server-Side Request Forgery (SSRF)**

The server is tricked into making a request to a place the attacker could not reach directly.

Story: a feature says "enter a URL to fetch your document". The attacker enters an internal address such as the metadata service of the cloud platform or an admin page that only the data-centre network can see. The bank's server fetches it and returns the secret to the attacker.

Prevention: allow only expected destinations, block requests to internal addresses, and do not pass the raw response back to the user.

### Classic attack names that sit inside the Top 10

**Cross-site scripting (XSS)**

An attacker plants a script in a page other users will open. Example: a script is saved inside a beneficiary nickname, and when a staff screen displays that nickname as HTML, the script runs in the staff browser and steals their session.

The application mixed untrusted data into HTML without proper encoding.

Prevention: encode output for the context it is going into, use safe frameworks, and set a Content Security Policy, which tells the browser which scripts are allowed to run. XSS relates to injection. It is still taught under its own name.

**Cross-site request forgery (CSRF)**

The customer is logged in to the bank. In another tab they open a malicious page. That page silently submits a transfer request to the bank. The browser helpfully attaches the bank cookie. The bank sees a valid session and might obey.

The customer did not intend the request. The browser sent the credential anyway.

Prevention: a CSRF token, which is an unpredictable value the real bank page includes and the malicious page cannot read; `SameSite` cookies; and requiring re-authentication for high-risk actions. Modern cookie defaults help, and high-value actions should still demand a fresh check.

**Clickjacking**

A transparent fake page is laid over the bank's page so the customer thinks they are clicking "Cancel" and actually click "Approve". Prevention includes frame controls such as `X-Frame-Options` or CSP `frame-ancestors`, which stop other sites from embedding the bank's page.

**Brute force and credential stuffing**

Brute force tries many passwords. Credential stuffing tries username-password pairs stolen from other sites. Prevention: MFA, rate limits, device and behaviour checks, and breached-password screening.

### OWASP beyond the Top 10

Worth a short mention if you want depth:

- **ASVS**, the Application Security Verification Standard, is a checklist of security requirements.
- **MASVS** is the mobile equivalent.
- The **OWASP API Security Top 10** focuses on API risks such as broken object-level authorisation, unbounded data exposure, and lack of rate limiting. For a bank full of APIs, this list is as practical as the web list.

**Say it in one line:** OWASP Top 10 is the standard awareness list of the most serious web application risks. Broken access control, cryptographic failure, injection, weak authentication, misconfiguration, outdated components, missing monitoring, and SSRF are the ideas to be able to explain.

---

## HTTPS

### What the letters mean

**HTTP** is the HyperText Transfer Protocol. It is the set of rules a browser and a server use to exchange requests and responses. It defines methods such as GET and POST, status codes such as 200 and 404, and headers.

**HTTPS** is HTTP carried over a secure transport. The S stands for Secure. Underneath, **TLS** provides the security.

What HTTPS gives you on the path between device and server:

| Protection | Meaning |
| --- | --- |
| Confidentiality | A person sharing the coffee-shop Wi-Fi cannot read the balance |
| Integrity | They cannot silently change the amount in transit |
| Server authentication | The browser checks that the certificate belongs to the site the customer asked for |

What HTTPS does not do by itself:

- It does not prove the customer is the account holder. Login does that.
- It does not stop the customer from typing their password into a look-alike fake site that has its own certificate. The customer still has to look at the real domain.
- It does not repair a SQL injection bug inside the application.
- It does not hide traffic from the bank's own server. The server sees the decrypted request. That is necessary.

The browser shows a padlock for HTTPS. Customers should still read the domain name. `sbi.example.com` and `sbi.example.com.attacker.net` are different sites. Teach the habit gently in service channels.

### Why plain HTTP is unacceptable for banking

On plain HTTP, anything on the path can read cookies, passwords, and account data, and can modify responses. A login page must be HTTPS. After login, every page and every API call must be HTTPS. Mixing them creates a **mixed content** problem and a downgrade opportunity.

**HSTS**, HTTP Strict Transport Security, is a header that tells the browser: "For a period of time, only use HTTPS for this site." It reduces the chance of being tricked onto plain HTTP.

---

## TLS

### What the letters mean

**TLS** means Transport Layer Security. It is the protocol that makes HTTPS secure. It also protects other connections, such as some email and database links, but the syllabus context is the web.

**SSL**, Secure Sockets Layer, is the ancestor of TLS. SSL 2.0 and SSL 3.0 are obsolete and unsafe. People still say "SSL certificate" out of habit. The accurate modern sentence is: **the certificate is used with TLS. Old SSL versions must be disabled.**

### What a certificate is

A **digital certificate** binds a site name to a public key, and a trusted party signs that binding.

The parties:

| Word | Meaning |
| --- | --- |
| Public key | A key that can be shared. Used to encrypt to the owner, or to check the owner's signature |
| Private key | The matching key that stays secret on the server. Used to decrypt or to create a signature. If this leaks, the certificate can no longer be trusted |
| Certificate Authority (CA) | An organisation browsers already trust, which signs certificates |
| Subject | The name the certificate is for, such as `online.bank.example` |
| Validity period | Start and end dates. An expired certificate makes the browser warn and may stop the app |
| Chain of trust | Your certificate is signed by an intermediate CA, which is signed by a root CA that the browser already knows |

The bank proves "we hold the private key that matches this certificate". The browser checks the signature chain, the dates, and that the name matches the site.

**Self-signed certificate** means the server signed its own certificate. Fine in a private lab if you manually trust it. Not acceptable for public customers, because nothing they already trust has vouched for it.

### The handshake, without mathematics

A TLS handshake is the short introduction before real data flows.

1. The browser says hello and lists TLS versions and cipher suites it supports. A **cipher suite** is an agreed set of algorithms.
2. The server chooses a modern suite and sends its certificate.
3. The browser checks the certificate: trusted issuer, correct name, not expired, not revoked.
4. They agree on session keys. In modern TLS the private key is not used to encrypt the whole conversation directly. It is used to authenticate and to help both sides derive fresh session keys. Older RSA key-exchange behaviour is retired in TLS 1.3.
5. From here, application data, meaning the HTTP request and response, is encrypted with those session keys.

**TLS 1.3** is the modern version. It is shorter and removes older unsafe options. TLS 1.2 is still widely used when configured with safe cipher suites. TLS 1.0 and 1.1 are obsolete. SSL is obsolete.

**Forward secrecy** means that if tomorrow someone steals the server's private key, they still cannot decrypt yesterday's recorded sessions. Fresh session keys that were not stored give this property. Modern configurations aim for it.

**Revocation** means declaring a certificate untrusted before it expires, because the private key leaked. Browsers use mechanisms such as CRL (certificate revocation list) and OCSP (online certificate status protocol). You only need the idea: a bad certificate can be cancelled.

### Where TLS ends

TLS protects the pipe. After the server decrypts the request, application security begins: access control, injection defence, and session handling. Examiners like this distinction.

**Say it in one line:** HTTPS is HTTP inside TLS. TLS uses a trusted certificate and a handshake to give a private, tamper-evident channel and to prove the server's identity. SSL is the old name and the old, retired protocol.

---

## Secure sessions

A session is the server's memory of a logged-in user. Securing it is as important as securing the password, because a stolen session can be used without the password.

### How a safe session is born

1. The customer authenticates with a password and a second factor.
2. The server creates a new session id using a long, random, unpredictable value. Not the user id. Not the account number. Not a simple counter.
3. The server stores session data server-side: user id, login time, last activity, authentication level, and maybe device information.
4. The server sends only the session id to the browser as a cookie with protective flags.
5. The server deletes any session id that existed before login and issues a fresh one. This is **session regeneration**. It stops **session fixation**, where an attacker plants a known session id and waits for the victim to log in to it.

### Cookie flags you should be able to explain

| Flag | What it does |
| --- | --- |
| `Secure` | The browser sends the cookie only over HTTPS |
| `HttpOnly` | JavaScript on the page cannot read the cookie. This reduces damage from XSS |
| `SameSite` | Limits when the browser attaches the cookie to a request that came from another site. This reduces CSRF. `Strict` and `Lax` are the useful settings. `None` allows cross-site sending and must be paired with `Secure` |
| Appropriate expiry | A session cookie that disappears when the browser closes, plus a server-side idle timeout, limits how long a stolen cookie works |

### Timeouts

- **Idle timeout** ends the session after a period with no activity. A customer who walks away from a logged-in phone is protected to a degree.
- **Absolute timeout** ends the session after a maximum life even if the customer is active, and then requires a fresh login. This caps the value of a stolen session.

Timeouts must exist on the server. Hiding the screen in the app while the server still honours the session is not a timeout.

### More session rules

- **Logout** must destroy the session on the server, not only delete the cookie on the phone. If only the cookie is deleted, a previously copied cookie may still work.
- **Concurrent sessions.** The bank decides whether a second login kills the first. For many financial apps, a new login invalidating the old session is the safer default, with a message to the customer.
- **Privilege change.** If a customer steps up from "view" to "pay" by entering an OTP, record that stronger level in the server-side session. Do not let the browser claim "I am step-up verified" in a value it can edit.
- **Session id in the URL** is a defect. URLs leak through history, bookmarks, referrer headers, and support screenshots.
- **Binding extras with care.** Some systems also check device or network fingerprints. This can detect theft and can also annoy genuine customers on changing mobile networks. It is a control to tune, not a magic wall.
- **Staff sessions** need shorter idle times and stronger audit, because one staff session can see many customers.

### Session versus token

| | Server session | Token such as JWT |
| --- | --- | --- |
| What the browser holds | A random id | A signed token that may contain user id and expiry |
| Where the detail lives | On the server | Often inside the token |
| Revoking immediately | Delete the server session | Harder, unless you keep a blocklist or keep tokens very short-lived |
| Fits | Traditional web channels | APIs and mobile apps, especially with an API gateway |

**JWT** means JSON Web Token. It is a signed pack of claims. Anyone can read the payload unless it is also encrypted. The signature stops undetected tampering. A JWT is not a place for secrets. A stolen JWT is a stolen login until it expires, so lifetime and revocation matter.

**Say it in one line:** A secure session uses a fresh random id after login, stores the real data on the server, sends the id only in a Secure, HttpOnly, SameSite cookie, times out, and dies properly on logout.

---

## A few more controls that complete the picture

These are not extra syllabus headings. They are how the headings are practised.

| Control | Plain meaning |
| --- | --- |
| Least privilege | Give each user, staff role, and system account only the access it needs |
| Defence in depth | Several controls. TLS plus authentication plus authorisation plus monitoring |
| Input validation | Check that input is the shape you expect, on the server |
| Output encoding | When showing user-supplied text, display it as text so it cannot become a script |
| Parameterised query | Keep user input out of the SQL sentence structure |
| WAF | Web Application Firewall. A filter in front of the app that blocks many malicious patterns. Useful, and not a substitute for safe code |
| DDoS protection | Distributed denial of service floods the site with traffic. Protection absorbs or filters that flood so genuine customers can enter |
| Rate limiting | Only so many login tries or API calls from one source in a period |
| Captcha | A challenge that is easier for humans than for mass scripts. Use with care for accessibility |
| Security headers | Small instructions to the browser: HSTS, content security policy, frame restrictions, and no-sniff |
| Secrets management | Passwords and keys live in a vault, not in source code or chat messages |
| Penetration test | Authorised experts try to break the system and report findings |
| Vulnerability assessment | A scan for known weaknesses |
| Patch management | Applying fixes in a governed, timely way |
| Incident response | A rehearsed plan for detecting, containing, notifying, and recovering |

### Privacy beside security

Security keeps data away from attackers. Privacy also limits how the bank itself collects, uses, and shares data. India's Digital Personal Data Protection Act, 2023, is the legal frame to be aware of: purpose, consent where required, limitation, and careful handling of personal data. You do not need section numbers for this technical syllabus. You need the instinct that customer data is not a free raw material.

---

## One attack, told with every control visible

A fraudster on public Wi-Fi wants to steal a session and also wants to inject a fake query.

| Step of the attack | What stops it |
| --- | --- |
| Read the password on the Wi-Fi path | TLS via HTTPS |
| Redirect the customer to a plain HTTP copy | HSTS and HTTPS-only cookies |
| Present a fake server | Certificate name and CA checks in the browser |
| Guess the password forever | MFA, rate limit, lockout |
| Plant a session id before login | Regenerate the session id at login |
| Steal the cookie with a script in a nickname | Output encoding against XSS, plus HttpOnly |
| Trigger a transfer from another website | SameSite cookie, CSRF token, step-up OTP |
| Change the account number in the request to someone else's | Server-side authorisation, the heart of fixing broken access control |
| Change the SQL by typing commands into the account field | Parameterised queries |
| Do all this quietly | Security logs, alerts, and a person who looks at them |

---

## Security mistakes that become exam traps

| Trap | The clear idea |
| --- | --- |
| "HTTPS encrypts the data inside the database" | HTTPS protects data in transit. Database protection is a separate control |
| "SSL and TLS are two current choices" | SSL is retired. TLS is the current protocol. The phrase "SSL certificate" is loose speech |
| "The padlock proves the website is the bank and is safe" | The padlock proves a valid certificate for that domain and a protected pipe. A criminal can have a certificate for a different domain |
| "OWASP Top 10 is a law or a complete audit" | It is a community priority list for web application risks |
| "Hiding the admin button is access control" | Access control is a server-side check on every action |
| "Logout is complete when the app forgets the cookie" | The server must invalidate the session |
| "A WAF means the code can stay unsafe" | A WAF is one layer. Injection must still be fixed in the code |

## Remember these lines

- CIA: confidentiality, integrity, availability.
- OWASP is the Open Worldwide Application Security Project. Its Top 10 highlights the most critical web risks.
- Broken access control means the server did not enforce who may do what.
- Injection, especially SQL injection, is stopped by keeping data separate from commands.
- HTTPS is HTTP over TLS. TLS replaced SSL.
- A certificate ties a name to a key and is signed by a CA the browser trusts.
- Secure sessions: random id, regenerate on login, HttpOnly, Secure, SameSite, server-side timeout, server-side logout.
