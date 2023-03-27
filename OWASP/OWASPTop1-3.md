# Broken access control

Allows unauthorized users or hackers to perform unauthorized activities outside of intended permissions.
Hackers will be exploited access control vulnerabilities compromise data, tarnish company image, could result in financial losses.
Hackers try changing information in the URLs to see what happens, e.g : user ID.

* Prevent broken access control
Assign limited privileges to users.
Regular Access Control checks.
Distribute limited public information about your application.
Disable directory listing in URLs, Web server directory where your web pages resides.

# Cryptographic failures

HTTP request contains plain sensitive data.

* Prevention
Attackers easily decrypt traditional encryption methods.
Encrypt sensitive data stored in databases.
Encrypt data at rest and in transit.
Use HTTPS instead http.
Avoid old protocols such as SMTP,FTP To prevent man-in-the-middle attacks.
Encryption keys.

# Injection

Untrusted information is transmitted with a command or query to an interpreter.
Interpreter is fooled in allowing unauthorized data access.
Common injections attacks include AQL, OS, HTTP, LDAP, cross-site scripting, code injection.

* Injection prevention
Use a secure API that avoids using the interpreter or offers parameterized interface.
Use an escape list to block keywords and special characters.
Keep keyword list updated.
Sanitize statements to see if attackers are utilizing SELECT statements.