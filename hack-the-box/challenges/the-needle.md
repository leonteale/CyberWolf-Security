---
description: The Needle Challenge Write-Up
---

# The Needle

### Challenge Description

The objective of this challenge was to extract and analyse the contents of a firmware binary (`firmware.bin`) to discover a hidden flag.

### Steps to Solve the Challenge

#### Step 1: Extract the Contents of the Firmware

First, I used `binwalk` to analyse and extract the contents of the `firmware.bin` file. The command used was:

```bash
binwalk --extract --directory data firmware.bin
```

The `binwalk` output indicated that the file contained a Linux kernel, some compressed data, and a SquashFS filesystem. The SquashFS filesystem is typically used in embedded systems, so it was likely that the flag would be found within this filesystem.

#### Step 2: Search for the Flag

After extracting the contents, I used `grep` to search for occurrences of the word "flag" within the extracted files:

```bash
grep -rnw . -e "flag"
```

However, the search results did not directly reveal the flag, but indicated multiple occurrences of the word "flag" in various scripts and libraries.

#### Step 3: Identify Login Credentials

Given the embedded system nature of the firmware, it was likely that some form of authentication was required. I searched for occurrences of the word "login":

```bash
grep -rnw . -e "login"
```

This revealed several interesting scripts, particularly `telnetd.sh`, which referenced a "sign" value in the `/etc/config/sign` file. This was worth investigating further.

#### Step 4: Retrieve the Sign Value

To retrieve the sign value, I located and read the `/etc/config/sign` file:

```bash
cat ./_firmware.bin-0.extracted/squashfs-root/etc/config/sign
```

The contents were:

```
qS6-X/n]u>fVfAt!
```

#### Step 5: Attempt to Login via Telnet

Armed with the possible login credentials, I tried to connect to the remote service using `telnet`:

```bash
telnet 94.237.53.113 30392
```

After several attempts using the username `Device_Admin` and the password from the `sign` file, I successfully logged into the system.

#### Step 6: Capture the Flag

Once logged in, I listed the directory contents and found a file named `flag.txt`. I then displayed the contents of this file:

```bash
cat flag.txt
```

The flag was revealed:

```
HTB{4_hug3_blund3r_d289a1_!!}
```

### Conclusion

This challenge demonstrated the process of reverse engineering a firmware binary to discover hidden credentials and ultimately capture the flag. The key steps involved extracting the filesystem, searching for relevant information, and using that information to gain access to the system.
