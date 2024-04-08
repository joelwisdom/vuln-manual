---
title: Injection Attack
slug: command-injection
abstract: Prevent Java injection attacks with robust input validation.
---

## Description
These attacks exploit vulnerabilities in input processing mechanisms to inject malicious code, jeopardizing the integrity, confidentiality, and availability of the application. Command Injection is a prevalent form of injection attack where malicious actors attempt to execute operating system (OS) commands on the host system of the application. Certain application features may require the use of system commands, and user input may form part of these commands.

### How Injection Attacks Work

Attackers exploit inadequate input validation and sanitization to inject malicious payloads into application inputs. This injected code is then executed by the application's interpreter, leading to unauthorized access, data manipulation, or system compromise



## Vulnerable Code Example
**Code:**
```java
String userInput = request.getParameter("input");
String command = "ls " + userInput;
Runtime.getRuntime().exec(command);
```

* Line 1: `String userInput = request.getParameter("input");`: Retrieves user input from the HTTP request parameter named "input".

* Line 2: `String command = "ls " + userInput;:` Concatenates the user input with the "ls" command, forming a new command string.

* Line 2: `Runtime.getRuntime().exec(command);`: Executes the constructed command using the exec() method from the Runtime class.

* The vulnerability lies in the direct concatenation of the user input with the command (`"ls " + userInput`). If the user input contains special characters or commands, an attacker can manipulate it to execute arbitrary commands on the system.


## Mitigation Techniques
- Input Validation and Sanitization: Thoroughly validate and sanitize all user inputs to ensure they adhere to expected formats and reject any input containing suspicious characters. Consider using an allowlist containing all valid values, especially in scenarios where the valid values for a given operation are limited.
- Least Privilege Principle: Implement the principle of least privilege to restrict the execution of OS commands to only the necessary operations and directories, reducing the impact of successful injection attacks.
- Use of Safe APIs: Whenever possible, utilize safe APIs provided by the language or framework to execute OS commands securely. For instance, in Java, prefer using methods such as `ProcessBuilder` over `Runtime.exec()` to execute commands with enhanced security controls.


## Code with Mitigation Implemented
**Code:**
```java
String userInput = request.getParameter("input");

// Validate the user input to ensure it contains only expected characters or patterns
if (isValidInput(userInput)) {
    // Construct the command using safe APIs like ProcessBuilder
    ProcessBuilder processBuilder = new ProcessBuilder("ls", userInput);
    
    // Start the process and execute the command
    Process process = processBuilder.start();
    
    // Optionally, capture and handle the command output or errors
    InputStream inputStream = process.getInputStream();
    // Handle the input stream
} else {
    // Handle invalid input
    // Respond with an error message or perform appropriate action
}
```

- Input Validation: Implement a robust input validation mechanism (`isValidInput()`) to ensure that the user input contains only expected characters or patterns. This validation helps to prevent injection of malicious commands.

- Safe APIs: Instead of directly concatenating user input with commands and using `Runtime.getRuntime().exec()`, utilize safe APIs like `ProcessBuilder` to construct and execute commands. `ProcessBuilder` separates the command and its arguments, reducing the risk of command injection.

- Error Handling: Handle invalid input gracefully and respond appropriately to the user. This might involve returning an error message or taking other corrective actions.




## References
- [Injection Prevention - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Injection_Prevention_Cheat_Sheet.html)
- OWASP - [Insufficient Input/Output Validation](https://owasp.org/www-project-mobile-top-10/2023-risks/m4-insufficient-input-output-validation)
