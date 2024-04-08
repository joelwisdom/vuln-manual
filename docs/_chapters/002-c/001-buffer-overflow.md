---
title: Buffer Overflow
slug: buffer-overflow
abstract: Learn about the famous buffer overflow  vulnerability and how to prevent them.
---

## Description
A buffer overflow is a critical security vulnerability that occurs when a program attempts to write more data into a fixed-size memory block (buffer) than it can hold. This overflow can have dire consequences, leading to:
* Program Crashes: The overflowing data might overwrite essential instructions or program data, causing the program to crash abruptly.
* Data Corruption: Critical program information or user data stored in adjacent memory locations can be corrupted, rendering it unusable.
* Security Exploits: In a more severe scenario, attackers can exploit buffer overflows to gain unauthorized access to a system, steal sensitive data, or execute malicious code.
### Understanding Buffers
In C, arrays act as buffers. They are contiguous blocks of memory that can hold a fixed number of elements of the same data type. Each element occupies a specific amount of memory space within the buffer.
C provides certain string manipulation functions like strcpy, sprintf, and gets that are notorious for causing buffer overflows if not used with caution. These functions lack built-in checks to ensure that the destination buffer has enough space to accommodate the source data. They simply copy data from the source to the destination without verifying the boundaries.


## Vulnerable Code Example
**Code:**
```C
#include <stdio.h>

int main() {
    char buffer[5];  // Buffer with space for 5 characters (including null terminator)

    printf("Enter a string: ");
    gets(buffer);  // Unsafe function: can lead to buffer overflow

    printf("You entered: %s\n", buffer);

    return 0;
}
```
- Buffer Size: The buffer array is declared with a size of 5 characters, meaning it can hold a maximum of 4 characters plus the null terminator (\0).
- Unsafe Function: The gets function is used to read user input from the console. However, it does not perform bounds checking, meaning it won't stop reading input even if it exceeds the buffer's capacity.
- Potential Overflow: If the user enters a string longer than 4 characters, the gets function will continue writing characters beyond the allocated space for buffer. This excess data will overflow into adjacent memory locations, potentially overwriting critical data or instructions.



## Mitigation Techniques
To prevent buffer overflows, here are some essential mitigation strategies:
1. **Use safer string manipulation functions**: opt for functions like strncpy or strlcpy that take the maximum number of characters to copy as an argument, preventing overflows.
2. **Perform bounds checking**: Explicitly check the size of the data being written to a buffer before writing it. Ensure it doesn't exceed the buffer's allocated size.
3. **Use memory allocation functions**: Instead of fixed-size buffers, consider using memory allocation functions like malloc and realloc to allocate memory dynamically based on the actual data size.
4. **Input validation**: When dealing with user input, validate its size before copying it into a buffer. This helps prevent attackers from injecting malicious code that might cause an overflow.
5. **Utilize bounds-checking tools**: Employ tools like AddressSanitizer or static code analyzers during development to identify potential buffer overflows.


## Code with Mitigation Implemented
**Code:**
```C
#include <stdio.h>
 int main() {
	char buffer[5];  // Buffer with space for 5 characters (including null terminator)
 
	printf("Enter a string: ");
	fgets(buffer, sizeof(buffer), stdin);  // Safe function: prevents buffer overflow
 
	printf("You entered: %s\n", buffer);
 
	return 0;
}
```
- The original code with the `gets()` function replaced by `fgets()` for mitigating the buffer overflow vulnerability:


## References
- Snyk Learn - [Use After Free](https://learn.snyk.io/lesson/use-after-free/)
- [Buffer overflow attack explained with a C program example](https://www.thegeekstuff.com/2013/06/buffer-overflow/)
- [Buffer overflow attack with example. ](https://www.geeksforgeeks.org/buffer-overflow-attack-with-example/)