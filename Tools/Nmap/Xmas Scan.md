
The Xmas scan gets its name after Christmas tree lights. An Xmas scan sets the **FIN**, **PSH**, and **URG** flags simultaneously. You can select Xmas scan with the option `-sX`.

Like the Null scan and FIN scan, if an *RST packet is received*, it means that the port is *closed*. Otherwise, it will be reported as open|filtered.

The following two figures show the case when the TCP port is open and the case when the TCP port is closed.

![[Xmas Scan open.png]]

![[Xmas Scan closed.png]]

The console output below shows an example of a Xmas scan against a Linux server. The obtained results are pretty similar to that of a null scan and a FIN scan.

```bash
sudo nmap -sX 10.10.38.1         
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

Nmap done: 1 IP address (1 host up) scanned in 14.21 seconds
```
