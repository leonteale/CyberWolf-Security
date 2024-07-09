# Password Cracking

These are a rough guide for how long it takes to do a PURE brute force using all ASCII characters, using a GPU instance that I use for cracking.&#x20;

The speeds will exponentially increase when I add more instances. This is with 2 instances. my buddy is using 16. so I will be looking to increase mine, so all these timeswill half and maybe evenspeed up 4x

<img src="../.gitbook/assets/image (10).png" alt="" data-size="original">

{% embed url="https://vps.cyberwolf-security.co.uk/speeds.html" %}

## Cracking MSCACH / LSA

### Formatting

You might have to format the hashes you get, the below is an example of a tools dumpoing the hashes but they are not the right format for hashcat by default.

<pre><code>DOMAIN.lan/user123:$DCC2$10240#user12345#ea373dca60f838378e952b9cd337c0d3
<strong>DOMAIN.lan/testuser:$DCC2$10240#testuser#0b8378e952b978df5378e952b955eb3d2
</strong></code></pre>

Run the below to format it to hashcat format.&#x20;

{% code overflow="wrap" %}
```bash
cut -d ":" -f 2 hashes.txt
```
{% endcode %}

### Using hashcat:

{% code overflow="wrap" %}
```bash
 .\hashcat.exe -m 2100 .\tocrack\hashes.txt .\rockyou.txt
```
{% endcode %}

example hash format

```
$DCC2$10240#user12345#ea373dca60f838378e952b9cd337c0d3
$DCC2$10240#testuser#0b8378e952b978df5378e952b955eb3d2
```
