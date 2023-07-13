# 3260 - iSCSI

## Discovery

This command initiates the iSCSI discovery process for the specified IP address, allowing the system to find and list available iSCSI targets on that address.

```
sudo iscsiadm -m discovery -t st -p <IP>
```

