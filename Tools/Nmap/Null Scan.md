
The null scan *does not set any flag*; all six flag bits are set to zero. You can choose this scan using the `-sN` option. A TCP packet with no flags set will *not trigger any response* when it reaches an open port. Therefore, from Nmap’s perspective, a lack of reply in a null scan indicates that either the port is *open or a firewall is blocking* the packet.

However, we expect the target server to *respond with an RST* packet if the *port is closed*. Consequently, we can use the lack of RST response to figure out the ports that are not closed: open or filtered.

![[Null Scan open.png]]

![[Null Scan closed.png]]

Below is an example of a null scan against a Linux server. The null scan we carried out has successfully identified the seven open ports on the target system. Because the null scan *relies on the lack of a response* to infer that the port is not closed, it cannot indicate with certainty that these ports are *open*; there is a possibility that the ports are *not responding due to a firewall* rule.

```bash
sudo nmap -sN 10.10.38.1         
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-02 13:06 AEDT
Nmap scan report for 10.10.38.1
Host is up (0.29s latency).
Not shown: 993 closed tcp ports (reset)
PORT    STATE         SERVICE
22/tcp  open|filtered ssh
25/tcp  open|filtered smtp
53/tcp  open|filtered domain
80/tcp  open|filtered http
110/tcp open|filtered pop3
111/tcp open|filtered rpcbind
143/tcp open|filtered imap

Nmap done: 1 IP address (1 host up) scanned in 13.26 seconds
```

