
## System Enumeration

One of the basic commands we can use is `systeminfo`. This will dump out all the basic information we need to know about the machine. Such as, OS version, Architecture, System Model, etc.

```PowerShell
PS C:\Users\fcastle> systeminfo

Host Name:                 THEPUNISHER
OS Name:                   Microsoft Windows 10 Enterprise Evaluation
OS Version:                10.0.19045 N/A Build 19045
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Member Workstation
OS Build Type:             Multiprocessor Free
Registered Owner:          frankcastle
Registered Organization:
Product ID:                00329-20000-00001-AA517
Original Install Date:     13/02/2025, 12:26:50 PM
System Boot Time:          17/06/2025, 6:59:28 PM
System Manufacturer:       VMware, Inc.
System Model:              VMware20,1
System Type:               x64-based PC
.
.
PS C:\Users\fcastle> systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
OS Name:                   Microsoft Windows 10 Enterprise Evaluation
OS Version:                10.0.19045 N/A Build 19045
System Type:               x64-based PC
```

We can also check for installed patches using `wmic` (Windows Management Instrumentation). The `qfe` (Quick Fix Engineering) command will list down the following information. 

```Powershell
PS C:\Users\fcastle> wmic qfe
Caption                                     CSName       Description      FixComments  HotFixID   InstallDate  InstalledBy          InstalledOn  Name  ServicePackInEffect  Status
http://support.microsoft.com/?kbid=5050576  THEPUNISHER  Update                        KB5050576               NT AUTHORITY\SYSTEM  3/4/2025
http://support.microsoft.com/?kbid=5049613  THEPUNISHER  Update                        KB5049613               NT AUTHORITY\SYSTEM  2/13/2025
http://support.microsoft.com/?kbid=5011048  THEPUNISHER  Update                        KB5011048               NT AUTHORITY\SYSTEM  2/13/2025
https://support.microsoft.com/help/5015684  THEPUNISHER  Update                        KB5015684                                    9/8/2022
https://support.microsoft.com/help/5026037  THEPUNISHER  Update                        KB5026037               NT AUTHORITY\SYSTEM  2/13/2025
https://support.microsoft.com/help/5053606  THEPUNISHER  Security Update               KB5053606               NT AUTHORITY\SYSTEM  3/24/2025
```

If we need filtered results, we can use the `get` option and list down the columns we want.

```PowerShell
PS C:\Users\fcastle> wmic qfe get Caption,Description,HotFixID,InstalledOn
Caption                                     Description      HotFixID   InstalledOn
http://support.microsoft.com/?kbid=5050576  Update           KB5050576  3/4/2025
http://support.microsoft.com/?kbid=5049613  Update           KB5049613  2/13/2025
http://support.microsoft.com/?kbid=5011048  Update           KB5011048  2/13/2025
https://support.microsoft.com/help/5015684  Update           KB5015684  9/8/2022
https://support.microsoft.com/help/5026037  Update           KB5026037  2/13/2025
https://support.microsoft.com/help/5053606  Security Update  KB5053606  3/24/2025
```

We can also list down the drives as follow.

```PowerShell
PS C:\Users\fcastle> wmic logicaldisk get Caption,Description,ProviderName
Caption  Description       ProviderName
C:       Local Fixed Disk
D:       CD-ROM Disc
```


## User Enumeration

The basic command we can use to find the user we have compromised to gain access to a machine, is by issuing the `whoami` command.

```
c:\windows\system32\inetsrv>whoami
iis apppool\web
```

Next, we can look at what privileges this user has.

```
c:\windows\system32\inetsrv>whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeShutdownPrivilege           Shut down the system                      Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeUndockPrivilege             Remove computer from docking station      Disabled
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
SeTimeZonePrivilege           Change the time zone                      Disabled
```

From a glance we can see that *impersonation is enabled* for this user. Which means we might be able to perform a [[Token Impersonation]] attack later on.

We can further look in to which groups this user belongs to. Even though the user might not be an Administrator, they **might be part of a group** that has some **administrative privileges**.

```
c:\windows\system32\inetsrv>whoami /groups

GROUP INFORMATION
-----------------

Group Name                           Type             SID          Attributes                                        
==================================== ================ ============ ==================================================
Mandatory Label\High Mandatory Level Label            S-1-16-12288                                                   
Everyone                             Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                        Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\SERVICE                 Well-known group S-1-5-6      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                        Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users     Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization       Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
BUILTIN\IIS_IUSRS                    Alias            S-1-5-32-568 Mandatory group, Enabled by default, Enabled group
LOCAL                                Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
                                     Unknown SID type S-1-5-82-0   Mandatory group, Enabled by default, Enabled group
```

