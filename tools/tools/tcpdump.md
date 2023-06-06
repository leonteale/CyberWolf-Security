---
description: >-
  TCPDump is a powerful command-line packet analyser tool that can be used to
  capture or filter TCP/IP packets that are received or transferred over a
  network on a specific interface.
---

# Tcpdump

### Installing TCPDump

Most Linux distributions include `tcpdump` by default. If it's not installed, you can install it using a package manager. For example, on Ubuntu or Debian, you would use:

```shell
sudo apt-get install tcpdump
```

### Finding Network Interfaces

You can list all network interfaces using the following command:

```shell
ip link show
```

This command will list all network interfaces. The active network interfaces are generally `eth0` for ethernet, `wlan0` for WiFi, and `lo` for localhost.

### Capturing Traffic

Once you've identified the correct network interface, you can use `tcpdump` to capture traffic. The following command captures all traffic on `wlan0` and writes it to `capture.pcap`:

```shell
tcpdump -i wlan0 -w capture.pcap
```

Replace `wlan0` with your network interface.

### Reading PCAP files

You can read the captured packets with `tcpdump` using the `-r` option:

```shell
tcpdump -r capture.pcap
```

### Example TCPDump Commands

Here are some examples of `tcpdump` commands to narrow down your analysis to potentially interesting or sensitive information:

1.  **Monitor HTTP GET requests:**

    ```shell
    tcpdump -A -s 0 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
    ```
2.  **Capture DNS traffic:**

    ```shell
    tcpdump -n 'udp port 53'
    ```
3.  **Capture traffic to/from a specific IP address:**

    ```shell
    tcpdump host 192.168.1.1
    ```
4.  **Capture traffic on a specific port:**

    ```shell
    tcpdump port 80
    ```
5.  **Capture and save traffic to a file:**

    ```shell
    tcpdump -w capture.pcap
    ```
6.  **Capture TCP SYN packets:**

    ```shell
    tcpdump 'tcp[13] & 2!=0'
    ```
7.  **Capture ICMP (ping) packets:**

    ```shell
    tcpdump icmp
    ```
8.  **Capture all HTTP POSTs:**

    ```shell
    tcpdump -A 'tcp port 80 and (tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504f5354)'
    ```
9.  **Capture all packets of length 500:**

    ```shell
    tcpdump 'len=500'
    ```
10. **Capture all packets of a certain length range:**

    ```shell
    tcpdump 'len >= 500 and len <= 1000'
    ```
11. **Capture packets from a specific source port:**

    ```shell
    tcpdump 'src port 80'
    ```
12. **Capture packets from a specific destination port:**

    ```shell
    tcpdump 'dst port 8080'
    ```
13. **Capture all TCP and UDP packets to or from a particular port:**

    ```shell
    tcpdump 'port 53 and (tcp or udp)'
    ```
14. **Capture all TCP packets with the PUSH and ACK flags set:**

    ```shell
    tcpdump 'tcp[13] = 24'
    ```
15. **Capture all UDP packets with a source or destination port of 53 (DNS):**

    ```shell
    tcpdump 'udp port 53'
    ```

This guide provides a basic introduction to using `tcpdump` to capture and analyse network traffic. Remember to perform these actions responsibly and only on networks and devices where you have permission.
