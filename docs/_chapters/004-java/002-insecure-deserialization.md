---
title: Insecure Deserialization
slug: insecure
abstract: Harden your Java app's defenses against insecure deserialization by validating all received data.
---

## Description


## Vulnerable Code Example
**Code:**
```java
String serializedData = request.getParameter("data");
ByteArrayInputStream bis = new ByteArrayInputStream(Base64.getDecoder().decode(serializedData));
ObjectInputStream ois = new ObjectInputStream(bis);
Object obj = ois.readObject();

```

- Line 1: `String serializedData = request.getParameter("data");`: Retrieves serialized data from the HTTP request parameter named "data".
- Line 2: `ByteArrayInputStream bis = new ByteArrayInputStream(Base64.getDecoder().decode(serializedData));`: Decodes the serialized data from Base64 encoding and creates a `ByteArrayInputStream`.
- Line 3: `ObjectInputStream ois = new ObjectInputStream(bis);`: Constructs an ObjectInputStream to deserialize the data.
- Line 4: `Object obj = ois.readObject();`: Reads the deserialized object from the `ObjectInputStream`.



## Mitigation Techniques
1. Implement Integrity Checks: Apply digital signatures or checksums to serialized objects to verify their integrity during deserialization.
2. Use Safe Deserialization Practices: Avoid deserializing untrusted data from unreliable sources and employ safe deserialization libraries or mechanisms.
3. Implement Whitelisting: Maintain a whitelist of permitted classes for deserialization and reject any serialized objects not belonging to the whitelist.

***Note***:
Ensure deserialization is performed in a restricted environment with limited privileges. Validate deserialized data before processing it further to mitigate the risk of exploitation.


## Code with Mitigation Implemented
**Code:**
```java
String serializedData = request.getParameter("data");

// Decode the serialized data from Base64 encoding
byte[] serializedBytes = Base64.getDecoder().decode(serializedData);

try {
    // Use a safe deserialization mechanism, such as JSON deserialization, instead of ObjectInputStream
    Object obj = deserializeFromJSON(serializedBytes);
    
    // Use the deserialized object safely
} catch (IOException | ClassNotFoundException | ClassCastException ex) {
    // Handle deserialization errors
    // Respond with an error message or perform appropriate action
}

// Safe JSON deserialization method
private Object deserializeFromJSON(byte[] serializedBytes) throws IOException, ClassNotFoundException {
    // Deserialize the data from JSON format
    ObjectMapper objectMapper = new ObjectMapper();
    return objectMapper.readValue(serializedBytes, Object.class);
}
```

* Decode Serialized Data: Decode the serialized data from Base64 encoding to obtain the byte array representation.

* Use Safe Deserialization Mechanism: Instead of using `ObjectInputStream` for deserialization, employ a safer deserialization mechanism such as JSON deserialization. JSON deserialization libraries like Jackson or Gson provide better security controls and mitigate the risk of executing arbitrary code.

* Error Handling: Handle deserialization errors gracefully by catching exceptions (`IOException`, `ClassNotFoundException`, `ClassCastException`) and responding with appropriate error messages or actions.



## References
- Waratek - [What is a Java Deserialization Vulnerability?](https://waratek.com/blog/java-deserialization-vulnerability/)
- Red Fox Security - [Insecure Deserialization in Java](https://redfoxsec.com/blog/insecure-deserialization-in-java/)