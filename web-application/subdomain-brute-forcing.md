# Subdomain brute forcing

### GoBuster

<pre><code>git clone https://github.com/danielmiessler/SecLists.git
<strong>sudo gobuster vhost -u http://horizontall.htb -w ~/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
</strong>

===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) &#x26; Christian Mehlmauer (@firefart)
===============================================================
[+] Url:          http://horizontall.htb
[+] Method:       GET
[+] Threads:      10
[+] Wordlist:     /home/kali/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
[+] User Agent:   gobuster/3.1.0
[+] Timeout:      10s
===============================================================
2022/01/14 08:40:24 Starting gobuster in VHOST enumeration mode
===============================================================
~
~
~
</code></pre>

### WFUZZ

```
wfuzz -w ~/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u http://shibboleth.htb/ -v -c -H "Host:FUZZ.shibboleth.htb" --hw 26
```

## AMASS

```
amass enum -active -d domain.com -brute -src -w /Wordlists/SecLists/Discovery/DNS/deepmagic.com-prefixes-top500.txt -src -ip -dir amass4owasp -o amass.domain.com.out
```
