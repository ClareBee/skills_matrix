### A07 - XXS (Cross-Site Scripting)
**Note**: Most common issue in OWASP Top Ten
>XSS flaws occur whenever an application includes untrusted data in a new web page without proper validation or escaping, or updates an existing web page with user-supplied data using a browser API that can create HTML or JavaScript. XSS allows attackers to execute scripts in the victim's browser which can hijack user sessions, deface web sites, or redirect the user to malicious sites.

Based on Injection Theory:
>Injection is an attacker's attempt to send data to an application in a way that will change the meaning of commands being sent to an interpreter.

>With Reflected/Stored the attack is injected into the application during server-side processing of requests where untrusted input is dynamically added to HTML. For DOM XSS, the attack is injected into the application during runtime in the client directly.

Source: https://www.owasp.org/index.php/Injection_Theory
See Also: https://code.google.com/archive/p/browsersec/

### Cheatsheets
Reflected/Stored XSS: https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html
DOM-based XSS: https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html
XSS Filter Evasion: https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet
## Vulnerability?

**Reflected XSS:**
- app or API includes unvalidated & unescaped user input as part of HTML output. Can allow attacker to execute arbitrary HTML & JS in victim’s browser. Usu user will need to interact w some malicious link pointing to attacker-controlled page, e.g. malicious watering hole websites, advertisements, etc.
**Stored XSS:**
- App or API stores unsanitised user input viewed at later time by another user/admin. Usu. a critical risk.
**DOM XSS:**
- JS frameworks, SPAs, & APIs that dynamically include attacker-controllable data to page are vulnerable to DOM XSS.

e.g. session stealing, account takeover, MFA bypass, DOM node replacement or defacement (such as trojan login panels), attacks against user's browser such as malicious software downloads, key logging, etc.

## Prevention
- Frameworks that automatically escape XSS by design, e.g. latest Ruby on Rails, React JS, & know limitations
- Escaping untrusted HTTP request data based on context in HTML output (body, attribute, JavaScript, CSS, or URL) for Reflected and Stored XSS vulnerabilities.
- Applying context-sensitive encoding when modifying browser document on client side for DOM XSS.
- Enabling a <u>Content Security Policy (CSP) as a defense-in-depth mitigating control. (Effective if no other vulnerabilities exist that allow placing malicious code via local file includes (e.g. path traversal overwrites or vulnerable libraries from permitted content delivery networks).
- Use HTTPOnly cookie flag
- Use the X-XSS-Protection Response Header

Source: https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)
___
Source for the below:
https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html
XSS Prevention Rules:
1. **Never Insert Untrusted Data Except in Allowed Locations**
(NOT in script tag, html comment, attribute name, tag name, css) & never run JS from untrusted source

