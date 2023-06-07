---
description: >-
  This guide provides a detailed checklist for Android mobile application
  penetration testing.
---

# Checklist and Methodology

## Checklist

**App Package Structure Analysis:**

* [ ] Analyse the APK file with tools such as APKTool, Jadx, or similar.
* [ ] Look for embedded secrets, such as API keys, in the decompiled code.
* [ ] Review the AndroidManifest.xml for sensitive data or misconfigurations.

**Insecure Data Storage:**

* [ ] Check shared preferences for sensitive data stored in clear text.
* [ ] Check SQLite databases for sensitive data.
* [ ] Review the internal & external storage directories for insecurely stored data.

**Network Analysis:**

* [ ] Use Wireshark or similar tools to analyse network traffic.
* [ ] Check if the app is transmitting sensitive data in plain text over the network.
* [ ] Verify if the app is implementing certificate pinning to prevent man-in-the-middle attacks.

**Log Analysis:**

* [ ] Use Logcat to collect system logs and analyse them for sensitive data leakage.

**Reverse Engineering and Code Analysis:**

* [ ] Use tools such as Jadx or JD-GUI to decompile the APK and analyse the source code.
* [ ] Check for hard-coded secrets, insecure functions, or insecure implementations.

**Runtime Analysis:**

* [ ] Use Frida, Xposed, or similar frameworks to hook into the app's methods during runtime for dynamic analysis.
* [ ] Test the app's behaviour and response to certain inputs or manipulated environment variables.

**Authentication Mechanisms:**

* [ ] Check for user enumeration vulnerabilities.
* [ ] Verify if the app uses secure protocols during authentication.
* [ ] Check for weak password policies.

**Session Management:**

* [ ] Check if sessions expire appropriately.
* [ ] Check if the app handles multiple simultaneous sessions correctly.

**Data Encryption:**

* [ ] Verify if data at rest is encrypted.
* [ ] Check if data in transit is encrypted.

**App Permissions:**

* [ ] Review the permissions requested by the app.
* [ ] Check if the app requests more permissions than necessary.

**Additional points:**

* [ ] Check if the app has any mechanisms to detect if the device is rooted or jailbroken.
* [ ] Verify if the app has proper error handling, ensuring it doesn't reveal sensitive information when errors occur.
* [ ] Check the resiliency against reverse engineering and tampering by using tools like Objection or APKtool.

Remember, the above points form a general checklist, and testing methods might change based on the app's functionality, architecture, and the kind of data it handles.

## Methodology

