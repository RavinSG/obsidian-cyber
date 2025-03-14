
## TCP SYN Ping

We can send a **packet with the SYN** (Synchronize) flag set to a TCP port, 80 by default, and wait for a response. An *open port should reply with a SYN/ACK* (Acknowledge); a *closed port would result in an RST* (Reset). In this case, we only check whether we will get any response to infer whether the host is up. 

If we want Nmap to use *TCP SYN ping*, we can do so via the option `-PS` followed by the port number, range, list, or a combination of them. For example, `-PS21` will target `port 21`, while` -PS21-25` will target `ports 21, 22, 23, 24, and 25`. Finally `-PS80,443,8080` will target the three `ports 80, 443, and 8080`

**Privileged users** (root and sudoers) can send TCP SYN packets and *don’t need to complete the TCP 3-way handshake* even if the port is open as shown below. **Unprivileged users** have no choice but to *complete the 3-way handshake* if the port is open.

![[TCP SYN RST.jpeg]]

## TCP ACK Ping

This sends a packet with an **ACK flag set**. You must be running Nmap as a *privileged user* to be able to accomplish this. If you try it as an unprivileged user, Nmap will attempt a 3-way handshake.

By *default, port 80* is used. The syntax is similar to TCP SYN ping. `-PA` should be followed by a port number, range, list, or a combination of them. For example, consider `-PA21`, `-PA21-25` and `-PA80,443,8080`. If no port is specified, port 80 will be used.

The *target responds with the RST* flag set because the TCP packet with the ACK flag is not part of any ongoing connection. 

## UDP Ping

Contrary to TCP SYN ping, sending a UDP packet to an *open port* is **not expected to lead to any reply**. However, if we send a UDP packet to a *closed UDP port*, we **expect to get an ICMP port unreachable** packet; this indicates that the target system is up and available.

![[UPD Ping.png]]

The syntax to specify the ports is similar to that of TCP SYN ping and TCP ACK ping; Nmap uses `-PU` for UDP ping.