2. **HTML Escape Before Inserting Untrusted Data into HTML Element Content**
Escape before inserting into body/div etc:
Escape w HTML entity encoding to prevent switching into any execution context, such as script, style, or event handlers. Using hex entities is recommended in the spec. In addition to the 5 characters significant in XML (&, <, >, ", '), the forward slash is included as it helps to end an HTML entity.
```html
 & --> &amp;
 < --> &lt;
 > --> &gt;
 " --> &quot;
 ' --> &#x27;     
 / --> &#x2F;     
```
 &apos; not recommended because not in HTML spec
 Forward slash included as helps end an HTML entity

3. **Attribute Escape Before Inserting Untrusted Data into HTML Common Attributes**
e.g. re: untrusted data into typical attribute values like width, name, value
Except for alphanumeric characters, escape all characters w ASCII values less than 256 with &#xHH; format (or named entity if available) to prevent switching out of attribute.

4. **JavaScript Escape Before Inserting Untrusted Data into JavaScript Data Values**
Except for alphanumeric characters, escape all characters less than 256 w the \xHH format to prevent switching out of data value into script context or into another attribute. DO NOT use any escaping shortcuts like \" because quote character may be matched by HTML attribute parser which runs first. These escaping shortcuts are also susceptible to escape-the-escape attacks where attacker sends \" and vulnerable code turns that into \\" which enables the quote.

</script> closing tag will close a script block even tho it's inside a quoted string because HTML parser runs before JavaScript parser. Please note this is an aggressive escaping policy that over-encodes.

4.1 **HTML escape JSON values in an HTML context and read the data with JSON.parse**
Ensure returned Content-Type header is application/json & not text/html - instructs browser not to misunderstand context & execute injected script

5. **CSS Escape And Strictly Validate Before Inserting Untrusted Data into HTML Style Property Values**
Stay away from putting untrusted data into complex properties like url, behavior, & custom (-moz-binding).

Don't put untrusted data into IE’s expression property value which allows JS.

Except for alphanumeric characters, escape all characters w ASCII values less than 256 w the \HH escaping format. DO NOT use any escaping shortcuts (see 4. also re: script tag)

 Aggressive CSS encoding and validation recommended to prevent XSS attacks for both quoted & unquoted attributes.

6. **URL Escape Before Inserting Untrusted Data into HTML URL Parameter Values**
For HTTP GET parameter value:
Except for alphanumeric characters, escape all w ASCII values less than 256 w %HH escaping format. Incl untrusted data: URLs should not be allowed as there is no good way to disable attacks w escaping to prevent switching out of the URL.
WARNING: Do not encode complete or relative URLs w URL encoding! If untrusted input is meant to be placed into href, src or other URL-based attributes, validate to ensure it doesn't point to unexpected protocol, especially js links. URLs should then be encoded based on context of display. E.g, user driven URLs in href links should be attribute encoded.

7. **Sanitize HTML Markup with a Library Designed for the Job**
If app handles markup - untrusted input that is supposed to contain HTML - w difficult to validate. Encoding also difficult, as would break tags, so library to parse/clean HTML formatted text.
e.g. HtmlSanitizer, OWASP Java HTML Sanitizer, Ruby on Rails SanitizeHelper

7. **Avoid JavaScript URLs**
Untrusted URL's that include the protocol js: will execute js code when used in URL DOM locations such as anchor tag HREF attributes or iFrame src locations. Validate all untrusted URLs to ensure they only contain safe schemes e.g. HTTPS.

8. **Prevent DOM-based XSS**
See below. Source https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html

### DOM-based XSS
When browser is rendering HTML & any other associated content like CSS, JavaScript, etc. it identifies various rendering contexts for different kinds of input & follows different rules for each.

Each parser has distinct & separate semantics. The complication is compounded by differing meanings & treatment of encoded values within each subcontext (HTML, HTML attribute, URL, and CSS) within the execution context.

1. **HTML Escape then JavaScript Escape Before Inserting Untrusted Data into HTML Subcontext within the Execution Context**
Example Dangerous HTML Methods
Attributes
 `element.innerHTML = "<HTML> Tags and markup";`
 `element.outerHTML = "<HTML> Tags and markup";`
Methods
 `document.write("<HTML> Tags and markup");`
 `document.writeln("<HTML> Tags and markup");`
Guideline
 `element.innerHTML = "<%=Encoder.encodeForJS(Encoder.encodeForHTML(untrustedData))%>";
 element.outerHTML = "<%=Encoder.encodeForJS(Encoder.encodeForHTML(untrustedData))%>";
 document.write("<%=Encoder.encodeForJS(Encoder.encodeForHTML(untrustedData))%>");
 document.writeln("<%=Encoder.encodeForJS(Encoder.encodeForHTML(untrustedData))%>");`
Note: The Encoder.encodeForHTML() and Encoder.encodeForJS() are just notional encoders.

2. **JavaScript Escape Before Inserting Untrusted Data into HTML Attribute Subcontext within the Execution Context**

When you are in a DOM execution context you only need to JavaScript encode HTML attributes which do not execute code (attributes other than event handler, CSS, & URL attributes).

SAFE but BROKEN example
```javascript
 var x = document.createElement("input");
 x.setAttribute("name", "company_name");
 // In the following line of code, companyName represents untrusted user input
 // The Encoder.encodeForHTMLAttr() is unnecessary and causes double-encoding
 x.setAttribute("value", '<%=Encoder.encodeForJS(Encoder.encodeForHTMLAttr(companyName))%>');
 var form1 = document.forms[0];
 form1.appendChild(x);
 ```
SAFE and FUNCTIONALLY CORRECT example
```javascript
 var x = document.createElement("input");
 x.setAttribute("name", "company_name");
 x.setAttribute("value", '<%=Encoder.encodeForJS(companyName)%>');
 var form1 = document.forms[0];
 form1.appendChild(x);
 ```
3. **Be Careful when Inserting Untrusted Data into the Event Handler and JavaScript code Subcontexts within an Execution Context**
Putting dynamic data within JS code is esp dangerous because JS encoding has different semantics for JS encoded data vs others. e.g. a JS encoded string will execute even tho it is JS encoded.

Therefore avoid including untrusted data in this context.
```javascript
var x = document.createElement("a");
x.href="#";
// In the line of code below, the encoded data on the right (the second argument to setAttribute)
// is an example of untrusted data that was properly JavaScript encoded but still executes.
x.setAttribute("onclick", "\u0061\u006c\u0065\u0072\u0074\u0028\u0032\u0032\u0029");
var y = document.createTextNode("Click To Test");
x.appendChild(y);
document.body.appendChild(x);
```
The setAttribute(name_string,value_string) method is dangerous because it implicitly coerces the value_string into the DOM attribute datatype of name_string.

In the case above, the attribute name is an JavaScript event handler, so the attribute value is implicitly converted to JavaScript code and evaluated. In the case above, JavaScript encoding does not mitigate against DOM based XSS.

see also:setTimeout, setInterval, new Function, etc.

```html
<!-- Does NOT work  -->
<a id="bb" href="#" onclick="\u0061\u006c\u0065\u0072\u0074\u0028\u0031\u0029"> Test Me</a>
```
Directly setting event handler attributes will allow JavaScript encoding to mitigate against DOM based XSS. Please note, it is always dangerous design to put untrusted data directly into a command execution context.
```javascript
<a id="bb" href="#"> Test Me</a>
//The following does NOT work because the event handler is being set to a string.  
//"alert(7)" is JavaScript encoded.
document.getElementById("bb").onclick = "\u0061\u006c\u0065\u0072\u0074\u0028\u0037\u0029";

//The following does NOT work because the event handler is being set to a string.
document.getElementById("bb").onmouseover = "testIt";

//The following does NOT work because of the encoded "(" and ")".
//"alert(77)" is JavaScript encoded.
document.getElementById("bb").onmouseover = \u0061\u006c\u0065\u0072\u0074\u0028\u0037\u0037\u0029;

//The following does NOT work because of the encoded ";".
//"testIt;testIt" is JavaScript encoded.
document.getElementById("bb").onmouseover = \u0074\u0065\u0073\u0074\u0049\u0074\u003b\u0074\u0065\u0073
                                            \u0074\u0049\u0074;

//The following DOES WORK because the encoded value is a valid variable name or function reference.  
//"testIt" is JavaScript encoded
document.getElementById("bb").onmouseover = \u0074\u0065\u0073\u0074\u0049\u0074;

function testIt() {
   alert("I was called.");
}
```
4. **JavaScript Escape Before Inserting Untrusted Data into the CSS Attribute Subcontext within the Execution Context**
Normally executing JavaScript from a CSS context required either passing javascript:attackCode() to the CSS url() method or invoking the CSS expression() method passing JavaScript code to be directly executed.

In order to mitigate against the CSS url() method, ensure that you are URL encoding the data passed to the CSS url() method.
```javascript
document.body.style.backgroundImage = "url(<%=Encoder.encodeForJS(Encoder.encodeForURL(companyName))%>)";
```
5. **URL Escape then JavaScript Escape Before Inserting Untrusted Data into URL Attribute Subcontext within the Execution Context**
```javascript
var x = document.createElement("a");
x.setAttribute("href", '<%=Encoder.encodeForJS(Encoder.encodeForURL(userRelativePath))%>');
var y = document.createTextElement("Click Me To Test");
x.appendChild(y);
document.body.appendChild(x);
```
If you utilise fully qualified URLs then this will break the links as the colon in the protocol identifier (http: or javascript:) will be URL encoded preventing the http and javascript protocols from being invoked.

6. **Populate the DOM using safe JavaScript functions or properties**
The most fundamental safe way to populate the DOM with untrusted data is to use the safe assignment property textContent.
```javascript
<script>
element.textContent = untrustedData;  //does not execute code
</script>
```
7. **Fixing DOM Cross-site Scripting Vulnerabilities**
If you want to use user input to write in a div tag element don’t use innerHtml, instead use innerText or textContent.

### **Guidelines for Developing Secure Applications Utilising JavaScript**
-difficult re: large attack surface & lack of standardisation across browsers.

1. - Untrusted data should only be treated as displayable text

2. - Always JavaScript encode and delimit untrusted data as quoted strings when entering the application when building templated Javascript

3. - document.createElement("..."), element.setAttribute("...","value"), element.appendChild(...) and similar are safe ways to build dynamic interfaces.

Please note, element.setAttribute is only safe for a limited number of attributes. Dangerous attributes include any attribute that is a command execution context, such as onclick or onblur.

Examples of safe attributes includes: align, alink, alt, bgcolor, border, cellpadding, cellspacing, class, color, cols, colspan, coords, dir, face, height, hspace, ismap, lang, marginheight, marginwidth, multiple, nohref, noresize, noshade, nowrap, ref, rel, rev, rows, rowspan, scrolling, shape, span, summary, tabindex, title, usemap, valign, value, vlink, vspace, width.

4. - Avoid sending untrusted data into HTML rendering methods
element.innerHTML = "...";/element.outerHTML = "...";
document.write(...);/document.writeln(...);
5. - Avoid the numerous methods which implicitly eval() data passed to it
Make sure that any untrusted data passed to these methods is:
Delimited with string delimiters/ Enclosed within a closure/ JavaScript encoded to N-levels based on usage/ Wrapped in a custom function.

An important implementation note is that if the JavaScript code tries to utilise the double or triple encoded data in string comparisons, the value may be interpreted as different values based on the number of evals() the data has passed through.

6. - Limit the usage of untrusted data to only right side operations
Instead of:
window[userData] = "moreUserData";
Do the following instead:
```javascript
if (userData === "location") {
   window.location = "static/path/or/properly/url/encoded/value";
}
```
7. - When URL encoding in DOM be aware of character set issues

8. - Limit access to properties objects when using object[x] accessors

9. - Run your JavaScript in a ECMAScript 5 canopy or sandbox

10. - Don’t eval() JSON to convert it to native JavaScript objects.

### Common Problems Associated with Mitigating DOM Based XSS
- Complex Contexts
- Inconsistencies of Encoding Libraries
(OWASP ESAPI/ OWASP Java Encoder/ Apache Commons Text StringEscapeUtils, replace one from Apache Commons Lang3/ Jtidy)
- Encoding Misconceptions
Many security training curriculums and papers advocate the blind usage of HTML encoding to resolve XSS, but remember that encodings are lost when you retrieve them using the value attribute of a DOM element.
- Usually Safe Methods
One example of an attribute which is thought to be safe is innerText, but depending on the tag which innerText is applied, code can be executed.
```javascript
<script>
 var tag = document.createElement("script");
 tag.innerText = "<%=untrustedData%>";  //executes code
</script>
```
---
### Rails-specific:
**Cross-site Scripting (XSS)**
By default, in Rails 3.0 and up protection against XSS comes as the default behavior. When string data is shown in views, it is escaped prior to being sent back to the browser. This goes a long way, but there are common cases where developers bypass this protection - for example to enable rich text editing. In the event that you want to pass variables to the front end with tags intact, it is tempting to do the following in your .erb file (ruby markup).

# Wrong! Older rails versions, do not do this!
`<%= raw @product.name %>`

# Wrong! Newer rails versions, do not do this!
`<%= @product.name.html_safe %>`

# Wrong! Newer rails versions, do not do this!
`<%= content_tag @product.name %>`
Unfortunately, any field that uses raw, html_safe, content_tag or similar like this will be a potential XSS target. Note that there are also widespread misunderstandings about html_safe().

If you must accept HTML content from users, consider a markup language for rich text in an application (Examples include: markdown and textile) and disallow HTML tags. This helps ensures that the input accepted doesn’t include HTML content that could be malicious.

If you cannot restrict your users from entering HTML, consider implementing content security policy to disallow the execution of any javascript. And finally, consider using the #sanitize method that lets you whitelist allowed tags. Be careful, this method has been shown to be flawed numerous times and will never be a complete solution.

An often overlooked XSS attack vector for older versions of rails is the href value of a link:

`<%= link_to "Personal Website", @user.website %>`
If @user.website contains a link that starts with javascript:, the content will execute when a user clicks the generated link:

`<a href="javascript:alert(‘Haxored’)">Personal Website</a>`
Newer Rails versions escape such links in a better way.

`link_to "Personal Website", 'javascript:alert(1);'.html_safe()`
# Will generate:
# "<a href="javascript:alert(1);">Personal Website</a>"
Using Content Security Policy is one more security measure to forbid execution for links starting with javascript: .

Brakeman scanner helps in finding XSS problems in Rails apps.

Source: https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Ruby_on_Rails_Cheatsheet.md
