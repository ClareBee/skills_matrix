### A02 - Broken Authentication
>Application functions related to authentication and session management are often implemented incorrectly, allowing attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to assume other users' identities temporarily or permanently.

### Threat Agents/Attack Vectors:
Attackers get username/passwords for credential stuffing (large-scale automated login requests)/ default administrative account lists/ automated brute force/dictionary attack tools
(* dictionary attack = aka brute force attack technique for defeating a cipher/authentication mechanism by determining decryption key or passphrase by 100s/millions attempts, e.g. words in a dictionary)

### Security Weaknesses
Vulnerability of session management -> session hijacking/sidejacking (via packet sniffer/cookie theft) (targeted eg admin, generic eg avg user)

### Impact
Potential access to admin account/money laundering/social security fraud/identity theft/sensitive info disclosure

#### Vulnerable?
Identity/auth/session management vital. Vulnerable if:
- Permits automated attacks e.g. credential stuffing
- Permits brute force attacks
- Permits default/weak/well-known passwords
- Uses weak credential recovery and forgot-password processes, such as "knowledge-based answers"
- Uses plain text/encrypted/weakly hashed passwords
- Has missing/ineffective multi-factor authentication
- Exposes Session IDs in URL (e.g. URL rewriting)
- Doesn't rotate Session IDs after login
- Doesn't properly invalidate Session IDs. User sessions or authentication tokens (esp single sign-on (SSO) tokens) aren't properly invalidated during logout or period of inactivity

#### Prevention
- MFA vs automated/credential stuffing/brute force/stolen credential re-use attacks
- Don't ship or deploy w default credentials, esp for admins
- Check for weak passwords
- Align password length, complexity & rotation policies with NIST 800-63 B's guidelines, section 5.1.1 Memorized Secrets (National Institute of Standards and Technology, 2017. e.g. min 8, max 64; don't require hints or special rules)
- Ensure registration/credential recovery/API pathways hardened vs account enumeration attacks by using same messages for each
- Limit/delay failed logins. Log failures & alert admins of attacks
- Use server-side, secure, built-in session manager that generates new random session ID ws high entropy after login. Session IDs should not be in the URL, be securely stored and invalidated after logout, idle, and absolute timeouts.

##### SIs usu. generated w pseudo-random number generator (PRNG). If not enough entropy (randomness), susceptible to statistical analysis. If attacker predicts valid session identifier, corresponding session can be immediately hijacked

----
### Background:

#### Web Session
- sequence of (stateless) network HTTP request and response transactions associated to same user
- ability to establish variables – e.g. access rights/localization for session duration
- session ID/token = name:value pair on session creation binds auth credentials (i.e. as a user session) to  user HTTP traffic & appropriate access controls enforced by the app

#### Session ID cont.
- SI fingerprinting -> rename default to id (as common SI names easily fingerprinted)
- SI length -> at least 128 bits (16 bytes) vs brute force attacks
- SI entropy -> good PRNG vs guessing
- SI content -> never include sensitive info & use cryptographic hash eg SHA56

#### Session Management Implementation:
>Stealing a user's session ID lets an attacker use the web application in the victim's name. - Rails Guide

SMI = SI exchange-mechanism between user + app e.g. cookies (standard HTTP header), URL params, URL args on GET requests, body args on POST requests, such as hidden form fields (HTML forms), or proprietary HTTP headers
Best practice = include token expiration date/time, usage constraints
Danger = if ID in URL could disclose session ID (web logs + links/browser history/referrer header/manipulation of ID/session fixation)

#### Transport Layer Security:
- HSTS Protects against active eavesdropping & passive disclosure in network traffic
- Secure cookie attribute used to ensure session ID only exchanged through encrypted channel
- Also protects against some session fixation attacks where attacker is able to intercept/manipulate web traffic to inject (or fix) the session ID on victims browser
- Apps should never switch a given session from HTTP to HTTPS, or vice versa, as this will disclose the session ID in the clear through the network
- Apps shouldn't mix encrypted + unencrypted contents (HTML pages, images, CSS, Javascript files, etc) on same host/domain, as request of any web object over unencrypted channels might disclose session ID
- Apps shouldn't offer public unencrypted contents + private encrypted contents from same host (instead e.g. www.example.com over HTTP (unencrypted + port TCP/80) for public contents + secure.example.com over HTTPS (encrypted + port TCP/443) for private/sensitive contents (where sessions exist))
- Apps should avoid extremely common HTTP to HTTPS redirection on home page (using a 30x HTTP response), as this single unprotected HTTP request/response exchange can be used by an attacker to gather (or fix) a valid session ID

