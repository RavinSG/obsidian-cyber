
Most networks are configured to work with IPv4 but have IPv6 turned on. Hence, there are situations where **no one is performing DNS resolution for IPv6** requests.

We can setup an attacker machine to masquerade as a DNS for IPv6 and get authentication to the domain controller via *LDAP* or *SMB*.

When a machine reboots, it triggers an event which will be relayed to us. We can use that to login to the domain controller and get a lot of information from the DC.

In addition if a user logs in, we can perform a **LDAP relay** attack with the NTLM credentials to log in. If it was an administrator, we can create new accounts as well.

### Setup ntlmrelayx

We are going to setup ntlmrelayx similar to what we did in the [[SMB Relay#^setupNtlmrelayx|SMB Relay attack]]. 

```bash
$ impacket-ntlmrelayx -6 -t ldaps://192.168.23.130 -wh fakewpad.marvel.local -l lootme/marvel
```

`-6`: We are listening for both IPv6 and IPv4
`-t`: Target to relay the credentials to, can be an IP, hostname or URL
`-wh`: Enable serving a WPAD file for Proxy Authentication attack, setting the proxy host to the one supplied
`-l`: Loot directory in which gathered loot such as SAM dumps will be stored (default: current directory).

We are going to target our domain controller in this attack which has the ip address `192.168.23.130`

### Initiating mitm6

We will then start the Man in the Middle attack with `mitm6` to intercept and send spoofed replies to IPv6 queries.

```bash
$ sudo mitm6 -d marvel.local
```

As soon as an event occurs in a network such as rebooting a machine or a person logging in to a device, mitm6 will allow us to take that and relay it to the domain controller with ntlmrelayx.

>[!warning] Do not set up and walk away
>Only run this in small sprints (5-10mins), or this will start causing issues in the network and might kick yourself out without being able to authenticate back into the compromised machine.

### Loot Folder

Once ntlmrelayx successfully authenticates as an user, it will then enumerate the relayed user's privileges and dump the domain info into the provided loot directory. This is done by a tool called `ldap_domain_dump`.

![[Loot Directory.png]]

If we look into the `domain_computers.html` file, we can see which OS each machine in the domain is running. If there are any older vulnerable OS available, we can target them 