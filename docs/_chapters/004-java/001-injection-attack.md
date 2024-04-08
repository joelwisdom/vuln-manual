---
title: Injection Attack
slug: injection
abstract: A guide to quickly setting up your site with this theme.
---

## Definition
Injection attacks are a prevalent security threat in software development, posing significant risks to applications that handle user input (Injection Prevention - OWASP Cheat Sheet Series, n.d.). Attackers exploit vulnerabilities in input processing mechanisms to inject malicious code, compromising the integrity, confidentiality, and availability of the application.

### Types of Injection Attacks

Common injection attacks include SQL injection and Command Injection. In SQL injection, attackers manipulate SQL queries to access unauthorized data or execute arbitrary commands. Command injection is a technique where a malicious actor tries to execute OS commands on the system hosting the application. Some features might need you to use system commands. In some cases, user input is part of these commands. (Command Injection in Java: Examples and Prevention, 2021.).


## Explanation
Attackers exploit inadequate input validation and sanitization to inject malicious payloads into application inputs. This injected code is then executed by the application's interpreter, leading to unauthorized access, data manipulation, or system compromise

## Mitigation Techniques
- Prepared Statements and Parameterized Queries: Use PreparedStatement or Parameterized Queries to construct SQL queries, preventing injection attacks by separating query structure from user input (Injection Prevention - OWASP Cheat Sheet Series, 2021)

- Input Validation and Sanitization: Validate and sanitize all user inputs to ensure they conform to expected formats and reject any input containing suspicious characters. Additionally, Sometimes, the valid values for a given operation are very few. In such scenarios, you might use an allowlist containing all valid values(Java SQL Injection Guide: Examples and Prevention, 2021)

- Least Privilege Principle: Restrict database access to only necessary tables and operations, reducing the impact of successful injection attacks


## Example
// Using Prepared Statement to prevent SQL Injection
String username = request.getParameter("username");
String password = request.getParameter("password");
String sql = "SELECT * FROM users WHERE username = ? AND password = ?";
PreparedStatement statement = connection.prepareStatement(sql);
statement.setString(1, username);
statement.setString(2, password);
ResultSet resultSet = statement.executeQuery();
```
