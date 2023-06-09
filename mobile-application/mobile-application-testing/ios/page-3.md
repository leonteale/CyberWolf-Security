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

### Pre-requisites

**Setup Testing Environment**

* A macOS-based system, as some tools used for iOS app testing are only available on macOS.
* An iOS device or a simulator for running the app, available in Xcode.

**Install and Configure Necessary Tools**

* **Xcode**: This is Apple's Integrated Development Environment (IDE). You can download it from the Mac App Store. It comes with the iOS Simulator and other necessary tools for iOS development and testing.
* **Homebrew**: This is a package manager for macOS. Open Terminal and run the following command to install it:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

* **SwiftLint**: This is a tool for enforcing Swift style and conventions. Install it with Homebrew:

```sh
brew install swiftlint
```

* **MobSF (Mobile Security Framework)**: An automated, all-in-one mobile application (Android/iOS/Windows) pen-testing, malware analysis, and security assessment framework capable of performing static and dynamic analysis. Instructions for its installation can be found on its [GitHub page](https://github.com/MobSF/Mobile-Security-Framework-MobSF).
* **class-dump**: This is a command-line tool for examining the Objective-C runtime information stored in Mach-O files. It generates declarations for the classes, categories, and protocols. You can install it using Homebrew:

```sh
brew install class-dump
```

* **Hopper Disassembler (Optional)**: This is a reverse engineering tool that can be used to disassemble, decompile, and debug your applications. Please note that it's not free, but it offers a 30-day trial.

Now, let's move on to how to perform individual parts of the static analysis.

### Code Quality Checks

### **Checking for Adherence to Coding Standards**

After SwiftLint is installed, navigate to the root directory of your project in Terminal and run the following command:

```sh
swiftlint
```

This will provide you a list of warnings and errors if the code doesn't adhere to the guidelines specified in SwiftLint. You can configure SwiftLint by adding a `.swiftlint.yml` file in your project directory. SwiftLint will automatically read this file.

### **Checking for Hardcoded Sensitive Data**

This can be performed by manual code review or by searching through the codebase using grep-like tools for patterns related to sensitive information like passwords, API keys, tokens, etc. Be aware that this process may produce false positives and negatives.

### Security Configuration Checks

### **Reviewing Configuration Settings**

Review `Info.plist` file and other configuration files for insecure settings, like allowing arbitrary loads, not enforcing App Transport Security (ATS), etc.

### **Checking for Appropriate Permission**

Review the app's requested permissions in `Info.plist` and ensure the app isn't requesting more permissions than it needs.

### Cryptography Checks

Manual code review should be conducted to find usage of weak cryptography protocols or hard-coded cryptographic keys. Developers should use Apple's built-in cryptographic APIs, like `CommonCrypto` and `CryptoKit`, instead of custom solutions.

### Privacy Checks

Review the app's data handling processes for any violation of user privacy. Make sure the app isn't collecting more data than necessary and it's using secure methods to store and transmit personal identifiable information (PII).

### Third-Party Libraries Checks

Check the versions of third-party libraries used in the app for any known vulnerabilities. You can use MobSF for scanning the app's codebase for known vulnerabilities.

Please note that each application is different and additional checks may be necessary depending on the specific features of the app being tested. Always refer back to the detailed OWASP guides when conducting these tests.

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
