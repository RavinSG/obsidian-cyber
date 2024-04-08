
Modules are small components within the Metasploit framework that are built to perform a specific task, such as exploiting a vulnerability, scanning a target, or performing a brute-force attack.

### [[Module Types]]
### Working with Modules

Once we have entered the [[msfconsole#Context|context]] of a module using the use command followed by the module name, as seen earlier, we will need to set [[Parameters]]. The most common parameters we will use are listed below. It is good practice to use the show options command to list the required parameters.

![[Module Parameters.png]]

Once all module parameters are set, we can launch the module using the `exploit` command. Metasploit also supports the `run` command, which is an alias created for the exploit command as the word exploit did not make sense when using modules that were not exploits (port scanners, vulnerability scanners, etc.).

The `exploit` command can be used without any parameters or using the “`-z`” parameter.
The `exploit -z` command will run the exploit and *background the session* as soon as it opens.

![[Run module.png]]

This will return you the context prompt from which you have run the exploit.
Some modules support the `check` option. This will check if the target system is vulnerable without exploiting it.

### Sessions

Once a vulnerability has been successfully exploited, *a session will be created*. This is the communication channel established between the target system and Metasploit.

We can use the `background` command to background the session prompt and go back to the msfconsole prompt.

![[Background.png]]

Alternatively, `CTRL+Z` can be used to background sessions.
The `sessions` command can be used from the msfconsole prompt or any context to see the existing sessions.

![[View Sessions.png]]

To interact with any session, we can use the `sessions -i` command followed by the desired session number.

![[Session Interaction.png]]

