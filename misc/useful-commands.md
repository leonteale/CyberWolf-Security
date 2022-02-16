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

Compile c scripts

```
gcc -s filename.c -o filename
```

