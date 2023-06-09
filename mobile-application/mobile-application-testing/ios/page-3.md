# Checklist and Methodology

## Checklist

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
