### A09 - Using Components With Known Vulnerabilities
>Components, such as libraries, frameworks, and other software modules, run with the same privileges as the application. If a vulnerable component is exploited, such an attack can facilitate serious data loss or server takeover. Applications and APIs using components with known vulnerabilities may undermine application defences and enable various attacks and impacts.

### Vulnerable?
- If you don't know versions of all components used (client-side & server-side) - direct use/nested dependencies
- If software is vulnerable/unsupported/out of date. Incl. OS, web/app server, DB management system (DBMS), APIs & all components, runtime environments, & libraries.
- If you don't scan regularly or subscribe to security bulletins.
- If you don't fix/upgrade underlying platform, frameworks, & dependencies 'in a risk-based, timely fashion'.
- If you don't test compatibility of updated/upgraded/patched libraries.
- If you don't secure components' config.

### Prevention
Patch management process to:
- Remove unused dependencies, unnec.features, components, files, & docs.
- Continuously inventory versions of both client-side & server-side components (e.g. frameworks, libraries) & dependencies using tools like versions, DependencyCheck, retire.js, etc.
- Continuously monitor sources like CVE & NVD for vulnerabilities.
- Use software composition analysis tools to automate.
- Subscribe to email alerts for security vulnerabilities.
- Only obtain components from official sources over secure links.
- Prefer signed packages to reduce chance of including a modified, malicious component.
- Monitor for unmaintained libraries & components or w/o security patches for older versions.
- If patching not poss, deploy virtual patch to monitor, detect, or protect against discovered issue.
>Every organisation must ensure that there is an ongoing plan for monitoring, triaging, and applying updates or configuration changes for the lifetime of the application or portfolio.

Source: https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities

See also: https://www.owasp.org/index.php/OWASP_Dependency_Check
Retire.js - https://github.com/retirejs/retire.js/
National Vulnerability Database (NVD) - https://nvd.nist.gov/
Common Vulnerabilities & Exposures (CVE) - https://cve.mitre.org/
RubySec - https://rubysec.com/ incl bundler-audit

PDF - https://cdn2.hubspot.net/hub/203759/file-1100864196-pdf/docs/Contrast_-_Insecure_Libraries_2014.pdf
