# NFS

Finding NFS services:

```
nmap -sV -T4 -p111,2049 <IP>

Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-19 14:58 BST
Stats: 0:00:07 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 0.00% done
Nmap scan report for HOSTNAME (IP)
Host is up (0.00070s latency).

PORT     STATE SERVICE VERSION
111/tcp  open  rpcbind 2-4 (RPC #100000)
2049/tcp open  nfs_acl 3 (RPC #100227)
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
