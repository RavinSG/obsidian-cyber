
Uriel Maimon first described this scan in 1996. In this scan, the **FIN** and ACK **bits** are set. The target should send an RST packet as a response. However, **certain BSD-derived systems** *drop the packet if it is an open port* exposing the open ports. 

This scan *won’t work* on most targets encountered *in modern networks*; however, it is included for better understand the port scanning mechanism and the hacking mindset. To select this scan type, use the `-sM` option.

Most target systems *respond with an RST packet regardless* of whether the TCP port is *open*. In such a case, we won’t be able to discover the open ports. The figure below shows the expected behaviour in the cases of both open and closed TCP ports.

![[Maimon Scan.png]]

The console output below is an example of a TCP Maimon scan against a Linux server. As mentioned, because open ports and closed ports are behaving the same way, the Maimon scan *could not discover any open ports* on the target system.

```bash
sudo nmap -sM 10.10.38.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-02 13:30 AEDT
Nmap scan report for 10.10.38.1
Host is up (0.31s latency).
All 1000 scanned ports on 10.10.38.1 are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap done: 1 IP address (1 host up) scanned in 3.53 seconds
```

This type of scan is *not the first scan* one would pick to discover a system; however, it is important to know about it as you don’t know when it could come in handy.