# Subdomain brute forcing

<figure><img src="../.gitbook/assets/Fou_zZuWIAAqH4-.jfif" alt=""><figcaption></figcaption></figure>

## [https://pentester.land/blog/subdomains-enumeration-cheatsheet/](https://pentester.land/blog/subdomains-enumeration-cheatsheet/)



## domained

[https://github.com/TypeError/domained](https://github.com/TypeError/domained)

### Subfinder

A subdomain discovery tool that discovers valid subdomains for websites. Designed as a passive framework to be useful for bug bounties and safe for penetration testing. More information: https://github.com/subfinder/subfinder.

{% code overflow="wrap" %}
```bash
 ### Find subdomains for a specific domain:
   subfinder -d example.com

 ### Show only the subdomains found:
   subfinder --silent -d example.com

### Use a brute-force attack to find subdomains:
   subfinder -d example.com -b

### Remove wildcard subdomains:
   subfinder -nW -d example.com

### Use a given comma-separated list of resolvers:
   subfinder -r 8.8.8.8,1.1.1.1 -d example.com
```
{% endcode %}

### GoBuster

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash">git clone https://github.com/danielmiessler/SecLists.git
<strong>sudo gobuster vhost -u http://horizontall.htb -w ~/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
</strong><strong>
</strong>gobuster vhost -u http://boardlight.htb -w /usr/share/wordlists/seclists/Discovery/DNS/combined_subdomains.txt --append-domain
</code></pre>

vhost = virtual hosts running on the same IP. \
DNS = seperate DNS entries that point to other IPs.&#x20;

### WFUZZ

{% code overflow="wrap" %}
```bash
wfuzz -w ~/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u http://shibboleth.htb/ -v -c -H "Host:FUZZ.shibboleth.htb" --hw 26
```
{% endcode %}

## AMASS

{% code overflow="wrap" %}
```bash
amass enum -active -d domain.com -brute -src -w /Wordlists/SecLists/Discovery/DNS/deepmagic.com-prefixes-top500.txt -src -ip -dir amass4owasp -o amass.domain.com.out
```
{% endcode %}

## FFUF

{% code overflow="wrap" %}
```bash
ffuf -c -w /Wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://thetoppers.htb -H "HOST:FUZZ.thetoppers.htb"
```
{% endcode %}

IF you get lots responding/false possitives etc.. then you can filter. such as filter out the size

```
ffuf ........ -fs <size>
```
