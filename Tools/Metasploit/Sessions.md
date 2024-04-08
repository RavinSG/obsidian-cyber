
The `sessions` command will list all active sessions. The `sessions` command supports a number of options that will help you manage sessions better.

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > sessions 

Active sessions
===============

  Id  Name  Type                     Information                   Connection
  --  ----  ----                     -----------                   ----------
  1         meterpreter x64/windows  NT AUTHORITY\SYSTEM @ JON-PC  10.4.72.115:4444 -> 10.10.187.212:49172 (10.10.187.212)

msf6 exploit(windows/smb/ms17_010_eternalblue) >
```

We can interact with any existing session using the `sessions -i` command followed by the session ID.

