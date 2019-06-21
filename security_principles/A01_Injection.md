### A01 - Injection

![bobby tables](https://imgs.xkcd.com/comics/exploits_of_a_mom.png)

>Injection flaws, such as SQL, NoSQL, OS, and LDAP injection, occur when untrusted data is sent to an interpreter as part of a command or query. The attacker's hostile data can trick the interpreter into executing unintended commands or accessing data without proper authorisation.

*(LDAP - Lightweight Directory Access Protocol)*
In-Band (results visible to hacker on same screen as input), Blind (result not visible but guessable indirectly e.g. from failed pages/response times), Out-of-Band (db forced to make DNS request to hacker's site)
Example:
Untrusted user input:
```java
String query = "SELECT * FROM accounts WHERE custID='" + request.getParameter("id") + "'";
```

#### Vulnerabilities
- user-provided data not sanitised/validated/filtered/restricted
- dynamic queries (i.e. at runtime rather than at compilation, usu w user input) or non-parameterised calls w/o context-aware escaping used directly in interpreter
- hostile data used w/in object-relational mapping (ORM) search parameters to extract additional, sensitive records
- hostile data directly used or concatenated, so SQL or command contains both structure + hostile data in dynamic queries/commands/stored procedures

#### Detection
- code review + automated testing of all params, headers, URL, cookies, JSON, SOAP, + XML data inputs. (CI/CD pipeline to include static source (SAST, ~ whitebox) + dynamic application test (DAST, ~ blackbox) tools)

#### Prevention
- use a safe API, avoiding use of interpreter entirely or via parameterised interface
- prepared statements/parameterised statements: using templates with '?' params (var bindings) (also more efficient)
e.g.
```ruby
Person.find_by_sql ['SELECT * from persons WHERE name = ?', name]
```
or
```ruby
Model.where("login = ? AND password = ?", entered_user_name, entered_password).first
```
- stored procedures
>The difference between prepared statements and stored procedures is that the SQL code for a stored procedure is defined and stored in the database itself, and then called from the application.

- migrate to use Object Relational Mapping Tools (ORMs) (e.g. Sequelize)
>Note: Even when parameterised, stored procedures can still introduce SQL injection if PL/SQL or T-SQL concatenates queries and data, or executes hostile data with EXECUTE IMMEDIATE or exec(). ORMs NOT enough.

- "whitelist" server-side input validation.
- in dynamic queries, escape special characters
>Note: SQL structure such as table names, column names, and so on cannot be escaped, and thus user-supplied structure names are dangerous. This is a common issue in report-writing software.
>The OWASP Enterprise Security API (ESAPI) is a free, open source, web application security control library that makes it easier for programmers to write lower-risk applications.

>Ruby on Rails has a built-in filter for special SQL characters, which will escape ' , " , NULL character and line breaks. Using Model.find(id) or Model.find_by_some thing(something) automatically applies this countermeasure. But in SQL fragments, especially in conditions fragments (where("...")), the connection.execute() or Model.find_by_sql() methods, it has to be applied manually.

- Use LIMIT + other SQL controls w/in queries to prevent mass disclosure of records
- Least Privilege
