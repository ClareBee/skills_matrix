### A05 - Broken Access Control
>Restrictions on what authenticated users are allowed to do are often not properly enforced. Attackers can exploit these flaws to access unauthorised functionality and/or data, such as access other users' accounts, view sensitive files, modify other users' data, change access rights, etc.

- common due to the lack of automated detection & effective functional testing
- best detected by manual testing, including HTTP method (GET vs PUT, etc), controller, direct object references, etc.

## Vulnerable?
- Bypassing access control checks by modifying the URL, internal application state, or HTML page, or simply using a custom API attack tool.
- Allowing primary key to be changed to another's users record, permitting viewing or editing someone else's account.
- Elevation of privilege. Acting as a user without being logged in, or acting as an admin when logged in as a user.
- Metadata manipulation, such as replaying or tampering with a JSON Web Token (JWT) access control token or a cookie or hidden field manipulated to elevate privileges, or abusing JWT invalidation
- CORS misconfiguration allows unauthorised API access.
- Force browsing to authenticated pages as an unauthenticated user or to privileged pages as a standard user. Accessing API with missing access controls for POST, PUT and DELETE.

## Prevention
- With the exception of public resources, deny by default.
- Implement access control mechanisms once and re-use them throughout the application, including minimising CORS usage.
- Model access controls should enforce record ownership
- Unique application business limit requirements should be enforced by domain models.
- Disable web server directory listing and ensure file metadata (e.g. .git) and backup files are not present within web roots.
- Log access control failures, alert admins when appropriate (e.g. repeated failures).
- Rate limit API and controller access to minimise the harm from automated attack tooling.
- JWT tokens should be invalidated on the server after logout.
- Developers and QA staff should include functional access control unit and integration tests.
Source: https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control

Access control involves the use of several protection mechanisms such as:
- Authentication (proving the identity of an actor)
- Authorisation (ensuring that a given actor can access a resource), and
- Accountability (tracking of activities that were performed)
Source: https://cwe.mitre.org/data/definitions/284.html


### Role Based Access Control (RBAC)
**Pros:**
- Roles assigned re: organisational structure w emphasis on its security policy
- Easy to use & administer
- Built into most frameworks
- Aligns with security principles like segregation of duties and least privileges

**Cons:**
- Documentation of roles/accesses has to be maintained stringently.
- Multi-tenancy can not be implemented effectively unless there is a way to associate the roles with multi-tenancy capability requirements e.g. OU in Active Directory
- Tendency for scope creep e.g. more accesses and privileges can be given than intended for.
- Does not support data based access control

**Note:**
- Roles must be only be transferred or delegated using strict sign-offs & procedures.
- When a user changes her role to another one, admin must make sure that earlier access is revoked (need to know basis).
- Assurance for RBAC must be carried out using strict access control review

### Discretionary Access Control (DAC)
- based on the identity of users and/or membership in certain groups. Access decisions are typically based on authorisations granted to user based on the credentials (user name, password, hardware/software token, etc.) Usu owner of info or any resource is able to change its permissions at her discretion

**Pros:**
- Easy to use & administer
- Fine grained access control possible
- Aligns to principle of least privileges.
- Object owner has total control over access granted

**Cons:**
- Documentation of roles/accesses has to be maintained stringently.
- Multi-tenancy can not be implemented effectively unless there is a way to associate the roles with multi-tenancy capability requirements e.g. OU in Active Directory
- Tendency for scope creep to happen e.g. more accesses and privileges can be given than intended for.

**Note:**
-Assurance for DAC must be carried out using strict access control reviews.

## Mandatory Access Control (MAC)
- ensures that the enforcement of organisational security policy does not rely on voluntary web app user compliance. MAC secures info by assigning sensitivity labels on info and comparing this to the level of sensitivity a user is operating at. MAC is usually appropriate for extremely secure systems including multilevel secure military applications or mission critical data applications.

**Pros:**
- Access based on sensitivity of object
- Access based on need to know is strictly adhered to & scope creep has minimal possibility
- Only an admin can grant access

**Cons:**
- Difficult & expensive to implement
- Not agile

**Note:**
- Assurance for MAC must ensure classification of objects is at appropriate level.

### Permission Based Access Control
- abstraction of application actions into a set of permissions e.g. string based name, "READ". Access decisions made by checking if current user has permission associated w requested application action.

- direct relationship between the user and permission (called a grant), or an indirect one, where permission grant is given to an intermediate entity such as user group (easier to manage for many users who inherit permissions).
- classes of permissions possible eg document permissions - read/write/delete, server permissions - start/stop/reboot

Source: https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html
