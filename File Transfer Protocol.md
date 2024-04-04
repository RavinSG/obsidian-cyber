---
aliases:
  - FTP
tags:
  - Protocol
---
File Transfer Protocol (FTP) was developed to make the transfer of files between different computers with different systems efficient.

### Telnet Login

FTP sends and receives data as **cleartext**; therefore, we can use Telnet (or Netcat) to communicate with an FTP server and act as an FTP client. 

We can login to a FTP server as follow:

```bash
telnet 10.10.48.106 21
Trying 10.10.48.106...
Connected to 10.10.48.106.
Escape character is '^]'.
220 (vsFTPd 3.0.3)
USER frank
331 Please specify the password.
PASS D2xc9CgD
230 Login successful.
```

A command like `STAT` can provide some added information. The `SYST` command shows the System Type of the target (UNIX in this case). `PASV` switches the mode to passive. 

```bash
STAT
211-FTP server status:
     Connected to ::ffff:10.4.72.115
     Logged in as frank
     TYPE: ASCII
     No session bandwidth limit
     Session timeout in seconds is 300
     Control connection is plain text
     Data connections will be plain text
     At session startup, client count was 1
     vsFTPd 3.0.3 - secure, fast, stable
211 End of status
SYST
215 UNIX Type: L8
PASV
227 Entering Passive Mode (10,10,48,106,133,173).
```


It is worth noting that there are two modes for FTP:

- **Active**: In the active mode, the *data is sent over* a separate channel originating from the *FTP server’s port 20*.
- **Passive**: In the passive mode, the *data is sent over* a separate channel originating from an *FTP client’s port above port number 1023*.

The command `TYPE` A switches the *file transfer mode to ASCII*, while `TYPE I` switches the *file transfer mode to binary*. However, we cannot transfer a file using a simple client such as Telnet because **FTP creates a separate connection for file transfer**.

### File Transfer

The image below shows how an actual file transfer would be conducted using FTP. The FTP client will initiate a connection to an FTP server, which listens on port 21 by default. All commands will be sent over the control channel. Once the client requests a file, another TCP connection will be established between them.

![[FTP.png]]


### FTP Client

Considering the sophistication of the data transfer over FTP, let’s use an actual FTP client to download a text file. 

```bash
$ ftp 10.10.48.106  
Connected to 10.10.48.106.
220 (vsFTPd 3.0.3)
Name (10.10.48.106:kali): frank
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||34153|)
150 Here comes the directory listing.
drwx------   10 1001     1001         4096 Sep 15  2021 Maildir
-rw-rw-r--    1 1001     1001         4006 Sep 15  2021 README.txt
-rw-rw-r--    1 1001     1001           39 Sep 15  2021 ftp_flag.thm
226 Directory send OK.
ftp> ascii
200 Switching to ASCII mode.
ftp> get README.txt
local: README.txt remote: README.txt
229 Entering Extended Passive Mode (|||29839|)
150 Opening BINARY mode data connection for README.txt (4006 bytes).
100% |****************************|  4006        1.16 MiB/s    00:00 ETA
226 Transfer complete.
WARNING! 9 bare linefeeds received in ASCII mode.
File may not have transferred correctly.
4006 bytes received in 00:00 (13.56 KiB/s)
```

FTP servers and FTP clients use the FTP protocol. There are various FTP server software that you can select from if you want to host your FTP file server. Examples of FTP server software include:

- vsftpd
- ProFTPD
- uFTP