Furthermore, we can see which users have accounts in the machine we are connected to with the `net user` command and find out more details about these users.

```
c:\windows\system32\inetsrv>net user

User accounts for \\

-------------------------------------------------------------------------------
Administrator            babis                    Guest
```

For example, the user `babis` is not part of the administrator group. Hence, we might not be able to get admin privileges even if we compromise this user. However, they *might have access to a password file or some other resource* that could help us to escalate our privileges.

```
c:\windows\system32\inetsrv>net user babis
User name                    babis
Full Name                    
Comment                      
User's comment               
Country code                 000 (System Default)
Account active               Yes
Account expires              Never

Password last set            18/3/2017 2:15:19 ��
Password expires             Never
Password changeable          18/3/2017 2:15:19 ��
Password required            No
User may change password     Yes

Workstations allowed         All
Logon script                 
User profile                 
Home directory               
Last logon                   18/3/2017 2:17:50 ��

Logon hours allowed          All

Local Group Memberships      *Users                
Global Group memberships     *None                 
The command completed successfully.
```

Finally, we can look into `localgroups` to see if there are any logon sessions available. Or, we can find out specifically which users belong to a group such as the administrators.

```
c:\windows\system32\inetsrv>net localgroup
System error 1312 has occurred.

A specified logon session does not exist. It may already have been terminated.


c:\windows\system32\inetsrv>net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
The command completed successfully.
```

and we confirm that **only the Administrator is part of the administrator group**.

## Network Enumeration

Once we have access to a machine, we can obtain basic information about the network the machine is connected to with the `ipconfig` command.

```
c:\windows\system32\inetsrv>ipconfig /all

Windows IP Configuration

   Host Name . . . . . . . . . . . . : devel
   Primary Dns Suffix  . . . . . . . : 
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No

Ethernet adapter Local Area Connection 4:

   Connection-specific DNS Suffix  . : 
   Description . . . . . . . . . . . : Intel(R) PRO/1000 MT Network Connection
   Physical Address. . . . . . . . . : 00-50-56-95-62-90
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   IPv6 Address. . . . . . . . . . . : dead:beef::f089:4669:1c10:c58f(Preferred) 
   Temporary IPv6 Address. . . . . . : dead:beef::8d58:2e12:2d9:eadd(Preferred) 
   Link-local IPv6 Address . . . . . : fe80::f089:4669:1c10:c58f%15(Preferred) 
   IPv4 Address. . . . . . . . . . . : 10.10.10.5(Preferred) 
   Subnet Mask . . . . . . . . . . . : 255.255.254.0
   Default Gateway . . . . . . . . . : fe80::250:56ff:feb9:6def%15
                                       10.10.10.2
   DNS Servers . . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
   NetBIOS over Tcpip. . . . . . . . : Enabled

Tunnel adapter isatap.{0B2931D6-69F8-4A00-8E64-237C531D469C}:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . : 
   Description . . . . . . . . . . . : Microsoft ISATAP Adapter
   Physical Address. . . . . . . . . : 00-00-00-00-00-00-00-E0
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes

```

We can gain some information about the network architecture such as the network we belong to, **default gateway**, and the **DNS servers** (the DC if there is one).

The next place we can look at is the ARP table. If we see any other IP addresses that belong to our network in addition to the above ones, it might be another machine that is communicating with us. Which might be a good place we can look into.

```
c:\windows\system32\inetsrv>arp -a

Interface: 10.10.10.5 --- 0xf
  Internet Address      Physical Address      Type
  10.10.10.2            00-50-56-b9-6d-ef     dynamic   
  10.10.11.255          ff-ff-ff-ff-ff-ff     static    
  224.0.0.22            01-00-5e-00-00-16     static    
  224.0.0.252           01-00-5e-00-00-fc     static 
```

Another place we can check is the routing table. This is another place we can look into see whether there are any other machines from our network that is communicating with us.

```
c:\windows\system32\inetsrv>route print
===========================================================================
Interface List
 15...00 50 56 95 62 90 ......Intel(R) PRO/1000 MT Network Connection
  1...........................Software Loopback Interface 1
 14...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter
===========================================================================

IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0       10.10.10.2       10.10.10.5    266
       10.10.10.0    255.255.254.0         On-link        10.10.10.5    266
       10.10.10.5  255.255.255.255         On-link        10.10.10.5    266
     10.10.11.255  255.255.255.255         On-link        10.10.10.5    266
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
        224.0.0.0        240.0.0.0         On-link        10.10.10.5    266
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
  255.255.255.255  255.255.255.255         On-link        10.10.10.5    266
===========================================================================
Persistent Routes:
  Network Address          Netmask  Gateway Address  Metric
          0.0.0.0          0.0.0.0       10.10.10.2  Default 
          0.0.0.0          0.0.0.0       10.10.10.2  Default 
          0.0.0.0          0.0.0.0       10.10.10.2  Default 
          0.0.0.0          0.0.0.0       10.10.10.2  Default 
===========================================================================
```


