---
description: >-
  Service Principal Names (SPNs) are a unique identifier tied to each instance
  of a Windows service that can delegate to authenticate using Kerberos
---

# service principle names (SPNs)

They allow a service running on a server to authenticate to client computers on a network. These are used primarily in Active Directory (AD) environments.

An SPN is tied to the logon account of the service, meaning it's possible for a user account to have multiple SPNs if it's running multiple services. If a service or user account has an SPN, then it's possible for that service or account to request a Kerberos ticket. This is where the potential for abuse can occur, and it's the basis for what's known as a Kerberoasting attack.

## Kerberoasting

Kerberoasting is a method where an attacker, even one without a high privilege level, can request Kerberos tickets for any user that has an SPN associated with their account. Since these tickets are encrypted with the user's password, they can then be taken offline and subjected to a brute force attack.

#### Testing for SPNs

To test for SPNs, you can use Impacket's GetUserSPNs tool. The tool allows you to request and export SPNs associated with a user account for offline cracking.&#x20;

```bash
impacket-GetUserSPNs -dc-ip 10.199.101.5 <domain>.local/<user>
impacket-GetUserSPNs -request -dc-ip 192.168.95.11 <domain>.local/<user>
```

Replace `<domain>.local` with the Fully Qualified Domain Name (FQDN) of your target, and `<user>` with the username you're interested in. The `-dc-ip` parameter specifies the IP address of the domain controller you're targeting.

Once you've gathered the SPNs, you can then attempt to crack the passwords offline using a tool like Hashcat. Here's an example of how to use Hashcat for this:

```bash
hashcat -m 13100 hashes.txt /mnt/hgfs/Host_Desktop/ -o cracked.txt
```

In this example, `-m 13100` specifies the mode (in this case, Kerberos 5 TGS-REP etype 23), `hashes.txt` is the file containing the SPN hashes you've exported, and `-o cracked.txt` is the output file for any cracked passwords.
