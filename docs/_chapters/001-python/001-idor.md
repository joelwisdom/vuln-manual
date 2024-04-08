---
title: Insecure Direct Object References (IDOR)
slug: idor
abstract: Learn what IDOR vulnerabilities are and how to prevent them.
---

## Description
IDOR vulnerabilities arise when an application grants access to resources based on user-controlled input without proper validation. This allows attackers to manipulate this input and potentially access or modify data belonging to other users. For instance, imagine a web application that constructs URLs containing user IDs to display user profiles. An attacker could modify the user ID in the URL to access someone else's profile if the application doesn't validate this input.

## Vulnerable Code Example
**Code:**
```python
def get_user_profile(user_id):
  filename = f"profiles/{user_id}.txt"  user_id
  with open(filename) as f:
    return f.read()
```
* Line 1: `def get_user_profile(user_id):` - This line defines a function named get_user_profile that takes a single argument, user_id. This function is intended to retrieve a user's profile information based on their ID.
* Line 2: `filename = f"profiles/{user_id}.txt"` – This is the vulnerable part. This line creates a variable named filename using f-strings. It constructs the file path dynamically by inserting the user_id directly into the string. This lacks validation, making it susceptible to IDOR attacks.
* Line 3: `with open(filename) as f:` - This line opens the file specified by the filename variable and the f variable becomes a file object that can be used to read the file's contents.
* Line 4: - This line reads the entire contents of the opened file then returns this content as the output.


## Mitigation Techniques
1. **Access Control Mechanisms:**
   - Implement authorization checks: Always perform authorization checks before granting access to any resource. This involves verifying that the user requesting the resource has the necessary permissions to access it.
   - Use access control lists (ACLs): Define access control lists (ACLs) that specify which users or groups can access specific resources. This allows for granular control over resource access.
   - Leverage role-based access control (RBAC): Implement Role-Based Access Control (RBAC) where users are assigned roles, and each role has predefined permissions for accessing resources. This simplifies management and reduces the risk of granting unauthorized access.
2. **Resource Identifiers:**
   - Use non-sequential identifiers: Avoid using predictable identifiers like sequential numbers or user IDs directly in URLs or request parameters. These can be easily manipulated by attackers. 
   - Encode identifiers: If you must use predictable identifiers, consider encoding them before including them in URLs or request parameters. This helps prevent attackers from easily guessing valid values. 
3. **Input Validation:**
   - Validate user input: Validate all user input, especially when it's used to construct resource identifiers. This helps prevent attackers from injecting malicious values that could lead to unauthorized access.

## Code with Mitigation Implemented
**Code:**
```python
def get_user_profile(user_id, current_user):
  if user_id != current_user.id:
    return "Unauthorized access"
  filename = f"profiles/{current_user.id}.txt" 
  with open(filename) as f:
    return f.read()
```
* Line 2 (Changed): `if user_id != current_user.id:` - This line introduces a security check. It compares the provided `user_id` with the id attribute of the `current_user` object. If they don't match, it means the user is trying to access a profile that doesn't belong to them. The function then returns "Unauthorized access" to indicate this.

## References
- [OWASP Testing for Insecure Direct Object References](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References)
- PortSwigger Web Security Academy - [Insecure Direct Object References Cheat Sheet](https://www.youtube.com/watch?v=Sd8jL96H0hc)
- Shim, T. (2024, April 7). [Python security vulnerabilities: common risks and how to mitigate them](https://www.bitcatcha.com/blog/python-security-vulnerabilities/)