The `netstat` command will list all network connections present in our machine and we can use the `-ano` flags to get a list of processes.

- `-a`: Show all connections and listening ports. This includes TCP and UDP connections and any ports that are open and waiting for connections.
- `-n`: Show addresses and port numbers numerically instead of resolving them to hostnames or service names.
- `-o`: Show the owning process ID (PID) for each connection.

```
c:\windows\system32\inetsrv>netstat -ano
netstat -ano

Active Connections

  Proto  Local Address          Foreign Address        State           PID
  TCP    0.0.0.0:21             0.0.0.0:0              LISTENING       1380
  TCP    0.0.0.0:80             0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       688
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:5357           0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:49152          0.0.0.0:0              LISTENING       376
  TCP    0.0.0.0:49153          0.0.0.0:0              LISTENING       768
  TCP    0.0.0.0:49154          0.0.0.0:0              LISTENING       872
  TCP    0.0.0.0:49155          0.0.0.0:0              LISTENING       476
  TCP    0.0.0.0:49156          0.0.0.0:0              LISTENING       492
  TCP    10.10.10.5:139         0.0.0.0:0              LISTENING       4
  TCP    10.10.10.5:49177       10.10.14.19:6666       ESTABLISHED     2368
  TCP    [::]:21                [::]:0                 LISTENING       1380
  TCP    [::]:80                [::]:0                 LISTENING       4
  TCP    [::]:135               [::]:0                 LISTENING       688
  TCP    [::]:445               [::]:0                 LISTENING       4
  TCP    [::]:5357              [::]:0                 LISTENING       4
  TCP    [::]:49152             [::]:0                 LISTENING       376
  TCP    [::]:49153             [::]:0                 LISTENING       768
  TCP    [::]:49154             [::]:0                 LISTENING       872
  TCP    [::]:49155             [::]:0                 LISTENING       476
  TCP    [::]:49156             [::]:0                 LISTENING       492
  UDP    0.0.0.0:123            *:*                                    972
  UDP    0.0.0.0:3702           *:*                                    1352
  UDP    0.0.0.0:3702           *:*                                    1352
  UDP    0.0.0.0:5355           *:*                                    1092
  UDP    0.0.0.0:49942          *:*                                    1352
  UDP    10.10.10.5:137         *:*                                    4
  UDP    10.10.10.5:138         *:*                                    4
  UDP    10.10.10.5:1900        *:*                                    1352
  UDP    127.0.0.1:1900         *:*                                    1352
  UDP    127.0.0.1:60495        *:*                                    1352
  UDP    [::]:123               *:*                                    972
  UDP    [::]:3702              *:*                                    1352
  UDP    [::]:3702              *:*                                    1352
  UDP    [::]:5355              *:*                                    1092
  UDP    [::]:49943             *:*                                    1352
  UDP    [::1]:1900             *:*                                    1352
  UDP    [::1]:60494            *:*                                    1352
  UDP    [fe80::f089:4669:1c10:c58f%15]:1900  *:*                      1352
```

From the above output we can see that there are *many ports* which are listening that we *did not pick up in out initial nmap scan*. This might be due to these ports only being listening in the local network.

We can use a tool like **plink** or **meterpreter** to perform port forwarding to access these internal services.

## Password Hunting

