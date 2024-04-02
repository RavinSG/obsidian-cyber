
The TCP window scan is *almost the same as the ACK scan*; however, it **examines the TCP Window field** of the RST packets returned. On specific systems, this can reveal that the port is open. You can select this scan type with the option `-sW`. As shown in the figure below, we expect to get an RST packet in reply to our “uninvited” ACK packets, regardless of whether the port is open or closed.

![[Window Scan.png]]

Similarly, launching a TCP window scan against a Linux *system with no firewall will not provide much information*. The results of the window scan against a Linux server with no firewall won't give any extra information compared to the ACK scan executed earlier.

However, as you would expect, if we repeat our TCP window scan against a server behind a firewall, we expect to get more satisfying results. In the console output shown below, the *TCP window scan pointed that three ports are detected as closed*. (This is in **contrast with the ACK scan** that labelled the same three ports as unfiltered.) Although we know that these three ports are not closed, we realise they responded differently, indicating that the firewall does not block them.

```bash
pentester@TryHackMe$ sudo nmap -sW 10.10.198.135

Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-07 11:39 BST
Nmap scan report for 10.10.198.135
Host is up (0.00040s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE
22/tcp  closed ssh
25/tcp  closed smtp
80/tcp  closed http
MAC Address: 02:78:C0:D0:4E:E9 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 14.84 seconds
```

On some systems, open ports use a positive window size (even for RST packets) while closed ones have a zero window. Window scan sends the same bare ACK probe as ACK scan, interpreting the results as shown below:

| Probe Response                                              | Assigned State |
| ----------------------------------------------------------- | -------------- |
| TCP RST response with non-zero window field                 | `open`         |
| TCP RST response with zero window field                     | `closed`       |
| No response received (even after retransmissions)           | `filtered`     |
| ICMP unreachable error (type 3, code 1, 2, 3, 9, 10, or 13) | `filtered`     |
This scan relies on an implementation detail of a *minority of systems* out on the Internet, so you can't always trust it. Systems that don't support it will **usually return all ports closed**.

