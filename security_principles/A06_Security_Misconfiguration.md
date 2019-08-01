### A06 - Security Misconfiguration
>Security misconfiguration is the most commonly seen issue. This is commonly a result of insecure default configurations, incomplete or ad hoc configurations, open cloud storage, misconfigured HTTP headers, and verbose error messages containing sensitive information. Not only must all operating systems, frameworks, libraries, and applications be securely configured, but they must be patched/upgraded in a timely fashion.

Security misconfiguration can happen at any level of an app stack, incl network services, platform, web server, application server, db, frameworks, custom code, & pre-installed virtual machines, containers, or storage.
Automated scanners are useful for detecting misconfigurations, use of default accounts or configurations, unnecessary services, legacy options, etc.

## Vulnerable?
- Missing appropriate security hardening across any part of app stack, or improperly configured permissions on cloud services.
- Unnec. features enabled or installed (e.g. unnecessary ports, services, pages, accounts, or privileges).
- Default accounts & passwords still enabled & unchanged.
- Error handling reveals stack traces or other overly informative error messages to users.
- For upgraded systems, latest security features are disabled or not configured securely.
- Security settings in app servers, app frameworks (e.g. Struts, Spring, ASP.NET), libraries, dbs, etc. not set to secure values.
- Server does not send security headers or directives or not set to secure values.
- Software is out of date or vulnerable

## Prevention
- Repeatable hardening process that makes it fast & easy to deploy another environment that's properly locked down. Development, QA, & production environments should all be configured identically, w different credentials used in each. Process should be automated to minimise effort required.
- A minimal platform without any unnec. features, components, docs, & samples. Remove or do not install unused features & frameworks.
- Review and update the configurations appropriate to all security notes, updates and patches as part of the patch management process, esp. cloud storage permissions (e.g. S3 bucket).
- A segmented app architecture that provides effective, secure separation between components or tenants, w segmentation, containerisation, or cloud security groups (ACLs).
- Sending security directives to clients, e.g. Security Headers.

ACLs - Access Control List (list of attached permissions)

Source: https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration
