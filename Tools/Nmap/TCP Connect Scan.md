
TCP connect scan works by *completing the TCP 3-way handshake*. In standard TCP connection establishment, the client sends a TCP packet with SYN flag set, and the server responds with SYN/ACK if the port is open; finally, the client completes the 3-way handshake by sending an ACK.

![[TCP Connect Scan.png]]

We are interested in learning whether the TCP port is open, not establishing a TCP connection. Hence the **connection is torn as soon as its state is confirmed by sending a RST/ACK**. You can choose to run TCP connect scan using `-sT`.

It is important to note that if you are *not a privileged user* (root or sudoer), a *TCP connect scan is the only possible option* to discover open TCP ports.

An example is shown below:
```bash
$ nmap -sT 10.10.61.190
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-02 10:22 AEDT
Nmap scan report for 10.10.61.190
Host is up (0.32s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
80/tcp  open  http
110/tcp open  pop3
111/tcp open  rpcbind
143/tcp open  imap

Nmap done: 1 IP address (1 host up) scanned in 28.34 seconds
```

