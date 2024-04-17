
Samba is the standard *Windows interoperability suite of programs for Linux and Unix.* It allows end users to access and use files, printers and other commonly shared resources on a companies intranet or internet. Its often referred to as a network file system.

Samba is based on the common client/server protocol of Server Message Block ([[SMB]]). SMB is developed only for Windows, without Samba, other computer platforms would be isolated from Windows machines, even if they were part of the same network.

Using nmap we can enumerate a machine for SMB shares.

```bash
$ nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse <IP>
```

SMB has two ports, 445 and 139.

- **Port 139**: SMB originally ran on top of NetBIOS using port 139. NetBIOS is an older transport layer that allows Windows computers to talk to each other on the same network
  
- **Port 445**: Later versions of SMB (after Windows 2000) began to use port 445 on top of a TCP stack. Using TCP allows SMB to work over the internet.

On most distributions of Linux smbclient is already installed. Lets inspect one of the shares.

```bash
$ smbclient //<IP>/anonymous
```

We can recursively download the SMB share too.

```bash
$ smbget -R smb://<IP>/anonymous
```
