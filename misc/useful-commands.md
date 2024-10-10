# Useful Commands

## Extract domain users from winscanx dcenum

```
cat dcenum.txt | grep "Domain Users" -C 2 | grep Username | awk {'print $2'}
```

## extract domain users from enum4linux dcenum

```
cat dcenum.txt | grep "Domain Users" | awk {'print $8'} | cut -d \\ -f 2
```

## List a user's groups

## Winscanx

{% code overflow="wrap" %}
```bash
grep -B 2 "user" dcenum.txt | grep "Group Name" | awk -F ': ' '{print $2}'
```
{% endcode %}

### enum4linux

```
// Some code
```

## Extract domain users from 'net user'

### On the windows DC/DS, cmd.exe:

```
net user > output.txt
```

### Using bash:

```
cat output.txt |awk {'print $1,"\n",$2,"\n",$3'} | awk {'print $1'} > users.txt
```

## Add a local user and add to administrators group

```
net user leon password /add
net localgroup administrators /add
```

## Compile c scripts

```
gcc -s hello.c -o exploit
```

If the machine does not have GCC installed, it can be compiled on the attacker machine, taking note of the system architecture first, using the following syntax:

### For x64 bit:

```
gcc -m64 hello.c -o exploit
```

### For x32 bit:

```
gcc -m32 hello.c -o exploit
```

## Find text between 2 strings

```
echo "Here is a string" | grep -o -P '(?<=Here).*(?=string)'
```

### Extract email address from file

```
grep -E -o "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" file.txt
```

## find users with specific weak passwords from pipal

```
grep -E '(Password1|Password1Password1|Hire123!|Trucks123Trucks123|PasswordPassword1\.|Service1Service1|Winteriscoming@23|Cryingfr33m@n23|Password1!)' crackedhashes.txt | awk -F: '{print $3 ":" $1}' | sort | awk -F: '{print $2 " : " $1}' > weakpasses_output.txt
```

## Add new entries to password leak (combs) for h8mail

This will take a file with the content format of\
email:password\
and then put it inside of the COMBS password leak into the relevant folder structure format.&#x20;

### Linux:

```
while IFS=: read -r email password; do echo "$email:$password" >> /Wordlists/COMB/CompilationOfManyBreaches/data/${email:0:1}/${email:1:1}/${email:2:1}; done < your_file.txt
```

### Windows:

```
@echo off
for /F "usebackq tokens=1,2 delims=:" %%a in (your_file.txt) do (
    echo %%a:%%b >> D:\CompilationOfManyBreaches\data\%%a\%%b\%%c
)
```

## Searching shares for passwords

1.  To find FILES with "password" in the file name, use the `find` command:

    {% code overflow="wrap" %}
    ```bash
    find /tmp/shared/ -type f -iname "*password*"
    ```
    {% endcode %}



This will search for FOLDERS

```bash
find /tmp/shared/ -iname "*password*"
```

\
This command will search the `/tmp/shared/` directory and all of its subdirectories for files (`-type f`) with "password" in the name. The `-iname` option makes the search case-insensitive.

1.  To find files that contain "password" in their contents, use `find` in combination with `grep`:

    {% code overflow="wrap" %}
    ```bash
    find . -type f -exec grep -iHn "password" {} \;
    ```
    {% endcode %}

    This command searches all files in the `/tmp/shared/` directory and its su

You can ignore/suppress the 'binary file matches' lines by adding -I to the grep. such as:

{% code overflow="wrap" %}
```bash
find . -type f -exec grep -iIHn "password" {} \;
```
{% endcode %}

To read the contents of the binary though, you can simply use 'strings'

{% code overflow="wrap" %}
```bash
strings ./file.msi | grep -i password

strings .file.msi | grep -A 1 -B 1 -i password
```
{% endcode %}

### Mount the share

1.  **Create a Mount Point**: Decide where you want to mount the share and create a directory for it. For example:

    ```bash
    sudo mkdir /mnt/netlogon
    ```
