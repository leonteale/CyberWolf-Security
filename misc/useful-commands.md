# Useful Commands

### Extract domain users from winscanx dcenum

```
cat dcenum.txt | grep "Domain Users" -C 2 | grep Username | awk {'print $2'}
```

### extract domain users from enum4linux dcenum

```
cat dcenum.txt | grep "Domain Users" | awk {'print $8'} | cut -d \\ -f 2
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

