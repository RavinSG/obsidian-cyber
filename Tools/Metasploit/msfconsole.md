
While using the Metasploit Framework, you will primarily interact with the Metasploit console. You can launch it from the terminal using the `msfconsole` command. 

```bash
msfconsole 
Metasploit tip: Search can apply complex filters such as search cve:2009 
type:exploit, see all the filters with help search
                                                  
IIIIII    dTb.dTb        _.---._
  II     4'  v  'B   .'"".'/|\`.""'.
  II     6.     .P  :  .' / | \ `.  :
  II     'T;. .;P'  '.'  /  |  \  `.'
  II      'T; ;P'    `. /   |   \ .'
IIIIII     'YvP'       `-.__|__.-'

I love shells --egypt


       =[ metasploit v6.3.55-dev                          ]
+ -- --=[ 2397 exploits - 1235 auxiliary - 422 post       ]
+ -- --=[ 1391 payloads - 46 encoders - 11 nops           ]
+ -- --=[ 9 evasion                                       ]
```

Once launched, you will see the command line changes to msf6. The Metasploit console (msfconsole) can be used just like a regular command-line shell, as you can see below.

```bash
msf6 > ls
[*] exec: ls

47138.py  47887.py  50477.py  initial
msf6 > ping -c 1 8.8.8.8
[*] exec: ping -c 1 8.8.8.8

PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=128 time=73.9 ms

--- 8.8.8.8 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 73.943/73.943/73.943/0.000 ms
```

It will *support most Linux commands*, including `clear` (to clear the terminal screen), but will not allow you to use some features of a regular command line.

The `help` command can be used on its own or for a specific command. Below is the help menu for the set command.

```bash
msf6 > help set
Usage: set [options] [name] [value]

Set the given option to value.  If value is omitted, print the current value.
If both are omitted, print options that are currently set.

If run from a module context, this will set the value in the module's
datastore.  Use -g to operate on the global datastore.

If setting a PAYLOAD, this command can take an index from `show payloads'.

OPTIONS:

    -c, --clear   Clear the values, explicitly setting to nil (default)
    -g, --global  Operate on global datastore variables
    -h, --help    Help banner.
```

## Context

Msfconsole is managed by context; this means that unless set as a global variable, all *parameter settings will be lost if you change the module* you have decided to use. In the example below, we have used the `ms17_010_eternalblue` exploit, and we have set parameters such as `RHOSTS`. 

If we were to switch to another module (e.g. a port scanner), we would need to set the `RHOSTS` value again as all *changes we have made remained in the context of the* `ms17_010_eternalblue` exploit. 

```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue 
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) >
```

Once you type the `use exploit/windows/smb/ms17_010_eternalblue` command, you will see the command line prompt change from `msf6` to 
`msf6 exploit(windows/smb/ms17_010_eternalblue)`. 

*The prompt tells us* we now have a *context set* in which we will work. You can see this by typing the `show options` command.

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options 

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use for authentication. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.139.128  yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target



View the full module info with the info, or info -d command.
```

This will print options related to the exploit we have chosen earlier. The `show options` command will have *different outputs depending on the context* it is used in. The example above shows that this exploit will require we set variables like `RHOSTS` and `RPORT`. 

On the other hand, a post-exploitation module may only need us to set a `SESSION ID`. A session is an existing connection to the target system that the post-exploitation module will use.

```bash
msf6 post(windows/gather/enum_domain_users) > show options 

Module options (post/windows/gather/enum_domain_users):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   HOST                      no        Target a specific host
   SESSION                   yes       The session to run this module on
   USER                      no        Target User for NetSessionEnum


View the full module info with the info, or info -d command.
```

The `show` command can be used in any context followed by a module type (*auxiliary*, *payload*, *exploit*, etc.) to list available modules. The example below lists payloads that can be used with the `ms17-010 Eternalblue exploit`.

![[Show Payload.png]]

You can leave the context using the `back` command. 

Further information on any module can be obtained by typing the `info` command within its context.

## Search

One of the most useful commands in msfconsole is [[Search]]

## Database

Metasploit has a [[Database]] function to simplify project management and avoid possible confusion when setting up parameter values. 

## Vulnerability Scanning

Metasploit allows you to quickly identify some critical vulnerabilities that could be considered as “*low hanging fruit*”.  The term “low hanging fruit” usually refers to easily identifiable and exploitable vulnerabilities that could *potentially* allow you to *gain* a *foothold on a system* and, in some cases, gain *high-level privileges* such as root or administrator.