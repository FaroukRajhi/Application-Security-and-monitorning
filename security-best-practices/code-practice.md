# General code practices

Secure software development lifecycle.
Secure coding standards.
Reusable object library.
Tested and approved code.
Focus on exposed threats.
Attend training.

# Input Validation

Check server-side input.
Check for:
  - Expected data types.
  - Data range, data length.
  - Allowed characters
Reject any non-conforming data.
Validate data from untrusted sources.
Hardened developer systems reduce risk.

# Input scrubbing

Whitelist validation.
Scrub malicious input characters <>"'%()&+\\'\"
Implement additional controls
  - Output encoding
  - Securing task-specific APIs
  - Accounting for all data

# Output encoding

Translating the input code to safe output code
Implement policy, practice for each type
Encode all characters
Sanitize untrusted query output
sanitize untrusted output to local operating system commands

# Error handling and logging

Improper handling exposes risks
Meaningful error messages and logging
Use Custom error pages, generic messages
Release allocated memory
Restricted access logs
Log any tampering events and failures:
  - Input
  - Authentication attempts
  - Access control
