# Identification and authentication failures

Application allows:
Credential stuffing, brute force, other automated attacks.
Occurs when attackers has a list of legitimate usernames and passwords.
Sessions identifiers visible in URLs.
No session timeout.

* Prevention

Software supply chain security tools (Scan app components).
Check and review configuration changes.
No sensitive data to untrusted resources.
MFA.
Server-side session manager (To generate new random session ID).

# Software and data integrity failures

Caused by insecure code and weak infrastructure.
App components from untrusted sources.
No integrity checks for automatic updates.
Lead to breaches and other types of attacks.

* Prevention

Segregate CI/CD pipeline.
Accurate configuration.
Access complete.
Software supply chain security tool.
No unsigned, unencrypted or serialized data to un trusted clients.
Digital signature, integrity checks that data or code comes from legitimate source.

# Security logging and monitoring failures.

Critical security tools.
Inadequacies mask serious issues.
Weak log messages impair troubleshooting.
Auditable events not logged.
Log data overwritten to quickly.
Lack of monitoring system
Infiltration goes undetected.

* Prevention

Straightforward log entries.
centralize and back them up.
Include auditable events.
Server-side input validation.
Context for identifying suspicious accounts.
Retention for delayed analysis.
Implement monitoring system.
Audit logs periodically.

# Server-side request forgery (SSRF)

Attacker Attacks web server and enter to to the internal server.
Types of SSRF:
Blind (basic): URL sent No data returned.
Semi-blind: URL sent some data returned.
Non-blind: URL sent URI data returned.

* Prevention

Sanitize and validate input data by client.
Create and use whitelists.
Disallow HTTP redirects