Here are some commands that we can use to search for cleartext passwords stored in a machine. Read more [here](https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/windows-privilege-escalation/#eop-looting-for-passwords) for more details.

```
c:\Windows\System32>findstr /si password *.txt *.ini *.config

WindowsPowerShell\v1.0\en-US\about_hash_tables.help.txt:          Msg1="Please enter your password."
WindowsPowerShell\v1.0\en-US\about_hash_tables.help.txt:          Msg1="Please enter your password.";
WindowsPowerShell\v1.0\en-US\about_hash_tables.help.txt:          >> Msg1="Please enter your password.";
WindowsPowerShell\v1.0\en-US\about_hash_tables.help.txt:          Msg1                    Please enter your password.
WindowsPowerShell\v1.0\en-US\about_hash_tables.help.txt:          Please enter your password.
WindowsPowerShell\v1.0\en-US\about_hash_tables.help.txt:          Msg1                           Please enter your password.
WindowsPowerShell\v1.0\en-US\about_hash_tables.help.txt:          Msg1                           Please enter your password.
WindowsPowerShell\v1.0\en-US\about_hash_tables.help.txt:          Msg1="Please enter your password."
WindowsPowerShell\v1.0\en-US\about_hash_tables.help.txt:        Msg1                           "Please enter your password."
WindowsPowerShell\v1.0\en-US\about_remote_FAQ.help.txt:    name and password credentials on the local computer or the credentials
WindowsPowerShell\v1.0\en-US\about_remote_troubleshooting.help.txt:    2. Verify that a password is set on the workgroup-based computer. If a
WindowsPowerShell\v1.0\en-US\about_remote_troubleshooting.help.txt:       password is not set or the password value is empty, you cannot run
WindowsPowerShell\v1.0\en-US\about_remote_troubleshooting.help.txt:       To set password for your user account, use User Accounts in Control
WindowsPowerShell\v1.0\en-US\about_Return.help.txt:          function ScreenPassword($instance)
WindowsPowerShell\v1.0\en-US\about_Return.help.txt:          foreach ($a in @(get-wmiobject win32_desktop)) { ScreenPassword($a) }
WindowsPowerShell\v1.0\en-US\about_Return.help.txt:      This script checks each user account. The ScreenPassword function returns 
WindowsPowerShell\v1.0\en-US\about_Return.help.txt:      the name of any user account that does not have a password-protected 
WindowsPowerShell\v1.0\en-US\about_Return.help.txt:      screen saver. If the screen saver is password protected, the function 
WindowsPowerShell\v1.0\en-US\about_Signing.help.txt:    The MakeCert.exe tool will prompt you for a private key password. The 
WindowsPowerShell\v1.0\en-US\about_Signing.help.txt:    password ensures that no one can use or access the certificate without
WindowsPowerShell\v1.0\en-US\about_Signing.help.txt:    your consent. Create and enter a password that you can remember. You will 
WindowsPowerShell\v1.0\en-US\about_Signing.help.txt:    use this password later to retrieve the certificate.
WindowsPowerShell\v1.0\en-US\about_Signing.help.txt:        6. Type a password, and then type it again to confirm.
WindowsPowerShell\v1.0\en-US\about_Signing.help.txt:        4. On the Password page, select "Enable strong private key protection",
WindowsPowerShell\v1.0\en-US\about_Signing.help.txt:           and then enter the password that you assigned during the export 
FINDSTR: Cannot open restore\MachineGuid.txt
```


Also, the following are a set of files that we can look into check whether there are any passwords stored.

```
c:\sysprep.inf
c:\sysprep\sysprep.xml
c:\unattend.xml
%WINDIR%\Panther\Unattend\Unattended.xml
%WINDIR%\Panther\Unattended.xml

dir c:\*vnc.ini /s /b
dir c:\*ultravnc.ini /s /b 
dir c:\ /s /b | findstr /si *vnc.ini
```


## AV Enumeration

 It's always good to check which anti virus software is running on the machine to get an idea on which attacks might work and might not. The first thing we can check is Windows Defender, and we can use the **service control** command to query the state of the service.

```
c:\windows\system32\inetsrv>sc query windefend

SERVICE_NAME: windefend 
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

If we want to list all services running on the machine, we can run the following command.

```
c:\windows\system32\inetsrv>sc queryex type= service

SERVICE_NAME: AppHostSvc
DISPLAY_NAME: Application Host Helper Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
        PID                : 1316
        FLAGS              : 

SERVICE_NAME: AudioEndpointBuilder
DISPLAY_NAME: Windows Audio Endpoint Builder
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
        PID                : 824
        FLAGS              : 
.
.
.
```

To obtain firewall information we can use either one of the following commands.

```
c:\windows\system32\inetsrv>netsh advfirewall firewall dump


c:\windows\system32\inetsrv>netsh firewall show state

Firewall status:
-------------------------------------------------------------------
Profile                           = Standard
Operational mode                  = Enable
Exception mode                    = Enable
Multicast/broadcast response mode = Enable
Notification mode                 = Enable
Group policy version              = Windows Firewall
Remote admin mode                 = Disable

Ports currently open on all network interfaces:
Port   Protocol  Version  Program
-------------------------------------------------------------------
No ports are currently open on all network interfaces.

IMPORTANT: Command executed successfully.
However, "netsh firewall" is deprecated;
use "netsh advfirewall firewall" instead.
For more information on using "netsh advfirewall firewall" commands
instead of "netsh firewall", see KB article 947709
at http://go.microsoft.com/fwlink/?linkid=121488 .

c:\windows\system32\inetsrv>netsh firewall show config

Domain profile configuration:
-------------------------------------------------------------------
Operational mode                  = Enable
Exception mode                    = Enable
Multicast/broadcast response mode = Enable
Notification mode                 = Enable
.
.
.
```


