
We can set parameters inside the context of a module by using the `set PARAMETER_NAME VALUE` command.

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set rhosts 10.10.165.39
rhosts => 10.10.165.39
```

Parameters we will often use are:

- **RHOSTS**: “Remote host”, the *IP address of the target system*. A *single IP address or a network range* can be set. This will support the CIDR (Classless Inter-Domain Routing) notation (/24, /16, etc.) or a network range (10.10.10.x – 10.10.10.y). You can also use a file where targets are listed, one target per line using `file:/path/of/the/target_file.txt`
  
- **RPORT**: “Remote port”, the port on the target system the vulnerable application is running on
  
- **PAYLOAD**: The payload you will use with the exploit
  
- **LHOST**: “Localhost”, the attacking machine (your AttackBox or Kali Linux) IP address
  
- **LPORT**: “Local port”, the port you will use for the reverse shell to connect back to. This is a port on your attacking machine, and you can set it to any port not used by any other application
  
- **SESSION**: Each connection established to the target system using *Metasploit will have a session ID*. You will use this with post-exploitation modules that will connect to the target system using an existing connection.

We can *override* any set parameter using the *set command again* with a different value. You can also *clear* any parameter value using the `unset` command or *clear all* set parameters with the `unset all` command.

We can use the `setg` command to set values that will be used for *all modules*. The `setg` command is used like the set command. The `setg` command allows you to set the value so it can be used by default across different modules. You can clear any value set with `setg` using `unsetg`.
