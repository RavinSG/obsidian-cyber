
We can save outputs in mainly 3 formats:

- Normal
- Grepable 
- XML

### Normal

As the name implies, the normal format is similar to the output you get on the screen when scanning a target. You can save your scan in normal format by using `-oN FILENAME;` N stands for normal. Here is an example of the result.

```bash
pentester@TryHackMe$ cat MACHINE_IP_scan.nmap 
# Nmap 7.60 scan initiated Fri Sep 10 05:14:19 2021 as: nmap -sS -sV -O -oN MACHINE_IP_scan MACHINE_IP
Nmap scan report for MACHINE_IP
Host is up (0.00086s latency).
Not shown: 994 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
25/tcp  open  smtp    Postfix smtpd
80/tcp  open  http    nginx 1.6.2
110/tcp open  pop3    Dovecot pop3d
111/tcp open  rpcbind 2-4 (RPC #100000)
143/tcp open  imap    Dovecot imapd
MAC Address: 02:A0:E7:B5:B6:C5 (Unknown)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3.13
OS details: Linux 3.13
Network Distance: 1 hop
Service Info: Host:  debra2.thm.local; OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Sep 10 05:14:28 2021 -- 1 IP address (1 host up) scanned in 9.99 seconds
```

### Grepable

The grepable format has its name from the command `grep`; **grep** stands for *Global Regular Expression Printer*. In simple terms, *it makes filtering the scan output for specific keywords or terms efficient*. 

You can save the scan result in grepable format using `-oG FILENAME`. The scan output, displayed above in normal format, is shown in the console below using grepable format. The normal output is 21 lines; however, the grepable output is only 4 lines. The main reason is that *Nmap wants to make each line meaningful and complete* when the user applies grep. As a result, in grepable output, the lines are so long and are not convenient to read compared to normal output. 

```bash
pentester@TryHackMe$ cat MACHINE_IP_scan.gnmap 
# Nmap 7.60 scan initiated Fri Sep 10 05:14:19 2021 as: nmap -sS -sV -O -oG MACHINE_IP_scan MACHINE_IP
Host: MACHINE_IP	Status: Up
Host: MACHINE_IP	Ports: 22/open/tcp//ssh//OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)/, 25/open/tcp//smtp//Postfix smtpd/, 80/open/tcp//http//nginx 1.6.2/, 110/open/tcp//pop3//Dovecot pop3d/, 111/open/tcp//rpcbind//2-4 (RPC #100000)/, 143/open/tcp//imap//Dovecot imapd/	Ignored State: closed (994)	OS: Linux 3.13	Seq Index: 257	IP ID Seq: All zeros
# Nmap done at Fri Sep 10 05:14:28 2021 -- 1 IP address (1 host up) scanned in 9.99 seconds
```

### XML

The third format is XML. You can save the scan results in XML format using `-oX FILENAME`. The XML format would be most *convenient to process the output in other programs*. Conveniently enough, you can save the scan output in all three formats using `-oA FILENAME` to combine `-oN`, `-oG`, and `-oX` for normal, grepable, and XML.