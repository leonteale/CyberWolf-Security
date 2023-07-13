# 3260 - iSCSI

## Discovery

This command initiates the iSCSI discovery process for the specified IP address, allowing the system to find and list available iSCSI targets on that address.

```
sudo iscsiadm -m discovery -t st -p <IP>
```

Then we try login

```
 sudo iscsiadm --mode node --targetname iqn.2004-04.com.qnap:ts-453bu:iscsi.berwinsveeam.3914ef --portal 172.20.0.165 --login
```

Then check dmesg to see where the iscsi was mounted to

```
dmesg
```

Then mount it to a directory

```
mkdir /tmp/temp
mount /dev/sdb2 /tmp/temp
cd /tmp/temp
ls
```
