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
    adb shell pm path com.example.package
    adb pull /data/app/com.example.package-1/base.apk
    ```
4.  **Decompile the APK File**

    * Use APKTool for decompiling the APK to view and analyze the source code.

    ```shell
    apktool d base.apk
    ```
5.  **Review the Source Code**

    * Use JADX to convert .dex files to .java files for easy code review.

    ```shell
    jadx -d output_dir base.apk
    ```
6.  **Check for Hardcoded Sensitive Information**

    * Search for sensitive data (like API keys, credentials) that should not be hardcoded into the app.

    when checking for hardcoded sensitive information, you would typically search for things like API keys, credentials, secret tokens, and other sensitive information that is hardcoded into the application. This can often be done by manually reviewing the source code or using a tool to search through the codebase.

    Here's an example of how you might do it with `grep`, a command-line tool available in Unix-like systems:\


    ```shell
    grep -ri "API_KEY" /path/to/source/code
    grep -ri "password" /path/to/source/code
    ```

    \
    In this example, `-r` means recursive (searching through all directories and files), `-i` means case insensitive, and "API\_KEY" or "password" is the string you're searching for.\


    This command would search through the given path for any instance of the strings "API\_KEY" or "password". If the codebase is large, this might produce a lot of output, so you might need to refine your search terms or use more advanced options.\


    You might also search for common terms that might indicate sensitive information, such as "secret", "token", "key", "admin", "user", "login", "credential", etc.\


    Remember, this is just a basic example and may not catch every instance of hardcoded sensitive information.
7. **Check for Insecure Storage**

*   Identify if the app is storing sensitive data insecurely on the device (like shared preferences, SQLite databases, internal and external storage).\
    \
    when checking for insecure storage, you would typically look for places where sensitive data might be stored insecurely. This can include shared preferences, SQLite databases, internal storage, external storage, and so on.\


    Here are some examples in markdown format on how you might perform these checks:

    1.  **Check Shared Preferences**

        * Shared Preferences in Android are often used to store small amounts of user data. They can be accessed in shell via ADB.

        ```shell
        adb shell run-as com.example.package cat shared_prefs/com.example.package_preferences.xml
        ```
    2.  **Check SQLite Databases**

        * Many Android apps use SQLite databases for storing data. They can be accessed in shell via ADB.

        ```shell
        adb shell run-as com.example.package sqlite3 databases/database.db .dump
        ```
    3.  **Check Internal Storage**

        * Apps may also store data in internal storage. This storage is private by default, but you should still check it.

        ```shell
        adb shell run-as com.example.package ls files/
        adb shell run-as com.example.package cat files/file.txt
        ```
    4.  **Check External Storage**

        * External storage is world-readable, and so sensitive data should not be stored here.

        ```shell
        adb shell ls /sdcard/Android/data/com.example.package/
        adb shell cat /sdcard/Android/data/com.example.package/file.txt
        ```

    Remember that this is only a basic guide and actual paths can vary. You should look for any sensitive information that is being stored insecurely, such as usernames, passwords, tokens, private keys, credit card numbers, and so on. Also, consider the security implications of the permissions required by these storage methods.

8. **Convert .dex Files to .jar Files**

* Use dex2jar tool for conversion and then JD-GUI for viewing the source code.

You can download .zip file with .sh and .bat files here [https://github.com/pxb1988/dex2jar](https://github.com/pxb1988/dex2jar)

Unzip it and use like this:&#x20;

```
sh d2j-dex2jar.sh -f -o output_jar.jar apk_to_decompile.apk
```

```shell
d2j-dex2jar.sh base.apk
```

9. **Identify and Understand Usage of Native Libraries**

* Check if the app uses native libraries, and if so, how they are used and how they could potentially be exploited.

in Android, native libraries are typically included as part of an APK file and are used to run code written in languages like C or C++. These libraries often contain the core functionalities of an app. However, they can be a point of concern from a security standpoint, as they are prone to common memory corruption vulnerabilities that can lead to serious exploits.

Here's how you can approach identifying and understanding the usage of native libraries:

1.  **Identify Native Libraries**

    * Extract the APK file and look for `.so` (shared object) files. These are the native libraries. You can use the `unzip` command to extract APK files.

    ```shell
    unzip base.apk
    ```
2.  **Analyze the Native Libraries**

    * Tools like `readelf` and `objdump` can be used to understand more about the binary. For instance, `readelf` can be used to list the symbols used by the binary.

    ```shell
    readelf -Ws /path/to/your/library.so
    ```

    * `objdump` can be used to disassemble the binary.

    ```shell
    objdump -D /path/to/your/library.so
    ```
3. **Reverse Engineer the Native Libraries**
   * Tools like IDA Pro, radare2, or Ghidra can be used to reverse engineer the native libraries. This can help you understand what the code is doing.
4. **Look for Common Vulnerabilities**
   * When analysing native libraries, you should look for common vulnerabilities such as buffer overflows, format string vulnerabilities, and integer overflows. These are common in code written in languages like C and C++.

### Dynamic Analysis

10. **Install the APK on the Device**

    * Use ADB to install the APK onto your device or emulator.

    ```shell
    adb install base.apk
    ```
11. **Monitor System Logs**

    * Use `logcat` command to check for errors and exceptions in real time.

    ```shell
    adb logcat
    ```
12. **Intercept and Analyse Network Traffic**
    * Set up Burp Suite as a proxy for the device and inspect the traffic.
13. **Capture Traffic with tcpdump and Analyse with Wireshark**

    * Use tcpdump to capture packets and Wireshark to analyze the network protocol details.

    ```shell
    adb shell tcpdump -w /sdcard/capture.pcap
    adb pull /sdcard/capture.pcap
    ```

    \
    If you get issue with tcpdump not being installed on the device then you an do the following:\


    1.  **Use an Android device/emulator with root access**

        On a rooted device, you can install `tcpdump` using the appropriate binary for your device's architecture. Here are some links to download tcpdump binaries:

        * [ARM](https://www.androidtcpdump.com/tcpdump)
        * [ARM64](https://www.androidtcpdump.com/tcpdump)
        * [x86](https://www.androidtcpdump.com/tcpdump)

        Once you have the binary, you can push it to the device using the `adb push` command:

        ```shell
        wget https://www.androidtcpdump.com/download/4.99.4.1.10.4/tcpdump
        chmod +x tcpdump
        adb push tcpdump /data/local/tcpdump
        adb shell chmod 755 /data/local/tcpdump
        ```



        The error message "Permission denied" suggests that ADB doesn't have the required permissions to push the file to the specified location on your Android device. This usually happens when your device isn't rooted or if the SELinux policy is preventing write access to the `/data/local` directory.

        Here are a few things you could try:

        1.  **Push to a different location**: Try pushing the file to the `/sdcard` directory. This directory usually allows write access from ADB. Here's an example:

            ```shell
            adb push tcpdump /sdcard/tcpdump
            ```
        2. **Check if your device is rooted**: If your device is not rooted, you'll need to gain root access to push files to the `/data/local` directory.
        3.  **Change SELinux policy**: If your device is rooted and you're still seeing this error, it could be because of the SELinux policy. You can temporarily set the SELinux policy to permissive with the following command:

            ```shell
            adb shell su 0 setenforce 0
            ```

            Then, try the `adb push` command again. Note that changing the SELinux policy can make your device less secure, so only do this if you understand the implications.

        Then, to capture packets:

        ```shell
        adb shell su -c '/data/local/tcpdump -w /sdcard/capture.pcap'
        ```

        \
        Make sure that it is capturing on the correct network, it might default to "dummy0". \


        ```shell
        adb shell ip link show
        ```

        This command will list all network interfaces on your Android device. Once you have identified the correct network interface, you can specify it when starting `tcpdump`.



        So, if you're looking to monitor network traffic from an app, it would likely be going through this interface (assuming the app is using WiFi to communicate). You can specify `wlan0` when starting `tcpdump` as follows:

        ```shell
        adb shell su -c '/data/local/tcpdump -i wlan0 -w /sdcard/capture.pcap'
        ```

        \
        This is the process of retrieving the `.pcap` file and analyzing it.

        1.  **Get the .pcap file to your machine**

            You can use the `adb pull` command to retrieve the `capture.pcap` file from your Android device to your local machine:

            ```shell
            adb pull /sdcard/capture.pcap
            ```

            This command will download the `capture.pcap` file from your Android device to the current directory on your local machine.
        2.  **Analyze the .pcap file**

            You can use a tool like Wireshark or `tcpdump` to analyze the `.pcap` file.

            *   **Wireshark**: Wireshark has a GUI that makes it easy to analyze `.pcap` files. You can install Wireshark and open the `.pcap` file with it:

                ```shell
                sudo apt install wireshark
                wireshark capture.pcap
                ```

                \
                In the Wireshark interface, you can use the display filter input at the top of the window to filter the displayed packets:

                1.  **Show all IP addresses**

                    You don't need to apply any specific filter to show all IP addresses. By default, Wireshark displays all packets in the `.pcap` file.
                2.  **Filter by a specific IP address**

                    To filter packets by a specific IP address, you can use the following filter:

                    ```
                    ip.src == 192.168.1.1
                    ```

                    Replace `192.168.1.1` with the IP address you're interested in. This filter will show all packets where the source IP address is the one you specified.
                3.  **Filter by HTTP requests**

                    To filter HTTP requests, you can use the following filter:

                    ```
                    http
                    ```

                    This filter will show all HTTP packets.
                4.  **Search for keywords**

                    To search for specific keywords in the packet data, you can use the `Find Packet` dialog (`Edit > Find Packet`, or Ctrl+F). In the dialog, select `String` for the search type, `Packet bytes` for the search in option, and enter your keyword in the search field.

                As with `tcpdump`, Wireshark can only analyze unencrypted traffic. If the network traffic in your `.pcap` file is encrypted (e.g., HTTPS, SSH), Wireshark will not be able to decipher the contents of the packets. Also, remember that sensitive information like passwords or PINs should not be sent over the network in plaintext.
            *   **tcpdump**: `tcpdump` can also be used to analyze `.pcap` files from the command line. Here's an example that prints all packets:

                ```shell
                tcpdump -r capture.pcap
                ```

            When analyzing the `.pcap` file, you're looking for anything out of the ordinary, such as unusual traffic patterns, unencrypted sensitive data, or communications with suspicious IP addresses.\
            \
            **Using `tcpdump` to analyze `.pcap` files**

            1.  Here are some examples of how you might use `tcpdump` to analyze a `.pcap` file:

                *   **Show all IP addresses**

                    ```shell
                    tcpdump -r capture.pcap -nn
                    ```
                *   **Filter by a specific IP address**

                    ```shell
                    tcpdump -r capture.pcap src 192.168.1.1
                    ```
                *   **Filter by HTTP requests**

                    ```shell
                    tcpdump -r capture.pcap -A 'tcp port 80'
                    ```
                *   **Search for keywords**

                    ```shell
                    tcpdump -r capture.pcap -A | grep 'password'
                    ```

                Please note that `tcpdump` can only analyze unencrypted traffic. If the network traffic in your `.pcap` file is encrypted (e.g., HTTPS, SSH), `tcpdump` will not be able to decipher the contents of the packets.\


                Additionally, it's very uncommon and highly insecure for passwords or PINs to be sent over the network in plaintext. In a secure setup, sensitive information like passwords or PINs would be sent over an encrypted connection, so `tcpdump` would not be able to recover them.\
                \
                For more tcpdump commands, refer to the following: [tcpdump](https://app.gitbook.com/s/uuomMqs69rpH2Wnxhc3O/\~/changes/321/tools/tools/tcpdump)
    2.  **Use an alternative tool**

        If you can't install `tcpdump` on your device, you can use an alternative tool. For example, you could use [`tPacketCapture`](https://play.google.com/store/apps/details?id=jp.co.taosoftware.android.packetcapture\&hl=en\&gl=US) app from the Play Store, which doesn't require root. However, please note that using such apps might not give you the level of control you would get with `tcpdump`.
    3.  **Use `tcpdump` on your host machine**

        If you're testing on an emulator, another option is to use `tcpdump` on your host machine to capture packets. Here's an example:

        ```shell
        sudo tcpdump -i any -w capture.pcap
        ```

        This command tells `tcpdump` to capture packets on all network interfaces (`-i any`) and write them to a file called `capture.pcap` (`-w capture.pcap`).
14. **Run the Application and Observe its Behavior**
    * Manually run the application, perform various operations, and observe its behavior and outputs.
15. **Use Drozer to Map the Attack Surface**

    * Use Drozer for a detailed analysis of the application's attack surface.

    ```shell
    drozer console connect
    run app.package.attacksurface com.example.package
    ```
16. **Use Frida/Objection for Dynamic Instrumentation and SSL Pinning Bypass**
    * Use Frida or Objection to modify the app behavior during runtime, and also to bypass SSL Pinning if implemented.
17. **Analyze SQLite Databases if Present**

    * Check for insecure data storage or sensitive information stored in SQLite databases.

    ```shell
    adb shell run-as com.example.package sqlite3 databases/database.db .dump
    ```

### Advanced Analysis

18. **Identify and Exploit Intent-Based Vulnerabilities**
    * Use Drozer or other tools to identify potential vulnerabilities with Android Intents.
19. **Examine File System for Insecure File Permissions**

    * Use ADB shell to navigate through the file system and check permissions.

    ```shell
    adb shell
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
    adb logcat
    ```
2.  **Filtering logcat Output**

    * You can filter `logcat` output for easier reading. For instance, you can filter by log level (Error, Warning, Info, Debug, Verbose), or by the source of the log messages (tag).

    ```shell
    adb logcat *:E
    adb logcat -s "YourAppTag"
    ```
3.  **Clearing the Log**

    * If you want to clear the current log, you can use the `-c` command.

    ```shell
    adb logcat -c
    ```
4.  **Saving log Output to a File**

    * If you need to save the log output for later analysis or to share it with your team, you can direct the output to a file.

    ```shell
    adb logcat > logcat.txt
    ```
5. **Analyzing the Log**
   * Once you have the log data, you can start analyzing it. You should look for any sensitive data that shouldn't be logged, such as passwords, credit card numbers, or personally identifiable information (PII). You should also look for error messages or exceptions that might reveal issues with the application, such as potential vulnerabilities or areas of weak security.
