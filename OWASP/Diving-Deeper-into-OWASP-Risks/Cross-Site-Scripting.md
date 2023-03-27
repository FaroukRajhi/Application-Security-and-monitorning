# What is cross-site scripting

Taking untrusted data and sending it to a web browser without proper validation or escaping.
Attacker use the cross-site scripting To execute scripts in the victim's browser.
Representing as XSS.

* Different ways:
  - Hijack user sessions: Attacks an app using untrusted data in the HTML snippet without validation or escaping, e.g credit card information are plain text
  - Deface websites by replacing or removing images or contents
  - Redirect user from trusted website to malicious sites

# Types of cross-site scripting

* Stored : In database or server
* Blind : Inject the script that has a payload to be executed on the backend of an application by the user or administrator.
* Reflected: Inject the script to be reflected from the attacker server to users on the system (malicious links)

# Preventing XSS attacks

* look for suspicious HTTP requests or keywords.
* Escape suspect lists or keywords, or block special characters.
* Turn off HTTP TRACE support on a web server : Eliminate collect user cookies and send them to a malicious server.
* Avoid using unsafe sinks by refactoring or by using text content or values.

