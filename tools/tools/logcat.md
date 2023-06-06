---
description: >-
  The logcat command-line tool is used to display system logs on Android
  devices. It's invaluable for debugging and monitoring what your application
  and the Android system are doing.
---

# Logcat

### Basics of Logcat

The basic command to start logcat is:

```shell
adb logcat
```

This will start printing the log data in the console.

### Filtering Logs

However, the amount of log data can be overwhelming. Therefore, you can use filters to narrow down the logs.

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

**Filter logs by TAG**

Every log message has a tag which is usually defined by the class name. If you want to filter out logs from a specific tag, you can use:

```shell
adb logcat -s "YourTAG"
```

**Combining TAG and Priority Filters**

You can also combine these filters:

```shell
adb logcat YourTAG:E *:S
```

This will print only the error (E) messages for `YourTAG` and silence (S) everything else.

### Searching for Sensitive Information

One of the most basic ways to search for sensitive information in logs is simply to use the `grep` command. This is not specific to `logcat` but can be used with it. For example:

```shell
adb logcat | grep "password"
```

This command will show you any log entries that include the word "password". You can replace "password" with any other word you're interested in, such as "token", "key", "pin", etc.
