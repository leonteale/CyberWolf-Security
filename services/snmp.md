---
description: udp/161
---

# SNMP

### Tools

```
snmpget -v 1 -c public IP version
snmpbulkwalk -v 2 -c public IP
```

```
apt-get install snmp
snmpwalk -v 1 -c public IP
snmpwalk -c public -v2c -On IP
```

```
sudo apt install snmpenum
snmpenum 10.129.137.66 public linux.txt
```
