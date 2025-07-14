
Once we have a meterpreter connection, we can simply use the `getsystem` command to elevate our privileges. 

```bash
meterpreter > getsystem -h
Usage: getsystem [options]

Attempt to elevate your privilege to that of local system.

OPTIONS:

    -h   Help Banner.
    -t   The technique to use. (Default to '0').
                0 : All techniques available
                1 : Named Pipe Impersonation (In Memory/Admin)
                2 : Named Pipe Impersonation (Dropper/Admin)
                3 : Token Duplication (In Memory/Admin)
                4 : Named Pipe Impersonation (RPCSS variant)
                5 : Named Pipe Impersonation (PrintSpooler variant)
                6 : Named Pipe Impersonation (EFSRPC variant - AKA EfsPotato)

meterpreter > getsystem 
...got system via technique 5 (Named Pipe Impersonation (PrintSpooler variant)).
meterpreter > getuid 
Server username: NT AUTHORITY\SYSTEM
```

## Technique 1: Named Pipe Impersonation 

- Meterpreter creates a named pipe server.
- It waits for a SYSTEM service to connect to that pipe.
- When the SYSTEM process authenticates, Meterpreter impersonates its security token.
- The context of the service is SYSTEM, so when you impersonate it, you become SYSTEM.
- **Limitation**: Modern Windows locks down pipe access. Often fails on Win10+.

## Technique 2: Named Pipe Impersonation (.DLL)

- Similar to technique 1, creates a named pipe and impersonates the security context of the first client to connect to it.
- To create a client with the SYSTEM user context, this technique drops a DLL to disk(!) and schedules `rundll32.exe` as a service to run the DLL as `SYSTEM`.
- **Limitation**: Since this drops a file to disk, can be easily detected by modern AV!

## Technique 3: Token Duplication

- Need to have `SeDebugPrivileges` privileges
- Loops through all open services to find one that is running as SYSTEM and with permission to inject into
- Uses reflective DLL injection to run `elevator.dll` in the memory space of the found service
- `elevator.dll` gets the SYSTEM token, opens the primary thread in Meterpreter, and tries to apply the SYSTEM token to it
- **Limitation**: This technique’s implementation limits itself to x86 environments only.

## Technique 4: Named Pipe Impersonation (RPCSS)

- Abuses DCOM/RPCSS to make a SYSTEM-level service authenticate to the pipe
- This is basically the idea behind JuicyPotato, RoguePotato, SweetPotato; they all target DCOM/COM activation requests.
- Need to have `SeImpersonatePrivilege`.

## Technique 5: Named Pipe Impersonation (PrintSpooler)

- Abuses the Spooler service to force it to connect back to the pipe.
- This is exactly the PrintSpoofer tool: it tricks the Spooler to authenticate to the malicious pipe.
- Works well on Win10, Server 2016/2019 if Spooler is running.

## Technique 6: Named Pipe Impersonation (EfsPotato)

- Abuses the EFSRPC API to coerce a SYSTEM-level machine account to authenticate to the pipe.
- Known as **PetitPotam** (2021).
- Often combined with NTLM relay for AD attacks, but local-only variant works if conditions are right.
- Need to have `SeImpersonatePrivilege`.