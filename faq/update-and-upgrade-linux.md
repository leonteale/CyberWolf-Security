# Update and upgrade Linux

`sudo apt-get update; apt-get upgrade`

``

Distribution upgrade: `apt-get dist-upgrade`

The `apt-get dist-upgrade` command intelligently handles changing dependencies with new versions of packages and will attempt to upgrade the most important packages at the expense of less important ones if necessary. Thus unlike `apt-get upgrade` , the `apt-get dist-upgrade` command may actually remove some packages in necessary instances.



To update Kali, first ensure that `/etc/apt/sources.list` is properly populated:

```
kali@kali:~$ cat /etc/apt/sources.list
# See https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/
deb http://http.kali.org/kali kali-rolling main contrib non-free

# Additional line for source packages
# deb-src http://http.kali.org/kali kali-rolling main contrib non-free
kali@kali:~$
```

After that we can run the following commands which will [upgrade us to the latest Kali version](https://www.kali.org/docs/general-use/updating-kali/).

```
kali@kali:~$ sudo apt update
kali@kali:~$
kali@kali:~$ sudo apt full-upgrade -y
kali@kali:~$
```











