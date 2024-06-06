# Proxy & Proxy Chains

## Proxychains

Sometimes we need to remain untraceable while performing a pentest activity. Proxychains helps us by allowing us to use an intermediary system whose IP can be left in the logs of the system without the worry of it tracing back to us.

Proxychains is a tool that allows any application to follow connection via proxy such as SOCKS5, Tor, and so on.

```
vim /etc/proxychains.conf
```

We can add all the proxies we want in the preceding highlighted area and then save.

![](<../.gitbook/assets/image (6).png>)

uncomment “strict\_chain”

Create your tunnel:

```
ssh -D 1234 user@host
```

example proxy chains:

socks4 127.0.0.1 9050\
socks5 127.0.0.1 9050\
\*\*\*\*socks5 127.0.0.1 1234

## Socat

Socat (short for "socket cat") is a command-line based utility that establishes two bidirectional byte streams and transfers data between them. It is a versatile networking tool that can be used for a wide variety of purposes including port forwarding, creating network tunnels, transferring files, and more.

Suppose you need to access a web service running on `127.0.0.1:8000` on a remote host. Direct access isn't possible since the service is bound to the localhost interface of the remote machine. However, using `socat`, you can forward this remote port to your local machine.

The following `socat` command listens on port `8000` on your local machine and forwards all traffic to port `8000` on the remote host `192.168.235.62`

{% code overflow="wrap" %}
```bash
socat TCP-LISTEN:8000,fork TCP:192.168.235.62:8000
```
{% endcode %}
