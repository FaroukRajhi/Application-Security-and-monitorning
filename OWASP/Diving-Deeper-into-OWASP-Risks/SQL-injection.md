# What is an SQL Injection?
An attack that passes a string input to exploit a database.
Can occur when request user input in a field on a web page.
The attacker modifies a SQL statement using set operations or, they alter WHERE claus to return alternate results. 

Four types of SQL injection:

# SQL manipulation
Modifying an SQL statement of set operations.
WHERE clause
UNION statements
=> grab data from all the tables.

# Other Types of SQL Injection Attacks

  * Code Injection
Inserting new SQL statements or databases commands into another SQL statement.
Execute two statements at one.
Before attack
BEGIN ENCRYPT_PASSWORD('alice', 'mypassword'); END;

After attack
1 BEGIN ENCRYPT_PASSWORD('alice', 'mypassword');
2 DELETE FROM users WHERE upper(username) = upper('admin'); END;

  * Function call injection
Inserting a custom function into a vulnerable AQL statement
  - Sending data from a database to a remote computer
  - Changing passwords
  - Performing sensitive database transactions

  * Buffer overflow
When a program allocates more date in a buffer than possible to store
A buffer contains temporary storage for data transfers.
Causes a system or a program to crash or execute malicious code.
Can exploit an unpatched database
Examples of exploitable functions
  - tz_offset
  - to_timestamp_tz
  - bfilename locator
All this functions had vulnerabilities

* Preventing SQL injection attacks
Use query parameters as placeholders to create dynamic statements.
  The SQL interpreter will check values in query when it execute
Use server-side validation to identify untrusted data inputs
Restrict user privileges: e.g start the access with read only access
Perform dynamic application security testing (DAST): check vulnerabilities when release code in production.
