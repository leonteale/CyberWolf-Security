# B4 - Network Mapping & Target Identification

> Analysis of output from tools used to map the route between the engagement point and a number of targets. Network sweeping techniques to prioritise a target list and the potential for false negatives.

Essentially, this is NMAP and ping sweeping with some false negative checking.&#x20;

## Basic NMAP commands

```
// Service versions
nmap -sV IP

// Auto run scripts against target
nmap -sC IP

// Ignore ICMP checks, assume live.
nmap -Pn IP

// Define the scanning speed, 1-5. speed 3 is the default. 
nmap -T4 IP

// Output results to all file types
nmap -oA outputname

// Only display open ports
nmap --open IP
```

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

traceroute -I #Linux version
tracert -I    #Windows version
```

## False Negative

It is important to note that performing a ping sweep with nmap using -sn uses ICMP in order to test for a response to a ping.  If a machine (or firewall) prevents ICMP requests, then the machine will return as offline. This is considered a FALSE NEGATIVE.&#x20;

In order to prove a false negative you would send further protocol communications to a host to see if it is alive. Such as TCP or UDP requests.  When performing an NMAP scan. Use -PN in order to tell nmap NOT to ping the host first. This will force NMAP to continue sending all its requests assuming the host is live even if ICMP says it is not.&#x20;

```
nmap -Pn -iL IPlist.txt
```

##
