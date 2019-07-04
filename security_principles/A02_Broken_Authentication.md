### A02 - Broken Authentication
>Application functions related to authentication and session management are often implemented incorrectly, allowing attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to assume other users' identities temporarily or permanently.

## Threat Agents/Attack Vectors:
Attackers get username/passwords for credential stuffing (large-scale automated login requests)/ default administrative account lists/ automated brute force/dictionary attack tools
(* dictionary attack = aka brute force attack technique for defeating a cipher/authentication mechanism by determining decryption key or passphrase by 100s/millions attempts, e.g. words in a dictionary)

## Security Weaknesses
Vulnerability of session management

## Impact
Potential access to admin account/money laundering/social security fraud/identity theft/sensitive info disclosure


## Assess Vulnerability
identity/auth/session management vital
Vulnerable if app:
- Permits automated attacks e.g. credential stuffing
- Permits brute force attacks
- Permits default/weak/well-known passwords
- Uses weak credential recovery and forgot-password processes, such as "knowledge-based answers"
- Uses plain text/encrypted/weakly hashed passwords
- Has missing/ineffective multi-factor authentication
- Exposes Session IDs in URL (e.g. URL rewriting)
- Doesn't rotate Session IDs after login
- Doesn't properly invalidate Session IDs. User sessions or authentication tokens (esp single sign-on (SSO) tokens) aren't properly invalidated during logout or period of inactivity

## Prevention
- MFA vs automated/credential stuffing/brute force/stolen credential re-use attacks
- Don't ship or deploy w default credentials, esp for admins
- Check for weak passwords
- Align password length, complexity & rotation policies with NIST 800-63 B's guidelines,section 5.1.1 Memorized Secrets (National Institute of Standards and Technology, 2017. e.g. min 8, max 64; don't require hints or special rules)
- Ensure registration/credential recovery/API pathways hardened vs account enumeration attacks by using same messages for each
- Limit/delay failed logins. Log failures & alert admins of attacks
- Use server-side, secure, built-in session manager that generates new random session ID ws high entropy after login. Session IDs should not be in the URL, be securely stored and invalidated after logout, idle, and absolute timeouts.

(SIs usu. generated w pseudo-random number generator (PRNG). If a PRNG doesn't yield enough entropy (randomness), susceptible to statistical analysis. If attacker predicts valid session identifier, corresponding session can be immediately hijacked)