## Pre-requisites

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
   * [scrcpy](https://github.com/Genymobile/scrcpy/blob/master/doc/linux.md) - Mirror and control your phone screen using ADB

## Static Analysis

3.  **Obtain the APK File**

    * Use ADB command to pull the APK file from the device if the app is already installed.

    ```shell
    adb shell pm path com.example.package
    adb pull /data/app/com.example.package-1/base.apk
    ```

    \
    or simply use the .apk file provided by the client (or downloaded from google play store)
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

### Certificate Pinning

While there's not a specific setting in the app you can check, you can inspect the app's code or configuration files to see if certificate pinning is being used. Here's how you can do it:

1. **Inspect the network security configuration**: \
   In Android 7.0 (API level 24) and later, you can configure how your app trusts custom CAs by adding a network security configuration file to your app. The file resides in the `res/xml` directory and is pointed to by the `android:networkSecurityConfig` attribute in the application tag in the manifest. If the app has this file, check it for any `<pin-set>` entries, which suggest the use of certificate pinning.
2. **Decompile the APK**: \
   Tools like `jadx` or `apktool` can decompile the APK file to inspect its source code. Look for uses of classes or methods related to certificate pinning. For example, in OkHttp library, `CertificatePinner` class is used for pinning. If you find such code, it's an indication that certificate pinning might be used.
3. **Inspect SSL libraries**: \
   If the app uses native code (C/C++), it might be using a custom SSL library. Check for .so (shared object) files in the lib directory of the unpacked APK. You might be able to glean some info about the use of certificate pinning from the names and versions of these libraries.

Remember that these checks can give you some clues about whether certificate pinning is used, but the definitive test would be trying to intercept the app's SSL traffic using a proxy tool, as I described in the previous message.

### Tap-Jacking

Tapjacking, also known as UI redress attack, is a malicious technique where an attacker overlays a transparent window over the application's user interface, tricking the user into clicking on hidden buttons or links. This could lead to unauthorised actions or disclosure of sensitive information.

Here's how you can check if an Android application is vulnerable to tapjacking:

1. **Check the Android Manifest file**: Look for the `android:filterTouchesWhenObscured` attribute. If the value is `true`, it means the app has tapjacking protection. If the value is `false` or the attribute is not present, it suggests that the app could be vulnerable to tapjacking. The attribute can be set at the individual view level as well.
2. **Dynamic Analysis**: You can also create a malicious app to overlay on the target app to check for tapjacking vulnerability. You would need to:
   * Create an overlay view in the malicious app with the same layout as the target app.
   * Set the view as clickable and focusable.
   * Start the malicious app and place it on top of the target app.

You can also install this tapjacking-poc.apk which will allow you to perform the above.

```bash
git clone https://github.com/mwrlabs/tapjacking-poc.git
cd tapjacking-poc
cd bin
adb install tapjacking.apk
```

If the target app is still able to receive touch events while being obscured by the overlay, then it's vulnerable to tapjacking.

3. **Tools**: Tools such as Drozer can also help in identifying tapjacking vulnerabilities.

Remember that tapjacking is more a user-interface concern rather than a data security concern. While it could be used as part of a larger attack, by itself it does not expose sensitive data or system resources.

## Dynamic Analysis

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



    This will start printing the log data in the console.\


    ### Filtering Logs

    However, the amount of log data can be overwhelming. Therefore, you can use filters to narrow down the logs.\


    **Filter logs by priority level**

    Log entries have a tag and a priority associated with them. The priority levels are as follows:

    * V — Verbose (lowest priority)
    * D — Debug
    * I — Info
    * W — Warning
    * E — Error
    * F — Fatal
    * S — Silent (highest priority, on which nothing is ever printed)

    You can filter log output by priority level, for example:

    ```shell
    adb logcat *:E
    ```

    This command will print only error messages.

    \
    **Filter logs by TAG**

    Every log message has a tag which is usually defined by the class name. If you want to filter out logs from a specific tag, you can use:

    ```shell
    adb logcat -s "YourTAG"
    ```

    \
    In Android's logging system, a "tag" is a short string that you define and associate with your log messages. Tags act as identifiers or labels that can help you filter, sort, and search for specific types of log data. They're especially helpful when you're dealing with a lot of log data from different sources.\


    Typically, developers use the name of the class or activity that the log call is in as the tag, so it's easier to identify where the log call is coming from.\


    For example, a log message could look like this:

    ```java
    Log.i("MainActivity", "Button clicked!");
    ```

    \
    Here, `"MainActivity"` is the tag, and `"Button clicked!"` is the log message.

    In a penetration testing scenario, identifying the tag depends on what you're trying to do:

    1.  **If you're examining a specific app,** you can use the package name of the app as a starting point. For example, if you're testing an app with the package name `com.example.myapp`, you can start with:

        ```shell
        adb logcat | grep "com.example.myapp"
        ```
    2. **If you're looking at the system logs in general,** there may not be a specific tag you should be looking for. Instead, you could grep for terms that are often related to sensitive information, such as "password", "token", "username", etc.
    3. **If you have the source code of the app,** you can look at the code to identify what tags the app is using for its log messages.

    \
    **Combining TAG and Priority Filters**

    You can also combine these filters:

    ```shell
    adb logcat YourTAG:E *:S
    ```

    This will print only the error (E) messages for `YourTAG` and silence (S) everything else.\


    ### Searching for Sensitive Information

    One of the most basic ways to search for sensitive information in logs is simply to use the `grep` command. This is not specific to `logcat` but can be used with it. For example:

    ```shell
    adb logcat | grep "password"
    ```

    This command will show you any log entries that include the word "password". You can replace "password" with any other word you're interested in, such as "token", "key", "pin", etc.\

12. **Intercept and Analyse Network Traffic**
    * Set up Burp Suite as a proxy for the device and inspect the traffic.

### **Capture Traffic - tcpdump and Wireshark**

* Use tcpdump to capture packets and Wireshark to analyse the network protocol details.

```shell
adb shell tcpdump -w /sdcard/capture.pcap
adb pull /sdcard/capture.pcap
```

\
If you get issue with tcpdump not being installed on the device then you an do the following:

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
    This is the process of retrieving the `.pcap` file and analysing it.

    1.  **Get the .pcap file to your machine**

        You can use the `adb pull` command to retrieve the `capture.pcap` file from your Android device to your local machine:

        ```shell
        adb pull /sdcard/capture.pcap
        ```

        This command will download the `capture.pcap` file from your Android device to the current directory on your local machine.
    2.  **Analyse the .pcap file**

        You can use a tool like Wireshark or `tcpdump` to analyse the `.pcap` file.

        *   **Wireshark**: Wireshark has a GUI that makes it easy to analyse `.pcap` files. You can install Wireshark and open the `.pcap` file with it:

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

            As with `tcpdump`, Wireshark can only analyse unencrypted traffic. If the network traffic in your `.pcap` file is encrypted (e.g., HTTPS, SSH), Wireshark will not be able to decipher the contents of the packets. Also, remember that sensitive information like passwords or PINs should not be sent over the network in plaintext.
        *   **tcpdump**: `tcpdump` can also be used to analyse `.pcap` files from the command line. Here's an example that prints all packets:

            ```shell
            tcpdump -r capture.pcap
            ```

        When analysing the `.pcap` file, you're looking for anything out of the ordinary, such as unusual traffic patterns, unencrypted sensitive data, or communications with suspicious IP addresses.\
        \
        **Using `tcpdump` to analyse `.pcap` files**

        1.  Here are some examples of how you might use `tcpdump` to analyse a `.pcap` file:

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

            Please note that `tcpdump` can only analyse unencrypted traffic. If the network traffic in your `.pcap` file is encrypted (e.g., HTTPS, SSH), `tcpdump` will not be able to decipher the contents of the packets.\


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
4. **Run the Application and Observe its Behaviour**
   * Manually run the application, perform various operations, and observe its behaviour and outputs.
5.  **Use Drozer to Map the Attack Surface**

    * Use Drozer for a detailed analysis of the application's attack surface.

    ```shell
    drozer console connect
    run app.package.attacksurface com.example.package
    ```
6. **Use Frida/Objection for Dynamic Instrumentation and SSL Pinning Bypass**
   * Use Frida or Objection to modify the app behaviour during runtime, and also to bypass SSL Pinning if implemented.
7.  **Analyse SQLite Databases if Present**

    * Check for insecure data storage or sensitive information stored in SQLite databases.

    ```shell
    adb shell run-as com.example.package sqlite3 databases/database.db .dump
    ```

    \
    SQLite is a lightweight disk-based database and it's used by many Android applications to store data. Sometimes sensitive data can be found in SQLite databases, so they should always be checked during a penetration test.

### Using logcat

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
5. **Analysing the Log**
   * Once you have the log data, you can start analysing it. You should look for any sensitive data that shouldn't be logged, such as passwords, credit card numbers, or personally identifiable information (PII). You should also look for error messages or exceptions that might reveal issues with the application, such as potential vulnerabilities or areas of weak security.

### Locating SQLite Databases

SQLite databases are usually stored in the `/data/data/<package_name>/databases/` directory on the device. However, applications can potentially store databases in other locations within their data directory.\


Use the following command to navigate to the application's main directory:

```shell
adb shell run-as org.app.mobile_app
```

\
The `run-as` command allows you to run commands as if you were the app that owns the data directory, but it only works for apps that are debuggable. Debuggable apps are typically only found during development, and should not be used in production due to security concerns.\


If the app is not marked as debuggable in its manifest file (AndroidManifest.xml), you won't be able to use the `run-as` command. This is a security feature to prevent unauthorized access to the app's private data.\


This does present a challenge when you're trying to perform a penetration test on a non-debuggable app. If the device is rooted, you can access the app's data directory without using the `run-as` command. However, be aware that rooting a device can pose its own security risks.\
\
Then you can search for SQLite database files recursively from the application's root directory using the `find` command. This will help you discover any databases that might be stored in non-standard locations:

```shell
find . -name "*.db"
```

\
This command will list all `.db` files (which are typically SQLite databases) in the current directory and all subdirectories.\


To find SQLite databases within an Android application's directory, even if they don't have a `.db` extension, you can use the `grep` command with the `-lr` flags and the `SQLite format 3` magic string. The `-l` flag tells `grep` to output the names of files where the pattern has been found, and the `-r` flag tells it to search recursively.\


```shell
find . -type f -exec grep -l "SQLite format 3" {} \;
```

\
This command will search all files in the current directory and its subdirectories for the `SQLite format 3` string, and print the names of the files where the string was found.\
\
If this finds a few databases, you can quickly dump them all to the /sdcard/ using the following command:\


```shell
for db in $(find . -type f -exec grep -l "SQLite format 3" {} \;); do sqlite3 $db ".output /sdcard/databases/$(basename $db).dump"; done
```

\
\
Before you run this command, ensure that the `databases` directory exists on your `sdcard`. If not, you can create it using the following command:

```bash
adb shell mkdir /sdcard/databases
```

\
Now, when you want to pull all the databases to your local machine, you can do so with one command:

```bash
adb pull /sdcard/databases .
```

\
This will copy all the files from the `databases` directory on your `sdcard` to the current directory on your local machine.\
\
If it seems like the `sqlite3` binary is not available on your Android device's shell. This binary is not included by default on some Android distributions for security reasons.\


You can try to use `adb` to push the `sqlite3` binary from your Kali machine to your Android device. The `sqlite3` binary is usually included with the Android SDK platform-tools, but if it's not present, you can download it from a trusted source.\


You can push it from your local machine to your Android device using the `adb push` command. The binary must be located in the `/system/xbin` directory and must have executable permissions. Here's how to do that:

1. Find the `sqlite3` binary on your local machine. If you have the Android SDK installed, it might be located in the `platform-tools` directory. If not, you might need to download it from a trusted source. Mine was located in: \
   \
   /usr/lib/android-sdk/platform-tools/sqlite3\

2. Push the binary to your Android device:

```bash
adb push /usr/lib/android-sdk/platform-tools/sqlite3 /data/local/tmp/
```

\
Move the binary to the `/system/xbin` directory and give it executable permissions:

```bash
adb shell
su
chmod 755 /data/local/tmp/sqlite3
export PATH=$PATH:/data/local/tmp
```

\
Now you should be able to use the `sqlite3` command in your Android shell\
\
If you get "not executable: 64-bit ELF file" the error typically indicates that you are trying to run an executable compiled for a different architecture than the one you're currently running.\


You can download a different version of SQLite from the SQLite's official download page.

Here is an example on how to download, push and give executable permissions to sqlite3 for arm64:\


<pre class="language-bash"><code class="lang-bash"># Downloading SQLite3 for ARM architecture
wget https://www.sqlite.org/2023/sqlite-android-3420000.aar

# Unzipping the package
unzip sqlite-android-3420000.aar -d sqlite-android-3420000

# Change to the sqlite directory
cd sqlite-android-3420000/jni/arm64-v8a/

# Pushing libsqliteX.so to the device
adb push libsqliteX.so /data/local/tmp/

<strong>#In the adb shell session on your Android device, set the file permissions of the libsqliteX.so file to make it executable.
</strong>chmod +x /data/local/tmp/libsqliteX.so

#Finally, you should be able to execute the SQLite binary by specifying the full path to the file.
/data/local/tmp/libsqliteX.so

# Giving executable permissions to sqlite3 inside the adb shell
<strong>chmod 755 /data/local/tmp/libsqliteX.so
</strong>export PATH=$PATH:/data/local/tmp/
</code></pre>

\
The above steps will push sqlite3 to the /data/local/tmp/ directory and give it executable permissions.

\
Please note, the URL in the wget command above may vary depending on the version of SQLite and the architecture of your device. You should download the version that suits your requirements.

\
After executing the above steps, you can use the sqlite3 command as `/data/local/tmp/sqlite3`.\
\
If you are not sure what version of Android arcitechture you have you can run:\


```shell
getprop ro.product.cpu.abilist

### Mine shows:
# arm64-v8a,armeabi-v7a,armeabi
```



Keep in mind that `grep` might not be available on all devices, so you may need to pull the files to your local machine to perform the search:

```shell
adb pull /data/data/org.innox.c4u_mobile_app . grep -lr "SQLite format 3" org.innox.c4u_mobile_app
```

\
This will first copy the app's data directory to your local machine, and then perform the search locally.\
\
Please note that the \`adb pull\` command requires appropriate permissions to access the app's data directory. If the app is not debuggable and the device is not rooted, you may not be able to access the app's private data.

### Dumping Database Content

\
Once you've found a database, you can use the `sqlite3` command-line tool to inspect it. The `.dump` command will print the contents of the entire database:

```shell
sqlite3 databases/database.db .dump
```

\
Remember to replace `databases/database.db` with the actual path to the database file you're interested in.



### Querying Specific Tables

\
If you know the structure of the database, you can query specific tables or data using standard SQL syntax. For example:

```shell
sqlite3 databases/database.db "SELECT * FROM user_table"
```

\
This command will select all data from the `user_table`. Replace `user_table` with the actual table name you are interested in.

or you can use a GUI for manual checking

```shell
sqlitebrowser

```

### Exporting Databases

\
If the database is large or you want to use a GUI tool like DB Browser for SQLite for a more convenient view, you can pull the database file from the device to your local machine:

```shell
adb exec-out run-as org.app.mobile_app cat databases/database.db > local_database.db
```

Or from within the device shell

```shell
adb shell
    su
    cd /data/data/org.app.mobile_app/databases/
    mkdir -p /sdcard/databases/
    cp * /sdcard/databases/

adb pull /sdcard/databases pull
```

\
This command will create a copy of the `database.db` on your local machine with the name `local_database.db`. You can open `local_database.db` with any SQLite database viewer on your local machine.



## Advanced Analysis

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
24. **Verify Secure Network Traffic and Analyse Protocols like MQTT, XMPP, etc.**
    *   Besides HTTP and HTTPS, check for secure implementation of other protocols like MQTT, XMPP etc.\
        \
        Network protocol analysis on Android is not straightforward due to the sandboxed nature of Android applications, but there are a few ways to approach it.\


        **Wireshark** and **tcpdump** are two tools that can be used to capture and analyse network traffic, which includes traffic from protocols like MQTT, XMPP, etc. We already discussed how to use these tools earlier.\


        However, to verify secure network traffic and analyse such protocols specifically for an Android application, you'll need to capture the traffic from that application. This can be done using VPN-based Android apps that can capture traffic from your device, or by routing your device's traffic through a proxy running on another machine.\


        #### Steps to analyse network protocols for an Android app



        **1. Setup a proxy to capture traffic:**\


        You can set up a proxy using tools like Wireshark on a separate machine, and then route all the traffic from your Android device through that proxy. To do this, you'll need to:\


        1.1. Set up the proxy on a separate machine and note down the IP address and port number.\


        1.2. On your Android device, go to Wi-Fi settings, long-press on the connected Wi-Fi network, and modify the network configuration to use the manual proxy settings. Enter the IP address and port number of your proxy machine.\


        1.3. Now, all the traffic from your Android device will go through the proxy, and you can capture this traffic.\


        **2. Capture and analyse the traffic:**\


        2.1. Start capturing traffic on your proxy using Wireshark or tcpdump.

        \
        2.2. Use the Android app as normal to generate network traffic.

        \
        2.3. Stop the capture on your proxy.\


        2.4. Analyse the captured traffic using Wireshark's various features. For example, you can filter by protocol (e.g., MQTT), inspect individual packets, follow TCP streams, etc.\


        **3. Use an Android app to capture traffic:**\


        3.1. If setting up a separate proxy is not feasible, there are Android apps available that can capture network traffic from your device. These apps work by creating a VPN connection on the device and capturing all traffic that goes through it.\


        3.2. One such app is [Packet Capture](https://play.google.com/store/apps/details?id=app.greyshirts.sslcapture\&hl=en\&gl=US). After installing the app, you can start the capture, use your app to generate traffic, and then stop the capture. The app will show the captured traffic, which you can then analyse.\


        **4. Check for secure implementation:**\


        To check for secure implementation of the protocols, you should check whether the communication is encrypted, check the certificates used for encryption, and try to decrypt the communication if possible.\
        \
        In order to use `tcpdump` to monitor for specific protocols such as MQTT and XMPP, you can use the `port` filter.

        1. **MQTT:** MQTT by default uses TCP port 1883 for non-encrypted communication and TCP port 8883 for encrypted communication.\


        ```bash
        tcpdump -i any -s 0 -w mqtt.pcap port 1883 or port 8883
        ```

        2. **XMPP:** XMPP uses TCP port 5222 for client connections and TCP port 5269 for server connections. For encrypted connections, port 5223 is often used.\


        ```bash
        tcpdump -i any -s 0 -w xmpp.pcap port 5222 or port 5223 or port 5269
        ```

        \
        Remember that MQTT and XMPP are just examples. You should check which protocols the Android app you are testing is using and what ports those protocols use.\


        If you don't know which protocols the app is using, you could monitor all TCP and UDP traffic and then analyse the pcap file using a tool like Wireshark to see which protocols are present:\


        ```bash
        tcpdump -i any -s 0 -w all_traffic.pcap
        ```

        \
        Wireshark can recognize many different protocols automatically and can help you filter and analyse the pcap file easily.

        \
        Please remember to replace `any` in the `-i` parameter with the actual interface you are listening on if you know it.

        \
        Please note, though `tcpdump` is powerful, it only gives you the raw network packets. To interpret and analyse those packets, a packet analyser like Wireshark will provide a more in-depth view into the packet data.
25. **Check for Weak Cryptographic Functions**
    * Check for usage of weak or deprecated cryptographic functions, or improper usage of cryptography.
26. **Check for Code Obfuscation and Anti-Reversing Techniques**
    * Check for presence of anti-reversing measures like code obfuscation, root detection, debugger detection etc.

## Post Exploitation

27. **Exploit Identified Vulnerabilities**
    * Use discovered vulnerabilities to demonstrate potential impacts.
28. **Document Findings and Create a Report**
    * Document all findings, provide evidence, and create a detailed report.

Please note, every application will have different security controls in place, so this checklist may need to be adjusted based on the application you're testing. Always follow ethical guidelines and only test applications you have permission to test.