*NB - SSL/TLS doesn't protect against SI prediction/brute force/client-side tampering/fixation BUT does protect against disclosure/capture from network traffic*

#### Cookies
> Another sort of attack you have to be aware of when using CookieStore is the replay attack. Rails Guides => avoided by nonce

**Secure Attribute**
- instructs web browsers to only send cookie through encrypted HTTPS (SSL/TLS) connection, vs MitM (Man-in-the-Middle) attacks (interception of SI from web browser traffic)

**HttpOnly Attribute**
- instructs web browsers not to allow scripts (e.g. JavaScript or VBscript) ability to access cookies via DOM document.cookie object vs XSS attacks

**SameSite Attribute**
- allows a server to define a cookie attribute preventing the browser from sending this cookie along w cross-site requests, vs risk of cross-origin info leakage/cross-site request forgery attacks

**Domain and Path Attributes**
- instructs web browsers to only send cookie to specified domain/subdomains. If not set, by default cookie only sent to origin server.
- recommended to use a narrow/restricted scope (Domain attribute shouldn't be set (restricting cookie just to origin server + Path attribute should be set as restrictive as possible to the web application path that makes use of the session ID)
- don't mix web apps of diff security levels on same domain (vulnerable to session fixation attacks) or host

*Cookies vulnerable to DNS spoofing/hijacking/poisoning attacks, where an attacker can manipulate the DNS resolution to force browser to disclose session ID for a given host/domain.*

**Expire and Max-Age Attributes**
- non-persistent (or session) cookies + persistent cookies.
persistent cookie = Max-Age or Expires attributes, stored on disk by  browser until expiration time.
- Tracking users after auth usu. via non-persistent = forces session closure on browser closure (vs cached)
- Ensure sensitive info encrypted/not persisted/stored on a need basis for as short a time as poss.
- Ensure that unauthorised activities impossible via cookie manipulation
- Ensure secure flag is set to prevent accidental transmission over “the wire” in a non-secure manner
- Determine if all state transitions in app code properly check for cookies and enforce use
- Define all cookies being used by the app, their name + why they're needed

----
### Rails-specific:
---
Default Cookie-based session store = no expiry, possibly vuln. to replay attacks
Sensitive information should never be put in session.
nonce avoids replay attacks
reset_session & expiry avoids session fixation

Best practice = database-based session:
```ruby
Project::Application.config.session_store :active_record_store
```

## Auth:
**Authentication**
Enable TLS in config:
```ruby
# config/environments/production.rb
# Force all access to the app over SSL, use Strict-Transport-Security,
# and use secure cookies
config.force_ssl = true
```
Usu w Devise gem
`$ gem 'devise'`
`$ rails generate devise:install`
```ruby
Rails.application.routes.draw do
  authenticate :user do
    resources :something do  # these resource require authentication
      ...
    end
  end

  devise_for :users # sign-up/-in/out routes

  root to: 'static#home' # no authentication required
end
```
Password complexity via zxcvbn gem
```ruby
class User < ApplicationRecord
  devise :database_authenticatable,
    # other devise features, then
    :zxcvbnable
end

# in config/initializers/devise.rb
Devise.setup do \|config\|
  # zxcvbn score for devise
  config.min_password_score = 4 # complexity score here.
  ...
```
Token Authentication possible w devise_token_auth gem w omniauth as dependency:
```ruby
# token-based authentication
$ gem 'devise_token_auth'
$ gem 'omniauth'
```
Then a route is defined:
`mount_devise_token_auth_for 'User', at: 'auth'`
or in a one-r
`$ rails g devise_token_auth:install [USER_CLASS] [MOUNT_PATH]`
