# NFS

Finding NFS services:

```
nmap -sV -T4 -p111,2049 <IP>
```

We can verify is the NFS service is actually running by querying a specific port and program number.

```
rpcinfo -p <IP>
   program vers proto   port  service
    100000    4   tcp    111  portmapper
    100000    3   tcp    111  portmapper
    100005    2   udp  50393  mountd
    100005    2   tcp  40745  mountd
    100005    3   udp  44139  mountd
    100005    3   tcp  52651  mountd
    100003    3   tcp   2049  nfs
    100003    4   tcp   2049  nfs
    100227    3   tcp   2049
    100003    3   udp   2049  nfs
    100227    3   udp   2049
    100021    1   udp  50710  nlockmgr
    100021    3   udp  50710  nlockmgr
  
rpcinfo -n 2049 -t <IP> 100003
    program 100003 version 3 ready and waiting
    program 100003 version 4 ready and waiting                                              
```

Now we can view the mounts that have been exported

```
showmount -e <IP>
    Export list for <IP>:
    /export/data
    /export
```

Now create a local directory that you will want to mount the exported share to

```
mkdir /tmp/directory
```

Now we can mount the share

```
sudo mount -t nfs <IP>:/export /tmp/export
    Created symlink /run/systemd/system/remote-fs.target.wants/rpc-statd.service → /lib/systemd/system/rpc-statd.service.
```

We can now see the files and folders mounted to the local directory we created

```
ls -la /tmp/export
    total 12
    drwxr-xr-x  3 root root  4096 Jan 26  2021 .
    drwxrwxrwt 13 root root  4096 Jul 19 14:47 ..
    drwxrwxr-x  5 root 10000 4096 Jan 25 09:53 data
```

From here, if you get "permission denied" when browsing the file and folder. Simple create a user with the same UID. That user should then get full access to the directory
