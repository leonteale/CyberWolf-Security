# Subdomain brute forcing

### GoBuster

<pre><code>git clone https://github.com/danielmiessler/SecLists.git
<strong>sudo gobuster vhost -u http://horizontall.htb -w ~/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
</strong></code></pre>

vhost = virtual hosts running on the same IP. \
DNS = seperate DNS entries that point to other IPs.&#x20;

### WFUZZ

```
wfuzz -w ~/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u http://shibboleth.htb/ -v -c -H "Host:FUZZ.shibboleth.htb" --hw 26
```

## AMASS

```
amass enum -active -d domain.com -brute -src -w /Wordlists/SecLists/Discovery/DNS/deepmagic.com-prefixes-top500.txt -src -ip -dir amass4owasp -o amass.domain.com.out
```

## FFUF

```
ffuf -c -w /Wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://thetoppers.htb -H "HOST:FUZZ.thetoppers.htb"
```

IF you get lots responding/false possitives etc.. then you can filter. such as filter out the size

```
ffuf ........ -fs <size>
```
