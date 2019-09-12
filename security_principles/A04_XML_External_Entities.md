### A04 - XML External Entities (XXE)
>Many older or poorly configured XML processors evaluate external entity references within XML documents. External entities can be used to disclose internal files using the file URI handler, internal file shares, internal port scanning, remote code execution, and denial of service attacks. ie. attack occurs when XML input containing a reference to an external entity is processed by a weakly configured XML parser.

[OWASP A04 overview](https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE))

**Definitions**
- **XML** - eXtensible Markup Language (to store/transport data)
- **DTD** - Document Type Definitions (valid building blocks for XML doc)
- **SAML** - Security Assertion Markup Language (exchanging auth data)
- **SOAP** - Simple Object Access Protocol (XML-based messaging protocol between computers)
- **SSO** - Single Sign On
- **WAF** - Web app firewall (filter/monitor/block HTTP traffic)

#### Vulnerable?
- App accepts XML directly or XML uploads, esp from untrusted sources, or inserts untrusted data into XML documents, parsed by XML processor.
- Any of the XML processors in the app or SOAP based web services has document type definitions (DTDs) enabled.
- If the app uses SAML for identity processing within federated security or single sign on (SSO) purposes. SAML uses XML for identity assertions, & may be vulnerable.
- If the application uses SOAP prior to version 1.2, it is likely susceptible to XXE attacks
- Being vulnerable to XXE attacks likely means that the app is vulnerable to denial of service attacks (e.g. incl 'endless file') including the Billion Laughs attack (https://en.wikipedia.org/wiki/Billion_laughs_attack) - XML entity-expansion

#### Prevention
- Whenever poss, use less complex data formats such as JSON, & avoid serialisation of sensitive data.
- Patch or upgrade all XML processors and libraries in use by app &  underlying operating system. Use dependency checkers. Update SOAP to SOAP 1.2 or higher.
- Disable XML external entity and DTD processing in all XML parsers in the app
- Implement positive ("whitelisting") server-side input validation, filtering, or sanitisation to prevent hostile data within XML documents, headers, or nodes.
- Verify that XML or XSL file upload functionality validates incoming XML using XSD validation or similar.
- SAST tools can help detect XXE in source code, although manual code review is the best alternative in large, complex applications with many integrations.
If these controls not poss, can use virtual patching, API security gateways, or Web Application Firewalls (WAFs) to detect/monitor/block XXE attacks.
___

- Source: https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)

- More info: https://cheatsheetseries.owasp.org/cheatsheets/XML_Security_Cheat_Sheet.html
