
The FIN scan sends a TCP packet with the *FIN flag set*. You can choose this scan type using the `-sF` option. *No response* will be sent if the TCP port is *open*. Again, Nmap cannot be sure if the port is open or if a firewall is blocking the traffic related to this TCP port.

However, the target system should *respond with an RST* if the port is *closed*. Consequently, we will be able to know which ports are closed and use this knowledge to infer the ports that are open or filtered. It's worth noting some firewalls will 'silently' drop the traffic without sending an RST.

![[FIN Scan open.png]]

![[FIN Scan closed.png]]

Below is an example of a FIN scan against a Linux server. The result is quite similar to a result obtained using a null scan.

```bash
sudo nmap -sF 10.10.38.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-02 13:09 AEDT
Nmap scan report for 10.10.38.1
Host is up (0.31s latency).
Not shown: 993 closed tcp ports (reset)
PORT    STATE         SERVICE
22/tcp  open|filtered ssh
25/tcp  open|filtered smtp
53/tcp  open|filtered domain
80/tcp  open|filtered http
110/tcp open|filtered pop3
111/tcp open|filtered rpcbind
143/tcp open|filtered imap

Nmap done: 1 IP address (1 host up) scanned in 25.12 seconds
```

