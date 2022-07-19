# Converting handshake to .hccapx for Hashcat

Online converter: [https://hashcat.net/cap2hashcat/](https://hashcat.net/cap2hashcat/)

Offline converter using hashcat-utils: [https://github.com/hashcat/hashcat-utils/releases](https://github.com/hashcat/hashcat-utils/releases)

`/usr/lib/hashcat-utils/cap2hccapx.bin handshake.cap output.hccapx`

Crack it using Hashcat

`./hashcat -m 22000 hash.hccapx wordlist.txt`
