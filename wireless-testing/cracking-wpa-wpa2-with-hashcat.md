# Cracking WPA/WPA2 hashes

## Handshake .cap

To crack WPA/WPA2 handshakes from a .cap file using Hashcat, you can use the following command:

```bash
hashcat -m 2500 (or 22000) capture.hccapx wordlist.txt
```

For this, you need to convert the .cap file to the .hccapx format first, which can be done using tools like `cap2hccapx`.

For John the Ripper, the command would look like:

```bash
john --wordlist=wordlist.txt --format=wpapsk capture.hccapx
```

John the Ripper also requires the handshake to be in a specific format, often a converted .hccapx or directly in John's own 'netntlm' format.



{% code overflow="wrap" %}
```bash
aircrack-ng -a 2 -w /usr/share/dict/wordlist-probable.txt --bssid CC:D0:83:70:22:E2 -l /tmp/wifitejrkm_0dh/wpakey.txt hs/handshake_wifi13-43-05.cap
```
{% endcode %}

## PMKID hash

To crack a PMKID hash, you can use the following command for Hashcat:

```bash
hashcat -m 16800 pmkid_hash.txt wordlist.txt
```

Here, `-m 16800` specifies the hash type for PMKID, `pmkid_hash.txt` is the file containing the captured PMKID hash, and `wordlist.txt` is your wordlist.

For John the Ripper, you'd first convert the PMKID hash into a format John can understand using `hcxpcaptool` or a similar conversion utility. Then, you can crack it using:

```bash
john --format=wpapsk pmkid_john_hash.txt
```

Here, `--format=wpapsk` specifies the hash type, and `pmkid_john_hash.txt` would be the John-readable hash.
