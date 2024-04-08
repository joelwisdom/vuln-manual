---
title: SQL Injection
slug: sqli
abstract: Secure your PHP app - Prevent SQL injection with proper input validation.
---

## Description
SQL injection vulnerabilities occur when user input is improperly sanitized and directly incorporated into SQL queries, allowing attackers to manipulate the queries and potentially execute arbitrary SQL commands. This can lead to unauthorized access to sensitive data, data manipulation, or even server compromise.

## Vulnerable Code Example
**Code:**
```php
$username = $_POST['username'];
$password = $_POST['password'];
$sql = "SELECT * FROM users WHERE username='$username' AND password='$password'";
$result = mysqli_query($conn, $sql);
```
- The vulnerable code directly inserts user-supplied input (`$_POST['username']` and `$_POST['password']`) into an SQL query without proper sanitization or parameterization.
- Attackers can exploit this vulnerability by injecting malicious SQL commands into the input fields, potentially bypassing authentication or executing unauthorized database operations.



## Mitigation Techniques
1. **Prepared Statements:**
   - Use prepared statements with parameterized queries to separate SQL code from user input. This prevents attackers from injecting malicious SQL commands.
2. **Input Sanitization**
   - Sanitize user input by using proper input validation and sanitization functions to remove or escape potentially harmful characters.
3. **least Privilege Principle**
   - Use database accounts with the least privileges necessary to perform their tasks. Avoid granting unnecessary permissions to prevent unauthorized access.
3. **ORMs and Query Builders**
   - Consider using Object-Relational Mapping (ORM) libraries or query builders, which often handle input sanitization and parameterization automatically.


## Code with Mitigation Implemented
**Code:**
```php
$username = mysqli_real_escape_string($conn, $_POST['username']);
$password = mysqli_real_escape_string($conn, $_POST['password']);
$sql = "SELECT * FROM users WHERE username=? AND password=?";
$stmt = mysqli_prepare($conn, $sql);
mysqli_stmt_bind_param($stmt, "ss", $username, $password);
mysqli_stmt_execute($stmt);
$result = mysqli_stmt_get_result($stmt);
```

- The code with mitigation utilizes prepared statements to parameterize the SQL query, preventing SQL injection attacks by separating user input from the query execution.
- User input is properly sanitized using `mysqli_real_escape_string()` function to prevent SQL injection vulnerabilities.


## References
- OWASP - [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- PHP Manual - [SQL Injection](https://www.php.net/manual/en/security.database.sql-injection.php)
- W3Schools -  [PHP MySQL Prepared Statements](https://www.w3schools.com/php/php_mysql_prepared_statements.asp)
