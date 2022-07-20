# Useful Commands

### Extract domain users from winscanx dcenum

```
cat dcenum.txt | grep "Domain Users" -C 2 | grep Username | awk {'print $2'}
```

### extract domain users from enum4linux dcenum

```
cat dcenum.txt | grep "Domain Users" | awk {'print $8'} | cut -d \\ -f 2
```

### Extract domain users from 'net user'

On the windows DC/DS, cmd.exe:

```
net user > output.txt
```

Using bash:

```
cat output.txt |awk {'print $1,"\n",$2,"\n",$3'} | awk {'print $1'} > users.txt
```

### Add a local user and add to administrators group

```
net user leon password /add
net localgroup administrators /add
```

### Compile c scripts

```
gcc -s hello.c -o exploit
```

If the machine does not have GCC installed, it can be compiled on the attacker machine, taking note of the system architecture first, using the following syntax:

For x64 bit:

```
gcc -m64 hello.c -o exploit
```

For x32 bit:

```
gcc -m32 hello.c -o exploit
```

### Find text between 2 strings

```
echo "Here is a string" | grep -o -P '(?<=Here).*(?=string)'
```

### Extract email address from file

```
grep -E -o "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" file.txt
```

List processes on host

```
ps
Process List
============

 PID    PPID  Name                                        Arch  Session  User                          Path
 ---    ----  ----                                        ----  -------  ----                          ----
 0      0     [System Process]
 4      0     System                                      x64   0
 88     4     Registry                                    x64   0
 108    1516  fontdrvhost.exe                             x64   2        Font Driver Host\UMFD-2       C:\Windows\System32\fontdrvhost.exe
 304    4     smss.exe                                    x64   0
 356    592   dwm.exe                                     x64   1        Window Manager\DWM-1          C:\Windows\System32\dwm.exe
 364    592   LogonUI.exe                                 x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\LogonUI.exe
 420    408   csrss.exe                                   x64   0
 428    660   svchost.exe                                 x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 524    408   wininit.exe                                 x64   0
 532    516   csrss.exe                                   x64   1
 592    516   winlogon.exe                                x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\winlogon.exe
 604    1720  taskhostw.exe                               x64   2        UNI\cyberes2                  C:\Windows\System32\taskhostw.exe
 656    660   svchost.exe                                 x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 660    524   services.exe                                x64   0
 668    524   lsass.exe                                   x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsass.exe
 804    660   svchost.exe                                 x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 844    592   fontdrvhost.exe                             x64   1        Font Driver Host\UMFD-1       C:\Windows\System32\fontdrvhost.exe
 872    660   svchost.exe                                 x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe

```

Migrate to another process

```
migrate <PID>
```

