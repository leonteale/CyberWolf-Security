# 3260 - iSCSI

## Discovery

This command initiates the iSCSI discovery process for the specified IP address, allowing the system to find and list available iSCSI targets on that address.

```
sudo iscsiadm -m discovery -t st -p <IP>
```

### Login

```
 sudo iscsiadm --mode node --targetname iqn.2004-04.com.qnap:ts-453bu:iscsi.berwinsveeam.3914ef --portal 172.20.0.165 --login
```

Then check dmesg to see where the iscsi was mounted to

```
dmesg
```

### Mount

```
mkdir /tmp/temp
mount /dev/sdb2 /tmp/temp
cd /tmp/temp
ls
```

### Logout

```
sudo iscsiadm --mode node --targetname iqn.2004-04.com.qnap:tvs-873:iscsi.hdd.5057c2 --portal 10.0.1.29 --logout
```
