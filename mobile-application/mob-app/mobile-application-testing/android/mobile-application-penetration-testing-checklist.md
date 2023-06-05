---
description: >-
  This guide provides a detailed checklist for Android mobile application
  penetration testing.
---

# Mobile Application Penetration Testing Checklist

### Setup

1. **Setup Testing Environment**
   * Prepare a suitable testing environment, which can be an actual Android device or an Android emulator like Genymotion.
2. **Install and Configure Necessary Tools**
   * ADB (Android Debug Bridge)
   * APKTool
   * JADX
   * Burp Suite
   * Drozer
   * dex2jar
   * JD-GUI
   * tcpdump
   * Wireshark
   * Frida
   * Objection

### Static Analysis

3.  **Obtain the APK File**

    * Use ADB command to pull the APK file from the device if the app is already installed.

    ```shell
    shell adb shell pm path com.example.package
    adb pull /data/app/com.example.package-1/base.apk
    ```
4.  **Decompile the APK File**

    * Use APKTool for decompiling the APK to view and analyze the source code.

    ```shell
    shell apktool d base.apk
    ```
5.  **Review the Source Code**

    * Use JADX to convert .dex files to .java files for easy code review.

    ```shell
    shell jadx -d output_dir base.apk
    ```
6. **Check for Hardcoded Sensitive Information**
   * Search for sensitive data (like API keys, credentials) that should not be hardcoded into the app.
7. **Check for Insecure Storage**
   * Identify if the app is storing sensitive data insecurely on the device (like shared preferences, SQLite databases, internal and external storage).
8.  **Convert .dex Files to .jar Files**

    * Use dex2jar tool for conversion and then JD-GUI for viewing the source code.

    ```shell
    shell d2j-dex2jar.sh base.apk
    ```
9. **Identify and Understand Usage of Native Libraries**
   * Check if the app uses native libraries, and if so, how they are used and how they could potentially be exploited.

### Dynamic Analysis

10. **Install the APK on the Device**

    * Use ADB to install the APK onto your device or emulator.

    ```shell
    shell adb install base.apk
    ```
11. **Monitor System Logs**

    * Use `logcat` command to check for errors and exceptions in real time.

    ```shell
    shell adb logcat
    ```
12. **Intercept and Analyze Network Traffic**
    * Set up Burp Suite as a proxy for the device and inspect the traffic.
13. **Capture Traffic with tcpdump and Analyze with Wireshark**

    * Use tcpdump to capture packets and Wireshark to analyze the network protocol details.

    ```shell
    shell adb shell tcpdump -w /sdcard/capture.pcap
    adb pull /sdcard/capture.pcap
    ```
14. **Run the Application and Observe its Behavior**
    * Manually run the application, perform various operations, and observe its behavior and outputs.
15. **Use Drozer to Map the Attack Surface**

    * Use Drozer for a detailed analysis of the application's attack surface.

    ```shell
    shell drozer console connect
    run app.package.attacksurface com.example.package
    ```
16. **Use Frida/Objection for Dynamic Instrumentation and SSL Pinning Bypass**
    * Use Frida or Objection to modify the app behavior during runtime, and also to bypass SSL Pinning if implemented.
17. **Analyze SQLite Databases if Present**

    * Check for insecure data storage or sensitive information stored in SQLite databases.

    ```shell
    shell adb shell run-as com.example.package sqlite3 databases/database.db .dump
    ```

### Advanced Analysis

18. **Identify and Exploit Intent-Based Vulnerabilities**
    * Use Drozer or other tools to identify potential vulnerabilities with Android Intents.
19. **Examine File System for Insecure File Permissions**

    * Use ADB shell to navigate through the file system and check permissions.

    ```shell
    shell adb shell
    ```
20. **Automate UI Interactions for Testing**
    * Use tools like Appium or ADB to script UI interactions.
21. **Fuzz Unexpected Inputs to the Application**
    * Generate unexpected or random inputs to the application to see how it reacts.
22. **Reverse Engineer Native Code**
    * Use tools like radare2 or Ghidra to reverse engineer the native libraries.
23. **Patch and Repackage the Application as Needed**
    * Use APKTool to decompile the app, modify the code, and then recompile the APK.
24. **Verify Secure Network Traffic and Analyze Protocols like MQTT, XMPP, etc.**
    * Besides HTTP and HTTPS, check for secure implementation of other protocols like MQTT, XMPP etc.
25. **Check for Weak Cryptographic Functions**
    * Check for usage of weak or deprecated cryptographic functions, or improper usage of cryptography.
26. **Check for Code Obfuscation and Anti-Reversing Techniques**
    * Check for presence of anti-reversing measures like code obfuscation, root detection, debugger detection etc.

### Post Exploitation

27. **Exploit Identified Vulnerabilities**
    * Use discovered vulnerabilities to demonstrate potential impacts.
28. **Document Findings and Create a Report**
    * Document all findings, provide evidence, and create a detailed report.

Please note, every application will have different security controls in place, so this checklist may need to be adjusted based on the application you're testing. Always follow ethical guidelines and only test applications you have permission to test.

### Using logcat in Android Pentesting

`logcat` is a command-line tool that dumps a log of system messages, which can include information about system processes, errors, warnings, stack traces, and more.

Here are some steps on how to use `logcat`:

1.  **Basic logcat Usage**

    * Simply type `adb logcat` in your command line and it will start displaying a real-time feed of system messages.

    ```shell
    shell adb logcat
    ```
2.  **Filtering logcat Output**

    * You can filter `logcat` output for easier reading. For instance, you can filter by log level (Error, Warning, Info, Debug, Verbose), or by the source of the log messages (tag).

    ```shell
    shell adb logcat *:E
    adb logcat -s "YourAppTag"
    ```
3.  **Clearing the Log**

    * If you want to clear the current log, you can use the `-c` command.

    ```shell
    shell adb logcat -c
    ```
4.  **Saving log Output to a File**

    * If you need to save the log output for later analysis or to share it with your team, you can direct the output to a file.

    ```shell
    shell adb logcat > logcat.txt
    ```
5. **Analyzing the Log**
   * Once you have the log data, you can start analyzing it. You should look for any sensitive data that shouldn't be logged, such as passwords, credit card numbers, or personally identifiable information (PII). You should also look for error messages or exceptions that might reveal issues with the application, such as potential vulnerabilities or areas of weak security.