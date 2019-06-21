### A01 - Injection

>Injection flaws, such as SQL, NoSQL, OS, and LDAP injection, occur when untrusted data is sent to an interpreter as part of a command or query. The attacker's hostile data can trick the interpreter into executing unintended commands or accessing data without proper authorisation.

*(LDAP - Lightweight Directory Access Protocol)*

Example:
Untrusted user input:
```java
String query = "SELECT * FROM accounts WHERE custID='" + request.getParameter("id") + "'";
```

#### Vulnerabilities
- user-provided data not sanitised/validated/filtered
- dynamic queries (i.e. at runtime rather than at compilation, usu w user input) or non-parameterised calls w/o context-aware escaping used directly in interpreter
- hostile data used w/in object-relational mapping (ORM) search parameters to extract additional, sensitive records
- hostile data directly used or concatenated, so SQL or command contains both structure + hostile data in dynamic queries/commands/stored procedures

#### Detection
- code review + automated testing of all params, headers, URL, cookies, JSON, SOAP, + XML data inputs. (CI/CD pipeline to include static source (SAST) + dynamic application test (DAST) tools)

#### Prevention
- use a safe API, avoiding use of interpreter entirely or via parameterised interface
- migrate to use Object Relational Mapping Tools (ORMs)
>Note: Even when parameterised, stored procedures can still introduce SQL injection if PL/SQL or T-SQL concatenates queries and data, or executes hostile data with EXECUTE IMMEDIATE or exec().

- "whitelist" server-side input validation.
- in dynamic queries, escape special characters
>Note: SQL structure such as table names, column names, and so on cannot be escaped, and thus user-supplied structure names are dangerous. This is a common issue in report-writing software.

- Use LIMIT + other SQL controls w/in queries to prevent mass disclosure of records
