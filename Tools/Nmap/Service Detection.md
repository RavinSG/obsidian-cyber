
Once Nmap discovers open ports, we can probe the available port to detect the running service. Further investigation of open ports is an essential piece of information as the pentester can use it to learn if there are any known vulnerabilities of the service.

Adding `-sV` to our Nmap command will collect and determine service and version information for the open ports. We can control the intensity with `--version-intensity LEVEL` where the level ranges between **0**, the **lightest**, and **9**, the **most complete**. `-sV --version-light` has an intensity of `2`, while `-sV --version-all` has an intensity of `9`.

It is important to note that using `-sV` will *force Nmap to proceed with the TCP 3-way handshake* and establish the connection. The connection establishment is necessary because Nmap *cannot discover the version without establishing a connection* fully and communicating with the listening service. 

The console output below shows a simple Nmap stealth SYN scan with the -sV option. Adding the -sV option leads to a new column in the output showing the version for each detected service.

```bash
sudo nmap -sV 10.10.191.190         
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-03 12:07 AEDT
Nmap scan report for 10.10.191.190
Host is up (0.31s latency).
Not shown: 994 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
25/tcp  open  smtp    Postfix smtpd
80/tcp  open  http    nginx 1.6.2
110/tcp open  pop3    Dovecot pop3d
111/tcp open  rpcbind 2-4 (RPC #100000)
143/tcp open  imap    Dovecot imapd
Service Info: Host:  debra2.thm.local; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.68 seconds
```

For instance, in the case of TCP port 22 being open, instead of `22/tcp open ssh`, we obtain `22/tcp open ssh OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)`. Notice that the SSH protocol is *guessed as the service* because TCP port 22 is open; Nmap didn’t need to connect to port 22 to confirm. However, `-sV` required connecting to this open port to grab the service banner and any version information it can get, such as `nginx 1.6.2`. Hence, unlike the service column, *the version column is not a guess*.