### A10 - Insufficient Logging and Monitoring
>Insufficient logging and monitoring, coupled with missing or ineffective integration with incident response, allows attackers to further attack systems, maintain persistence, pivot to more systems, and tamper, extract, or destroy data. Most breach studies show time to detect a breach is over 200 days, typically detected by external parties rather than internal processes or monitoring.

- included in OWASP Top 10 re: industry survey.
- good idea to check following penetration testing!
>In 2016, identifying a breach took an average of 191 days – plenty of time for damage to be inflicted.

#### Vulnerable?
Insufficient logging/detection/monitoring/active response occurs any time:

- Auditable events, such as logins, failed logins, & high-value transactions aren't logged.
- Warnings & errors generate no/inadequate/unclear log msgs.
- Logs of apps & APIs aren't monitored for suspicious activity.
- Logs only stored locally.
- Appropriate alerting thresholds & response escalation processes aren't in place or effective.
- Penetration testing & scans by DAST tools (such as OWASP ZAP) don't trigger alerts.
- App unable to detect/escalate/alert for active attacks in real time or near real time.
- Vulnerable to info leakage if logging & alerting events visible to a user or attacker (cf. Sensitive Data Exposure).

#### Prevention
- Ensure all login/access control failures/server-side input validation failures logged w sufficient user context to identify suspicious or malicious accounts, & held for sufficient time to allow delayed forensic analysis.
- Ensure logs generated in format easily consumed by centralised log management solutions.
- Ensure high-value transactions have audit trail w integrity controls to prevent tampering or deletion, such as append-only database tables or similar.
- Establish effective monitoring & alerting such that suspicious activities are detected & responded to quickly.
- Establish or adopt an incident response and recovery plan, such as NIST 800-61 rev 2 or later.
- Commercial and OS app protection frameworks such as OWASP AppSensor, web app firewalls such as ModSecurity w OWASP ModSecurity Core Rule Set, & log correlation software w custom dashboards & alerting.

___
Source: https://www.owasp.org/index.php/Top_10-2017_A10-Insufficient_Logging%26Monitoring
___

**Why log?**
- Identify security incidents
- Monitor policy violations
- Establish baselines
- Assist non-repudiation controls
- Provide info about problems & unusual conditions
- Contribute additional app-specific data for incident investigation

**Monitoring:**
- security, business, audit, performance, compliance, GDPR requests etc.

#### Design/Implementation/Testing

**Event data sources:**
Primarily the app code itself; client software; app firewalls; network firewalls; network & host intrusion detection systems (NIDS and HIDS); database apps; OS monitoring; fraud monitoring etc.

**Where to record:**
Usu to separate database/partitioned filesystem
centralised?
>Use standard formats over secure protocols to record and send event data, or log files, to other systems e.g. Common Log File System (CLFS) or Common Event Format (CEF) over syslog; standard formats facilitate integration with centralised logging services

**Which events:**
- Input validation failures e.g. protocol violations, unacceptable encodings, invalid parameter names & values
- Output validation failures e.g. database record set mismatch, invalid data encoding
- Authentication successes & failures
- Authorisation (access control) failures
- Session management failures e.g. cookie session identification value modification
- Application errors & system events e.g. syntax and runtime errors, connectivity problems, performance issues, third party service error messages, file system errors, file upload virus detection, configuration changes
- Application and related systems start-ups and shut-downs, and logging initialisation (starting, stopping or pausing)
- Use of higher-risk functionality e.g. network connections, addition or deletion of users, changes to privileges, assigning users to tokens, adding or deleting tokens, use of systems admin privileges, etc.

**Event attributes:**
when, where, who, what, codes etc

**Exclude:**
access tokens, source code, sensitive data, session identification values, db connection strings, encryption keys, etc.
---
Source: https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html
