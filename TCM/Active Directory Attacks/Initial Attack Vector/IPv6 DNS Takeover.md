
Most networks are configured to work with IPv4 but have IPv6 turned on. Hence, there are situations where **no one is performing DNS resolution for IPv6** requests.

We can setup an attacker machine to masquerade as a DNS for IPv6 and get authentication to the domain controller via *LDAP* or *SMB*.

When a machine reboots, it triggers an event which will be relayed to us. We can use that to login to the domain controller and get a lot of information from the DC.

In addition if a user logs in, we can perform a **LDAP relay** attack with the NTLM credentials to log in. If it was an administrator, we can create new accounts as well.

### Setup ntlmrelayx

We are going to setup ntlmrelayx similar to what we did in the [[SMB Relay#^setupNtlmrelayx|SMB Relay attack]]. 

```bash
$ ntlmrelayx.py -6 -t ldaps://192.168.23.130 -wh fakewpad.marvel.local -l lootme/marvel
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

mitm6 will start assigning IPv6 addresses to machines in the network as below.

![[IPv6 Address Assign.png]]

As soon as an event occurs in a network such as rebooting a machine or a person logging in to a device, mitm6 will allow us to take that and relay it to the domain controller with ntlmrelayx. 

The following image shows a successful authentication attack that takes place when the `MARVEL\PUNISHER` machine reboots. Even without a user entering a password, we are able to gain access to the domain controller and dump the available information to the loot directory.

![[ntlmrelayx Authentication Success.png]]

>[!warning] Do not set up and walk away
>Only run this in small sprints (5-10mins), or this will start causing issues in the network and might kick yourself out without being able to authenticate back into the compromised machine.

### Loot Folder

Once ntlmrelayx successfully authenticates as an user, it will then enumerate the relayed user's privileges and dump the domain info into the provided loot directory. This is done by a tool called `ldap_domain_dump`.

![[Loot Directory.png]]

If we look into the `domain_computers.html` file, we can see which OS each machine in the domain is running. If there are **any older vulnerable OS available**, we can target them.

![[Loot Dir Domain Computers.png]]

One of the other important files to look at is the `domain_users_by_group.html` file. We can gain information such as who are *Administrators*, *Domain Admins*, and *Enterprise Admins*.

We can further get to know when were these *accounts created*, when were the *last logins* to the accounts, and *flags*. If we see an account with no logins since created, it might be a **honeypot** account.

Furthermore, there can be instances where passwords are stored in the description due to user error or laziness, as shown below.

![[Loot Dir Domain Users by Group.png]]

### Administrator Login

If an administrator logs in while the attack is running, this will be captured by ntlmrelayx as follow.

![[mitm6 Admin Login.png]]

Once this login in event is captures, ntlmrelayx is going to create an Access Control List for us and create a new user. In this case, we have successfully created a user named `FjtVrSYkpZ` with the password `+>Xp-byamJTtpp;` for us to log in as. We can now try to do a [[DCSync Attack]] with `secretsdump.py` as suggested as well.

We can verify the user creation by logging into the server and checking the available users. 

![[New User Permissions.png]]

From the above screenshot we can see that a new user has been created. However, they are not a Domain Admin but a **Domain User**. Hence, they do not have access to all machines in the domain. 

However, they have access to the **Enterprise Admins** group and they are going to have the necessary access needed to run `secretsdump` and dump all the secrets of the domain and compromise the domain to completely own the domain.

### Defences

IPv6 poisoning abuses the fact that Windows *queries for an IPv6 address even in IPv4-only environments*. If you do not use IPv6 internally, the safest way to prevent mitm6 is to **block DHCPv6 traffic and incoming router advertisements** in Windows Firewall via Group Policy. 

1. *Disabling IPv6 entirely may have unwanted side effects*. Setting the following predefined rules to Block instead of Allow prevents the attack from working:
	- (Inbound) Core Networking - Dynamic Host Configuration Protocol for IPv6(DHCPV6-In)
	- (Inbound) Core Networking - Router Advertisement (ICMPv6-In)
	- (Outbound) Core Networking - Dynamic Host Configuration Protocol for IPv6(DHCPV6- Out)

2. If *WPAD is not in use internally*, **disable** it via Group Policy and by **disabling the WinHttpAutoProxySvc service**.
   
3. Relaying to LDAP and LDAPS can only be mitigated by *enabling both LDAP signing and LDAP channel binding*.
   
4. Consider *Administrative users to the Protected Users group* or marking them as *Account is sensitive and cannot be delegated*, which will prevent any impersonation of that user via delegation.