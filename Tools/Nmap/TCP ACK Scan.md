
As the name implies, an ACK scan will send a *TCP packet with the ACK flag set*. Use the `-sA` option to choose this scan. As shown in the figure below, the target would *respond to the ACK with RST regardless of the state of the port*. 

This behaviour happens because a TCP packet with the *ACK flag set should be sent only in response to a received TCP packet* to acknowledge the receipt of some data, unlike our case. Hence, this scan *won’t tell us whether the target port is open in a simple setup*.

![[TCP ACK Scan.png]]

This kind of scan **would be helpful if there is a firewall** in front of the target. Consequently, based on which ACK packets resulted in responses, you will learn which ports were *not blocked by the firewall.* In other words, this type of scan is more *suitable to discover firewall rule sets and configuration*.

In the following example, we scanned the target VM before installing a firewall on it. As expected, we couldn’t learn which ports were open.

```bash
pentester@TryHackMe$ sudo nmap -sA 10.10.198.135

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:37 BST
Nmap scan report for 10.10.198.135
Host is up (0.0013s latency).
All 1000 scanned ports on 10.10.198.135 are unfiltered
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.68 seconds
```

After setting up the target VM with a firewall, we repeated the ACK scan. This time, we received some interesting results. As seen in the console output below, we have *three ports that aren't being blocked by the firewall*. This result indicates that the firewall is blocking all other ports except for these three ports.

```bash
pentester@TryHackMe$ sudo nmap -sA 10.10.198.135

Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-07 11:34 BST
Nmap scan report for 10.10.198.135
Host is up (0.00046s latency).
Not shown: 997 filtered ports
PORT    STATE      SERVICE
22/tcp  unfiltered ssh
25/tcp  unfiltered smtp
80/tcp  unfiltered http
MAC Address: 02:78:C0:D0:4E:E9 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 15.45 seconds
```