2.  **Mount the Share**: Use the `mount` command to mount the share. You'll need to know the username and password for the share. Replace `USERNAME` and `PASSWORD` with your credentials:

    ```bash
    sudo mount -t cifs //domainserver.local/NETLOGON /mnt/netlogon -o username=USERNAME,password=PASSWORD
    ```

    If you need to specify a domain, you can add the `domain` option:

    ```bash
    sudo mount -t cifs //domainserver.local/NETLOGON /mnt/netlogon -o username=USERNAME,password=PASSWORD,domain=DOMAIN
    ```
3.  **Unmounting**: When you're done, you can unmount the share using:

    ```bash
    sudo umount /mnt/netlogon
    ```

## Find what groups a user is a member of

It will look like this

```
--------------------------------------------
User: leont
Group Name: Domain Users
Group Name: LT-DAVY_JONES_DATA-RW
Group Name: LT-DAVYPRESS-F-RW
Group Name: LT-SPACE-D-RO
Group Name: LT-SPACEPROCESSDATA-RO
Group Name: LT-Data
Group Name: LT-RECORDERS
Group Name: Site-Halifax
Group Name: XEN-ACCESS
--------------------------------------------
```

### winscanx

{% code overflow="wrap" %}
```bash
awk -v user="leont" 'BEGIN {print "\033[0m--------------------------------------------\n\033[32mUser: " user "\033[0m"} /Username:/ {if ($0 ~ "Username:   " user) {print group_name}} {group_name=last; last=$0} END {print "--------------------------------------------"}' winscanx.txt | grep -i --color=always 'admin\|'
```
{% endcode %}

### enum4linux

{% code overflow="wrap" %}
```bash
awk -v user="leont" 'BEGIN {print "\033[0m--------------------------------------------\n\033[32mUser: " user "\033[0m"} $0 ~ "has member: .+\\\\" user {split($0, a, "'\''"); print "Group Name:", a[2]} END {print "--------------------------------------------"}' dcenum.txt | grep -i --color=always 'admin\|'
```
{% endcode %}

For loop

{% code overflow="wrap" %}
```bash
for i in $(cat admins.txt); do awk -v user="$i" 'BEGIN {print "\033[0m--------------------------------------------\n\033[32mUser: " user "\033[0m"} $0 ~ "has member: .+\\\\" user {split($0, a, "'\''"); print "Group Name:", a[2]} END {print "--------------------------------------------"}' dcenum.txt | grep -i --color=always 'admin\|';done
```
{% endcode %}

### Responder - found users unique

{% code overflow="wrap" %}
```bash
find /usr/share/responder/logs -name '*.txt' -newerct 'today 00:00:00' -exec cat {} \; | grep -vE '\$' | awk -F':' '!seen[$1]++'
```
{% endcode %}



### Query LDAP about a user



{% code overflow="wrap" %}
```bash
ldapsearch -x -D "user@domain" -W -C -H ldap://192.168.1.20 -b "dc=domain,dc=gov,dc=uk" "(&(objectClass=user)(samaccountname=USERTOQUERY))"
```
{% endcode %}

### Search for exploits from CVEs

If you export 'vulns' from msfconsole (if you have imported things such as Nessus):



{% code overflow="wrap" %}
```bash
msf6 > vulns -o vulns.csv
```
{% endcode %}

&#x20;You can then extract all the CVEs that are identified using the following command:

{% code overflow="wrap" %}
```bash
cat vulns.csv | awk -F ',' '{gsub(/"/, "", $0); for (i=1; i<=NF; i++) {if ($i ~ /^CVE-/) print $i}}' | sort -u
```
{% endcode %}

You can go 1 step further and automate Searchsploit to try find exploits relating to those CVEs using my script here: [https://raw.githubusercontent.com/leonteale/pentestpackage/master/Utilities/CVE-exploit-finder.sh](https://raw.githubusercontent.com/leonteale/pentestpackage/master/Utilities/CVE-exploit-finder.sh)

This is what a found exploit will look like (you'll recognise it from Searchsploit)

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

or no findings will look like this:

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Change metasploit's search

{% code overflow="wrap" %}
```bash
 features set hierarchical_search_table false
```
{% endcode %}

&#x20;
