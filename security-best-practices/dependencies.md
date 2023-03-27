# What is Dependencies?

Piece of code relied on by other code.
Adds features and functionality.
No written from scratch : Reusable code.
Fount in library, package or module.
You can use package manager to download ans install.

# Benefits and importance

Speeds up development process
Deliver software quickly
More features, functionality
No written from scratch
Dependencies could perform better than native implementation

It's a piece of software
Program A calls to program B for a feature/functionality
Program B is a dependency for program A.

# Dependency challenges and risks

Using code from internet is risky
Exposure to vulnerabilities
Production risk
  - Performance issues, crashes
  - Data leaks
Licensing challenges
  - compliance ans compatibility

# Inspection and management

Vet dependencies before implementation
  - Design
  - Quality
  - Testing
  - Debugging
  - Maintenance
  - Usage
  - Security
  - Use dependency management tools
# Dependency's dependencies

A dependency can have a dependency
Indirect dependencies may impact your code
Inspect indirect dependencies
Use dependency manager for tracking
Indirect dependency infiltration

# Example: Flask dependencies

Python-based web framework.
Provides tools, libraries for building web applications
Used by LinkedIn, Pinterest
Has its own dependencies:
  - Werkzeug: web server gateway interface
  - Jinja: Template language for rendering web pages
  - MarkupSafe: a security dependency for untrusted inputs
  - Its Dangerous: secure data integrity dependency
  - Click : Framework for writing command line applications