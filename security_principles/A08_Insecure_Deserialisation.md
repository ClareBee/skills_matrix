### A08 - Insecure Deserialisation
>Insecure deserialisation often leads to remote code execution. Even if deserialisation flaws do not result in remote code execution, they can be used to perform attacks, including replay attacks, injection attacks, and privilege escalation attacks.

Serialisation = converting object into format persistible to disk (e.g. file), sendable through streams (e.g. stdout) or over network. - esp JSON/XML (ALSO: Remote- and inter-process communication (RPC/IPC)/Wire protocols/ web services/ message brokers/ Caching/Persistence/ Databases/ cache servers/ file systems/ HTTP cookies/ HTML form params/ API auth tokens
Deserialisation = transforming serialised data coming from a file, stream or network socket into an object.
E.g.s of Insecure/Untrusted Deserialisation:
- 2016 Java Deserialisation Apocalypse (see related https://github.com/mbechler/marshalsec)
- Struts 2 remote execution incident, 2017, the attack vector exploited in Equifax hack.

### Vulnerable?
- if deserialising hostile or tampered objects supplied by attacker.
a) Object & data structure-related attacks where attacker modifies app logic or achieves arbitrary remote code execution if classes available to app that change behaviour during or after deserialisation.
b) Typical data-tampering attacks e.g. access-control-related attacks where existing data structures used but content changed.

### Prevention
- Don't accept serialised objects from untrusted sources
- Use serialisation mediums that only permit primitive data types.
- Integrity checks, eg digital signatures, on serialised objects to prevent tampering/hostile object creation
- strict type constraints during deserialisation
- isolate/run deserialisation in low privilege env wherever poss
- Log exceptions & failures
- Restrict/monitor incoming & outgoing network connectivity from containers or servers that deserialise.

Source: https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization
Cheatsheet: https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html
