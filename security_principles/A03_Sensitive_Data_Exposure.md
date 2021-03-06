### A03 - Sensitive Data Exposure
>Many web applications and APIs do not properly protect sensitive data, such as financial, healthcare, and PII. Attackers may steal or modify such weakly protected data to conduct credit card fraud, identity theft, or other crimes. Sensitive data may be compromised without extra protection, such as encryption at rest or in transit, and requires special precautions when exchanged with the browser.

[OWASP A03 overview](https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure)

- manual attack usu needed, facilitated by unencrypted data/weak algorithms

#### Vulnerable?
- Determine protection needs of data in transit & at rest. e.g. passwords/credit card numbers etc. require extra protection, esp if privacy laws, e.g. EU's General Data Protection Regulation (GDPR)/regulations, e.g. financial data protection such as PCI Data Security Standard (PCI DSS).

- Data transmitted in clear text? (HTTP, SMTP, and FTP protocols). External internet traffic is esp dangerous. Verify all internal traffic e.g. between load balancers/web servers/back-end systems.
- Any old or weak cryptographic algorithms?
- Default crypto keys in use, weak crypto keys generated or re-used, proper key management or rotation missing?
- Encryption not enforced, e.g. any user agent (browser) security directives or headers missing?
- Does user agent (e.g. app, mail client) not verify if received server certificate is valid?

#### Prevention
- Classify data processed, stored or transmitted. Identify which data is sensitive acc. to privacy laws, regulatory requirements, or business needs.
- Apply controls as per classification.
- Don't store sensitive data unnecessarily. Discard it asap or use PCI DSS compliant tokenisation/truncation.
- Encrypt all sensitive data at rest.
- Ensure up-to-date and strong standard algorithms, protocols, and keys use proper key management.
- Encrypt all data in transit with secure protocols such as TLS w perfect forward secrecy (PFS) ciphers (vs man in the middle), cipher prioritisation by the server, and secure parameters. Enforce encryption e.g. HTTP Strict Transport Security (HSTS).
- Disable caching for response containing sensitive data.
- Store passwords using strong adaptive & salted hashing functions with a work factor (delay factor), such as Argon2, scrypt, bcrypt or PBKDF2.
- Verify independently.

**TLS**
  - [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) validated (security of cryptomodule & its services)
  - Use Secure cookie flag
  - Only support strong protocols
  - Prefer ephemeral key exchanges (based on Diffie-Hellman and use per-session, temporary keys during initial SSL/TLS handshake)
  - Support TLS-PSK (pre-shared key) & TLS-SRP (secure remote password) for mutual auth
  - Don't mix tls w non-tls
  - Don't put sensitive info in url
  - Don't use wildcard certificates
  - Don't use RFC 1989 addresses in certificates (e.g. 92.168/16, 172.16/12, and 10/8.)
  - Have SHA-1 deprecation plan
  Basic requirements:
  - access to a Public Key Infrastructure (PKI) in order to obtain certificates
  - access to directory or Online Certificate Status Protocol (OCSP) responder in order to check certificate revocation status
  - agreement/ability to support a minimum configuration of protocol versions and protocol options for each version)
Source: https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html

**Digital Certificate Pinning**
>Certificate Pinning is the practice of hardcoding or storing a pre-defined set of information (usually hashes) for digital certificates/public keys in the user agent (be it web browser, mobile app or browser plugin) such that only the predefined certificates/public keys are used for secure communication, and all others will fail, even if the user trusted (implicitly or explicitly) the other certificates/public keys.

Source: https://cheatsheetseries.owasp.org/cheatsheets/User_Privacy_Protection_Cheat_Sheet.html

**Privacy supported by:**
  - Strong cryptography (auth, nonrepudiation, confidentiality, integrity)
  - HSTS - HTTP Strict Transport Security
  - Digital Cert Pinning (see above)
  - Panic Modes
  - Remote Session Invalidation
  - Tor etc Anonymity Networks
  - Prevent IP Address Leakage
  - Password protection (see https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)


### Rails specific:
**Sensitive Files**
- certain files that should be either excluded or carefully managed.

/config/database.yml                 -  May contain production credentials.
/config/initializers/secret_token.rb -  Contains a secret used to hash session cookie.
/db/seeds.rb                         -  May contain seed data including bootstrap admin user.
/db/development.sqlite3              -  May contain real data.

**Encryption**
Devise => bcrypt for password hashing
___

Source: https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Ruby_on_Rails_Cheatsheet.md
