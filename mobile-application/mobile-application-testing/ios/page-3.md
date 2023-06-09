# Checklist and Methodology

## Checklist

### Static Analysis

Static analysis involves inspecting the app's code and configuration without running it. It checks for coding and design issues that may lead to security vulnerabilities.

#### 1. Code Quality Checks

* Check for coding standards adherence.
* Look for common coding errors and insecure coding practices.
* Identify hardcoded sensitive data such as passwords, API keys, and tokens.

#### 2. Security Configuration Checks

* Review the app's configuration settings for any insecure default or inappropriate settings.
* Check for appropriate permissions requested by the app.
* Inspect the app for any insecure file storage.

#### 3. Cryptography Checks

* Review the usage of cryptographic protocols and algorithms.
* Look for hard-coded cryptographic keys.

#### 4. Privacy Checks

* Inspect the code for any violation of user privacy.
* Look for proper handling of personally identifiable information (PII).

#### 5. Third-Party Libraries Checks

* Inspect the use of third-party libraries and frameworks.
* Check the versions of the libraries for known vulnerabilities.

### Dynamic Analysis

Dynamic analysis involves testing the app while it is running. It helps to identify security vulnerabilities at runtime, interactions with the environment, and user input handling issues.

#### 1. Input Validation Checks

* Check for proper validation of user-provided inputs.
* Test for common injection attacks such as SQL injection, command injection, etc.

#### 2. Authentication and Session Management Checks

* Verify proper implementation of authentication mechanisms.
* Check for proper session management.
* Test for potential bypass of authentication mechanism.

#### 3. Network Communication Checks

* Verify the security of network communications.
* Check for proper use of SSL/TLS and certificate pinning.

#### 4. Data Storage and Privacy Checks

* Inspect how the app handles data storage.
* Test for insecure data storage vulnerabilities.
* Verify proper implementation of data encryption.

#### 5. Inter-process Communication (IPC) Checks

* Test for vulnerabilities related to IPC mechanisms.

#### 6. Environment Interaction Checks

* Check for vulnerabilities related to the interaction with the mobile environment.
* Test for code execution in the background.

## Methodology



### Binary uses insecure APIs (iOS)

**How to Test**

**Extracting the binary from the IPA**: First, we need to extract the binary file from the IPA. An IPA file is just a ZIP archive. Unzip it and you will find the binary in the Payload folder.

```bash
unzip YourApp.ipa
cd Payload/YourApp.app
```

**Searching for insecure APIs**: Use the `strings` command to extract strings from the binary and `grep` to filter the results.

```bash
strings YourApp | grep -E "_memcpy|_fopen|_sscanf|_printf"
```

This command will search all the strings in the binary for the insecure APIs and print the matches.

You can use the following command to combine the two, in order to check for false possitives.

```bash
grep -rl "_memcpy\|_fopen\|_sscanf\|_printf" . | while read -r filename; do for func in _memcpy _fopen _sscanf _printf; do match=$(strings "$filename" | grep "$func"); if [ ! -z "$match" ]; then echo "$filename: $match"; fi; done; done
```

Alternatively, you could use&#x20;

You can also use reverse engineering tools like Ghidra, IDA Pro, or radare2 to perform a more in-depth analysis and understand the context of these API usages.

**Using Tools**:

The Clang Static Analyzer is a source code analysis tool that finds bugs in C, C++, and Objective-C programs. It can be used to detect the usage of insecure APIs.

**Remediation Advice**

If the insecure APIs are found, developers should replace these functions with their secure counterparts where possible.

For `_memcpy`, consider using `memcpy_s` (if available) or ensure the copied data is limited to the size of the destination buffer.

For `_fopen`, consider using `fopen_s` or other methods with better error handling and more secure behaviors.

For `_sscanf`, use `sscanf_s` or ensure input validation.

For `_printf`, use `snprintf`, `vsnprintf`, or similar to avoid buffer overflows.

However, it's important to note that the "\_s" versions of these functions are part of the optional Annex K of the ISO C11 standard, and may not be available on all platforms or compilers. Therefore, when using these functions in iOS, especially when using Objective-C or Swift, the traditional versions of these functions might be the only ones available. In such cases, additional care should be taken to use these functions in a secure manner, such as by ensuring proper size checks and error handling.

The specific remediation steps will depend on the nature of the native code, the iOS app's functionality, and the specific usage of these insecure functions within the context of the app. It's crucial to review the code carefully to determine the best mitigation strategy for each instance of insecure API usage.
