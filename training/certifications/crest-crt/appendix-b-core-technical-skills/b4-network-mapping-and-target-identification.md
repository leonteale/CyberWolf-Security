# B4 - Network Mapping & Target Identification

> Analysis of output from tools used to map the route between the engagement point and a number of targets. Network sweeping techniques to prioritise a target list and the potential for false negatives.

Essentially, this is NMAP and ping sweeping with some false negative checking.&#x20;

## Basic NMAP commands

### nmap versions

## Ping sweep

### ping sweep with nmap

```
nmap -sn IP/24
nmap -sn -iL IPlist.txt
```

There are other tools available to perform ping sweeps. Some of these are below:

```
fping -g IP/24
fping -f IPlist.txt

ping IP    #IPv4 by default
ping -6 IP #IPv6
```

## False Negative

It is important to note that performing a ping sweep with nmap using -sn uses ICMP in order to test for a response to a ping.  If a machine (or firewall) prevents ICMP requests, then the machine will return as offline. This is considered a FALSE NEGATIVE.&#x20;

##
