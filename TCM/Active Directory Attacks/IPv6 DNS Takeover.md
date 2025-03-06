
Most networks are configured to work with IPv4 but have IPv6 turned on. Hence, there are situations where **no one is performing DNS resolution for IPv6** requests.

We can setup an attacker machine to masquerade as a DNS for IPv6 and get authentication to the domain controller via *LDAP* or *SMB*.

When a machine reboots, it triggers an event which will be relayed to us. We can use that to login to the domain controller and get a lot of information from the DC.

In addition if a user logs in, we can perform a **LDAP relay** attack with the NTLM credentials to log in. If it was an administrator, we can create new accounts as well.

### Setup ntlmrelayx

We are going to setup ntlmrelayx similar to what we did in the [[SMB Relay#^setupNtlmrelayx|SMB Relay attack]]. 